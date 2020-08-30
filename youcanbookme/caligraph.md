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


## Authentication


```
例を書く
```
