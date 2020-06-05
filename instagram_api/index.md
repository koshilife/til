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

他の人が投稿したMedia、コメント情報の取得はできないようだ。

他の人が投稿したMediaに対するコメント一覧の取得を試みたときのエラーメッセージ
```.json
{
  "error": {
    "message": "Unsupported get request. Object with ID 'xxx' does not exist, cannot be loaded due to missing permissions, or does not support this operation. Please read the Graph API documentation at https://developers.facebook.com/docs/graph-api",
    "type": "GraphMethodException",
    "code": 100,
    "error_subcode": 33,
    "fbtrace_id": "xxx"
  }
}
```

# IG Comment

https://developers.facebook.com/docs/instagram-api/reference/comment

利用できるパラメーター

- hidden 非表示化の有無
- id コメントID
- like_count コメントに対するいいね数
- media 返信対象のMedia ID
- replies コメントに対する返信一覧
- text コメント本文
- timestamp 投稿日時
- user 本人の投稿の場合のみ、UserIDを返却する
- username スクリーン名

例:
```
{
  "data": [
    {
      "user": {
        "id": "<USER_ID>"
      },
      "username": "<USER_NAME>",
      "text": "たくさんのご応募ありがとうございました",
      "id": "<COMMENT_ID>"
    },
    {
      "hidden": false,
      "id": "<COMMENT_ID>",
      "like_count": 0,
      "media": {
        "id": "<MEDIA_ID>"
      },
      "replies": {
        "data": [
          {
            "timestamp": "2020-06-04T23:35:54+0000",
            "text": "@<USER-NAME> ストーリーしました！",
            "id": "<REPLY_ID>"
          }
        ]
      },
      "text": "めちゃくちゃ可愛いデザインですね素敵！",
      "timestamp": "2020-06-04T23:35:37+0000",
      "username": "<USER_NAME>"
    }
  ],
  "paging": {
    "cursors": {
      "after": "xxxxxxxxxxxxxxxxxxxxxxxxxxx"
    },
    "next": "https://graph.facebook.com/v7.0/MEDIA_ID/comments?access_token=xxxxxxxxx&pretty=0&fields=hidden%2Cid%2Clike_count%2Cmedia%2Creplies%2Ctext%2Ctimestamp%2Cuser%2Cusername&limit=25&after=xxxxxx"
  }
}
```



