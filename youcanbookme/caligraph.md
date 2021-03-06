# YouCanBook.me API / Caligraph について　

Doc https://api.youcanbook.me/docs/index.html

予約情報、プロフィールなどのYouCanBook.meの全てのリソースが参照でき、なお更新が可能とのこと。
Google/Microsoftなどのリモート接続先のカレンダーリソースも統合して変更ができるようになるよ。

CalDAV, iCalendarにおいて、包括的な適用ができないことがあったり、REST JSON形式でのIFはサポートされていない問題があった。
YouCanBook.meのAPI / Caligraph は、主要なカレンダーサービスに対して、APIプロビジョニングの最新のベストプラクティスを使うことで
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

1. Accountに紐づくAPI KEY
  - `https://caligraph.youcanbook.me/v1/jane@example.com?fields=apiKey` で取得可
2. YouCanBook.me のパスワード
  - dashboardから設定可
3. ワンタイムトークン リクエストすれば、アカウントのメールアドレスに送信する
  - `https://caligraph.youcanbook.me/v1/jane@example.com?fields=oneTimeToken%2ConeTimeTokenExpiresAt` で取得可
4. セッショントークン
  - リセットができて有効期限がある
  - `https://caligraph.youcanbook.me/v1/jane@example.com?fields=sessionToken%2CsessionTokenExpiresAt` で取得可

推奨は有効期限があり、長めの文字列のセッショントークンを用いてほしいとのこと。


## Fields

リソースにアクセスしたときに、fieldsプロパティでデフォルト値以外のデータを取得することができる。カンマ区切りで指定。 
関連リレーションの場合は.(ドット)で連結すれば返却される。
ただし、どのフィールドが参照できるかの情報は公式Doc内には見当たらなかった。


```.json
// 'profiles', 'pro\iles.actions', 'profiles.actions.subject' の3段階の深さでfieldsを指定
// GET https://api.youcanbook.me/v1/foobar@example.com?fields=profiles%2Cprofiles.actions%2Cprofiles.actions.subject
{
  "profiles": [
    {
      "actions": [
        { "subject": "Meeting canceled by {FNAME}" },
        {
          "subject": "Your meeting with {BOOKING-PAGE-TITLE} has been rescheduled"
        },
        { "subject": "Your meeting with {BOOKING-PAGE-TITLE}" },
        {
          "subject": "Your meeting with {BOOKING-PAGE-TITLE} has been canceled"
        },
        { "subject": "New meeting scheduled with {FNAME}" },
        { "subject": "Meeting rescheduled by {FNAME}" },
        {
          "subject": "Your meeting with {BOOKING-PAGE-TITLE} is starting soon"
        },
        { "subject": "New meeting scheduled with {FNAME}" }
      ]
    },
    {
      "actions": [
        { "subject": "New meeting scheduled with {FNAME}" },
        { "subject": "Your meeting with {BOOKING-PAGE-TITLE}" },
        {
          "subject": "Your meeting with {BOOKING-PAGE-TITLE} has been canceled"
        },
        { "subject": "Meeting canceled by {FNAME}" },
        {
          "subject": "Your meeting with {BOOKING-PAGE-TITLE} has been rescheduled"
        },
        {
          "subject": "Your meeting with {BOOKING-PAGE-TITLE} is starting soon"
        },
        { "subject": "Meeting rescheduled by {FNAME}" }
      ]
    },
    {
      "actions": [
        { "subject": "Your meeting with {BOOKING-PAGE-TITLE}" },
        {
          "subject": "Your meeting with {BOOKING-PAGE-TITLE} has been canceled"
        },
        {
          "subject": "Your meeting with {BOOKING-PAGE-TITLE} has been rescheduled"
        },
        { "subject": "Meeting canceled by {FNAME}" },
        {
          "subject": "Your meeting with {BOOKING-PAGE-TITLE} is starting soon"
        },
        { "subject": "New meeting scheduled with {FNAME}" },
        { "subject": "Meeting rescheduled by {FNAME}" },
        { "subject": "New meeting scheduled with {FNAME}" }
      ]
    }
  ]
}

```


## Resource

### RemoteAccounts

連携するカレンダーサービス例えば Google Calendar, Microsoft Calendar等のアカウントを指す。
Caligraphを用いてRemoteAccountを作成することもできるが、だいたいOAuthプロトコルを使っているのでダッシュボードから
外部アカウントの連携をするのが簡単だよ。

```
https://caligraph.youcanbook.me/v1/[ACCOUNT_ID]/remoteaccounts?fields=id,type,username
```

### Calendars

RemoteAccountが提供するカレンダー情報に対するCRUD操作を可能にします。
DELETEしたら元に戻す事ができないので注意してね。

カレンダー一覧

```
https://caligraph.youcanbook.me/v1/[ACCOUNT_ID]/remoteaccounts/[REMOTE_ACCOUNT_ID]/calendars?fields=id,title,timeZone
```

### Events

カレンダーに紐づくイベント情報


### Queries

(記述なし)

### Profiles

誰かが予約できる予約ページの情報を指す。
e.g. https://example.youcanbook.me.

有料ユーザは複数の予約ページを持つことが可能。
営業時間、キャンセルルール、ロゴ、予約フォームなど多岐の情報を登録、編集可能。
サブオブジェクトのteams, services,questionや、予約イベントで発生するテンプレートイベントを制御するaction情報にもアクセス可能。

### Bookings

予約ページで予約された情報、質問への回答内容を取得できる。
予約情報の全量を取得するために、ページネーションも提供している。
検索パラメーターでフィルター機能も提供している。


# APIs


# API所感

- APIの目指すところが、連携した複数のリモートアカウントのリソース操作も含んでいるのでかなり広範囲で大きいため、やれることは多いが、複雑性が高い。
- 各エンドポイントのリクエストパラメーター、レスポンスデータの項目数が多くキャッチアップコストが高め、Webhookの設定とかはどうしたらいいんだっけ？とか答えに行き着いておらず、単純なRESTベースでシンプルなCalendlyとは異なるため、習熟にはキャッチアップ時間がかなり必要。


