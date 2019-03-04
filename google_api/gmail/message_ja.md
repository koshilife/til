# Class: Google::Apis::GmailV1::GmailService に関する学び


### メール取得

- 2019/03/04
  - Google APIs Explorer を使うと実際のAPIをその場で叩けて便利
  - 試したAPI [Users.messages: list](https://developers.google.com/gmail/api/v1/reference/users/messages/list)
- 2019/02/28 本文の取得は2段階、message_id を取得した後、message_id を指定して別APIで取得する。
  - [RubyでGmail本文を取得する 1](http://shidetake.com/gmail_api_1/)
  - [Google API Ruby Quickstart](https://developers.google.com/gmail/api/quickstart/ruby)
