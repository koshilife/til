# Microsoft Api Push Notification について

カレンダーデータの変更通知について調べた
マスターデータであるMicrosoft側のデータとサービス側のカレンダーデータのデータ同期の仕方について調べた。


変更通知を受け取るには、2020年2月時点での推奨は Microsoft Graph であるが、Outlook v2 でもサポートしている。

- [Graph](https://docs.microsoft.com/en-us/graph/api/resources/webhooks?view=graph-rest-1.0)
- [Outlook v2](https://docs.microsoft.com/en-us/previous-versions/office/office-365-api/api/version-2.0/notify-rest-operations)

調べたところ、Graph APIでは、変更通知にID以外の情報を付与する機能であるRich Notificationには対応していないようだ。

https://stackoverflow.com/questions/54221923/unable-to-subscribe-to-rich-notifications-in-ms-graph
https://stackoverflow.com/questions/53649141/rich-notifications-microsoft-graph

また、監視対象のカレンダーリソース `me/events` は自身のデフォルトカレンダーのみであるため、
デフォルト以外のサブカレンダーや他人から共有されているカレンダーの変更通知を自身のOAuth権限では受け取ることができなかった。
そのため、自身の所有しているカレンダー全てに対して変更通知を受け取ることはできない。

自身のカレンダーのみであれば、上記変更通知APIに合わせて、delta 関数を用いることで、
変更通知を受けて、表示すべき期間のイベント情報の差分のみ取得することが可能になる。

https://docs.microsoft.com/en-us/graph/api/event-delta?view=graph-rest-1.0&tabs=http
https://docs.microsoft.com/en-us/graph/delta-query-events?tabs=http

https://qiita.com/kenakamu/items/63a8d5d136223ad1939a

# graph-explorer が便利

API仕様の確認ができるサイトが重宝する
https://developer.microsoft.com/en-us/graph/graph-explorer
