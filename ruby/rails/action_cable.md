# ActionCable

2019/11 調査目的:

Redis <=> ActionCable サーバ間で 意図しない unsubscribe イベントが発生して
発生のメカニズムと対策を調査するために Rails のコードを読むことに。
メモをまとめる。

=> 結論　Nginxのタイムアウトで　disconnect が発生していた。

# Rails

## Rails ActionCable

https://github.com/rails/rails/blob/master/actioncable/lib/action_cable.rb

```.rb
module ActionCable
  extend ActiveSupport::Autoload

  INTERNAL = {
    message_types: {
      welcome: "welcome",
      disconnect: "disconnect",
      ping: "ping",
      confirmation: "confirm_subscription",
      rejection: "reject_subscription"
    },
    disconnect_reasons: {
      unauthorized: "unauthorized",
      invalid_request: "invalid_request",
      server_restart: "server_restart"
    },
    default_mount_path: "/cable",
    protocols: ["actioncable-v1-json", "actioncable-unsupported"].freeze
  }

  # Singleton instance of the server
  module_function def server
    @server ||= ActionCable::Server::Base.new
  end
  
  autoload :Server
  autoload :Connection
  autoload :Channel
  autoload :RemoteConnections
  autoload :SubscriptionAdapter
  autoload :TestHelper
  autoload :TestCase
end

```

## Rails ActionCable::Server::Connections

https://github.com/rails/rails/blob/master/actioncable/lib/action_cable/server/connections.rb

3秒おきに `connections.map(&:beat) する 設定

```.rb
      BEAT_INTERVAL = 3
      
      # WebSocket connection implementations differ on when they'll mark a connection as stale. We basically never want a connection to go stale, as you
      # then can't rely on being able to communicate with the connection. To solve this, a 3 second heartbeat runs on all connections. If the beat fails, we automatically
      # disconnect.
      def setup_heartbeat_timer
        @heartbeat_timer ||= event_loop.timer(BEAT_INTERVAL) do
          event_loop.post { connections.map(&:beat) }
        end
      end
```

## rails ActionCable::Connection::Base

https://github.com/rails/rails/blob/master/actioncable/lib/action_cable/connection/base.rb


```.rb
      attr_reader :websocket
      
      def initialize
        ...
        @websocket = ActionCable::Connection::WebSocket.new(env, self, event_loop)
        ...
      end

      def beat
        transmit type: ActionCable::INTERNAL[:message_types][:ping], message: Time.now.to_i
      end
      
      def transmit(cable_message) # :nodoc:
        websocket.transmit encode(cable_message)
      end

```

## rails ActionCable::Connection::WebSocket

https://github.com/rails/rails/blob/master/actioncable/lib/action_cable/connection/web_socket.rb

```.rb
      def initialize(env, event_target, event_loop, protocols: ActionCable::INTERNAL[:protocols])
        @websocket = ::WebSocket::Driver.websocket?(env) ? ClientSocket.new(env, event_target, event_loop, protocols) : nil
      end
      

      def transmit(data)
        websocket.transmit data
      end
```      

:WebSocket::Driver は以下のGemを利用している。
https://github.com/faye/websocket-driver-ruby


## rails ActionCable::SubscriptionAdapter::Redis

https://github.com/rails/rails/blob/master/actioncable/lib/action_cable/subscription_adapter/redis.rb

- cattr_accessor redis_connector
  - デフォルトでは、 [redis/redis-rb](https://github.com/redis/redis-rb) のインスタンスを返却
  - config は :adapter, :channel_prefix 以外が引数で渡る

- @server.event_loop
  - return ActionCable::Connection::StreamEventLoop.new

- private listener
  - return Listener.new(self, @server.event_loop)
- private redis_connection_for_broadcasts
  - return self.class.redis_connector.call(@server.config.cable)

## その他

- 11月の Rails master 時点では https://github.com/faye/websocket-driver-ruby/ をラップして WebSocket通信を行っている
- 3秒おきにRailsからClientに対してWebSocketの死活監視を行い、 ping イベントを発行している。
  - Connection::Base#transmit -> ClientSocket#transmit, @driver.text
    - ClientSocket#initialize @driver.on(:open, :message, :close, :error ) にてイベントを紐付けてる。
  - TODO: ここまでしか終えていないので、もうちょっとコードを読む

参考:
https://github.com/faye/websocket-driver-ruby/blob/ee39af83d03ae3059c775583e4c4b291641257b8/examples/em_client.rb

wscatでAction Cableと通信する
https://blog.kymmt.com/entry/communicate-with-action-cable-by-wscat

# クライアント (js)

- 接続をコントロールする役割は `ConnectionMonitor` が担っており、切断時の自動再接続 `reconnectIfStale` など実装されてる
  - 3s ~ 30sec でリトライ試行数に応じて時間を開けて再接続を行っている exponential back off の考え。

https://github.com/rails/rails/blob/master/actioncable/app/javascript/action_cable/connection_monitor.js

- Websocketのメッセージプロトコルは 以下の message_types で定義されている 5種類

```action_cable.js
  var INTERNAL = {
    message_types: {
      welcome: "welcome",
      disconnect: "disconnect",
      ping: "ping",
      confirmation: "confirm_subscription",
      rejection: "reject_subscription"
    },
    disconnect_reasons: {
      unauthorized: "unauthorized",
      invalid_request: "invalid_request",
      server_restart: "server_restart"
    },
    default_mount_path: "/cable",
    protocols: [ "actioncable-v1-json", "actioncable-unsupported" ]
  };
  ```

https://github.com/rails/rails/blob/master/actioncable/app/assets/javascripts/action_cable.js

- disconnect が呼ばれた時は、
  - connection.js#close が呼ばれ、
    - 切断時刻を記録し、再接続のサイクルは `ConnectionMonitor`　に移管。
    - subscriptions.notifyAll で全購読中のsubscriptionの disconnected を呼び出す処理をする

https://github.com/rails/rails/blob/master/actioncable/app/javascript/action_cable/subscription.js
https://github.com/rails/rails/blob/master/actioncable/app/javascript/action_cable/subscriptions.js
https://github.com/rails/rails/blob/master/actioncable/app/javascript/action_cable/connection.js

