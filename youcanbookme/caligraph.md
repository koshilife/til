# YouCanBook.me API Caligraph について　

Doc https://api.youcanbook.me/docs/index.html

予約情報、プロフィールなどのYouCanBook.meの全てのリソースが参照でき、なお更新が可能とのこと。
Google/Microsoftなどのリモート接続先のカレンダーリソースも統合して変更ができるようになるよ。

CalDAV, iCalendarにおいて、包括的な適用ができないことがあったり、REST JSON形式でのIFはサポートされていない問題があった。
YouCanBook.meのAPI "Caligraph" は、主要なカレンダーサービスに対して、APIプロビジョニングの最新のベストプラクティスを使うことで
これらの解決するものである。

- 解決する問題:
  - the lack of comprehensive adoption.
  - there is not yet an established way to make REST style JSON requests under the CalDAV approach.

## 主要リソース

- Remote Accounts
- Calendars
- Events
- Queries
- Profiles
- Bookings

## BaseURLs

URLにUserID or Email を含めて各アカウントのリソースにアクセスする形となる。

```
https://caligraph.youcanbook.me/v1/jane@example.com
```

## Authentication

http authentication を用いた認証を使っている。
username は account email
password は以下の４種をサポートしている。

- Accountに紐づくAPI KEY (`https://caligraph.youcanbook.me/v1/jane@example.com?fields=apiKey` で取得可能)
- YouCanBook.me のパスワード (dashboardから設定可能)
- ワンタイムトークン リクエストすれば、アカウントのメールアドレスに送信する
- セッショントークン （リセットができて有効期限がある）

推奨は有効期限があり、長めの文字列のセッショントークンを用いてほしいとのこと。

## Fields

リソースにアクセスしたときに、fieldsプロパティでデフォルト値以外のデータを取得することができる。カンマ区切りで指定。


## Resource




```
例を書く
```
