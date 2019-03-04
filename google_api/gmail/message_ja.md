# Class: Google::Apis::GmailV1::GmailService に関する学び

## 全般

- 2019/03/04
  - Google APIs Explorer を使うと実際のAPIをその場で叩けて便利

### メール取得

- 2019/03/04
  - users/messages/list users/history/list の違い: history_idはあるデータ同期のタイミングのIDから変更された情報を取得するときに使う。pub/subとか。 get_profile 関数でユーザのhistory id を知ることができる。 messages は history_id とか不要で特定条件のメールを検索することができる。
    - [Users.messages: list](https://developers.google.com/gmail/api/v1/reference/users/messages/list)
    - [Users.history: list](https://developers.google.com/gmail/api/v1/reference/users/history/list)
  - Gmailクエリ [Gmail で使用できる検索演算子](https://support.google.com/mail/answer/7190?hl=ja)
    - `newer_than:2m`: 直近2ヶ月のメール取得
    - `after:2019/02/01` or `newer:2019/02/01` : 2019/02/01 以降のメール
- 2019/02/28 本文の取得は2段階、message_id を取得した後、message_id を指定して別APIで取得する。
  - [RubyでGmail本文を取得する 1](http://shidetake.com/gmail_api_1/)
  - [Google API Ruby Quickstart](https://developers.google.com/gmail/api/quickstart/ruby)
