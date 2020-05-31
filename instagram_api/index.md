# Instagram API について

2020/06/01 更新
https://developers.facebook.com/docs/instagram

インスタグラムのアカウントは一般アカウントとProアカウントに分類される。
APIも２種類

- Instagram グラフAPI Proアカウント用
- Instagram 基本表示 API 一般アカウント用

所感：情報アクセス権限の設計的に、個人のプライバシーを考慮した設計になっており、
他アカウントから一般アカウントの投稿情報やフォロー、フォロワーのネットワーク情報にはアクセスできない。
ただしビジネス用途で、業務を効率化したいニーズに答えるために、Instagram グラフAPIが提供されているとざっくり理解。


## [Instagram 基本表示 API](https://developers.facebook.com/docs/instagram-basic-display-api)

- OAuth認証した本人のプロフィール、画像、動画、アルバム情報へアクセス可能
- `GET /{user-id}` でOAuth認証したアカウント以外のユーザ情報が取れるかは不明。

## [InstagramグラフAPI](https://developers.facebook.com/docs/instagram-api)

- メディアの取得
- 他Proアカウントの情報取得
- コメント取得、作成、編集、削除
- メディア、プロフィールへのインサイトデータの取得
- ハッシュタグの検索
- Proアカウントのタグ付、メンションベースでのメディア/コメント検索
- WebHookの登録 (コメント、メンション、ストーリー終了時）

