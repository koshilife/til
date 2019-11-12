# ActionCable

2019/11 調査目的:

Redis <=> ActionCable サーバ間で 意図しない unsubscribe イベントが発生して
発生のメカニズムと対策を調査するために Rails のコードを読むことに。
メモをまとめる。

## Rails ActionCable::SubscriptionAdapter::Redis

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


