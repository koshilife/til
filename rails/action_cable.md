# ActionCable

2019/11 調査目的:

Redis <=> ActionCable サーバ間で 意図しない unsubscribe イベントが発生して
発生のメカニズムと対策を調査するために Rails のコードを読むことに。
メモをまとめる。

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


