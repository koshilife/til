action mailboxに関するメモ

guide
https://railsguides.jp/active_job_basics.html

ある処理をキューイングして、指定の実行サイクルや後で実行させることで
システムの信頼性向上に寄与できたり、実行タイミングの管理を楽にする仕組み。
瞬間的なアクセス増でも、キューイングだけする、ジョブ実行サーバは別マシンに任せるなどよくある負荷分散パターン。

標準で、いくつかのジョブフレームワークに対応している。
https://api.rubyonrails.org/classes/ActiveJob/QueueAdapters.html

2020年1月時点では、Sidekiq が一番利用されているっぽい。by google trends

参考
Railsで非同期処理：キュー。Sidekiq（+ActiveJob）がResqueよりも、とても簡単便利。
https://qiita.com/zaru/items/8385fdddbd1be25fe370

