# ログ周りのメモ

## 雑にメモ：formatter

ログにリクエストIDを付与する

```
I, [2019-12-23T10:50:40.354242 #56754]  INFO -- : [65f02305-5f28-49de-a753-d2e26aac2f26] Started GET "/" for 127.0.0.1 at 2019-12-23 10:50:40 +0900
F, [2019-12-23T10:50:40.357625 #56754] FATAL -- : [65f02305-5f28-49de-a753-d2e26aac2f26]   
[65f02305-5f28-49de-a753-d2e26aac2f26] ActionController::RoutingError (No route matches [GET] "/"):
[65f02305-5f28-49de-a753-d2e26aac2f26]   
[65f02305-5f28-49de-a753-d2e26aac2f26] actionpack (6.0.2) lib/action_dispatch/middleware/debug_exceptions.rb:36:in `call'
[65f02305-5f28-49de-a753-d2e26aac2f26] actionpack (6.0.2) lib/action_dispatch/middleware/show_exceptions.rb:33:in `call'
[65f02305-5f28-49de-a753-d2e26aac2f26] railties (6.0.2) lib/rails/rack/logger.rb:38:in `call_app'
[65f02305-5f28-49de-a753-d2e26aac2f26] railties (6.0.2) lib/rails/rack/logger.rb:26:in `block in call'
```

Railsのlogger周りのコードリーディング
https://blog.freedom-man.com/rails-logger-codereading

class Logger
https://docs.ruby-lang.org/ja/latest/class/Logger.html

調査しやすくするためログにリクエストIDを入れましょう
https://tech.unifa-e.com/entry/2017/01/31/192820

【Ruby on Rails】ログ出力とログローテートの設定まとめ【Logger】
https://techblog.kyamanak.com/entry/2017/12/25/151921

Rails6 のちょい足しな新機能を試す28（ActiveSupport::TaggedLogging編）
https://qiita.com/suketa/items/dfff84951cc251df834e

Nginxでリクエスト毎に発番して、Railsのログに書き込むまで
https://webuilder240.hatenablog.com/entry/2018/07/30/
