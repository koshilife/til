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

# IG Media

```
caption* (excludes album children)
children* (carousel albums only)
comments (excludes album children, replies to comments, and the caption)
comments_count* (excludes album children and the caption, includes replies)
id*
ig_id
is_comment_enabled (excludes album children)
like_count* (excludes album children and likes on promoted posts created from the media object, includes replies)
media_type*
media_url* (not available on video IG Media objects that have been flagged for copyright violations)
owner (only returned if the IG User making the query also owns the IG Media object, otherwise the username field will be included)
permalink*
shortcode
thumbnail_url (only available on video IG Media objects)
timestamp* — ISO 8601 formatted creation date in UTC (default is UTC ±00:00)
username*
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

# Mension

https://developers.facebook.com/docs/instagram-api/guides/mentions

例:
`<IG_USER_ID>/tags?fields=caption,children,comments_count,id,like_count,media_type,media_url,permalink,timestamp,username`
`

自身に対するメンションが含まれるメディアを取得します。
IG Comment は含まれないので、WebHookを利用する方法が紹介されていた。

# Webhooks

https://developers.facebook.com/docs/instagram-api/guides/webhooks

- イベント
  - comments 自身のIG Mediaに対して新規コメント投稿時
  - mentions 自身のスクリーンネームが含まれるメンションがつけられた時
  - story_insights 自身が投稿したストーリーが期限切れした時


### mentions @comment

```.json
[
  {
    "entry": [
      {
        "changes": [
          {
            "field": "mentions",
            "value": {
              "comment_id": "17894227972186120",
              "media_id": "17918195224117851"
            }
          }
        ],
        "id": "17841405726653026",
        "time": 1520622968
      }
    ],
    "object": "instagram"
  }
]
```

コメント内容は GET /{ig-user-id}/mentioned_comment で取得することが可能。
その後コメント投稿APIにつなげれば、自動返信などの用途で利用できる


### mentions @media

```.json
[
  {
    "entry": [
      {
        "changes": [
          {
            "field": "mentions",
            "value": {
              "media_id": "17918195224117851"
            }
          }
        ],
        "id": "17841405726653026",
        "time": 1520622968
      }
    ],
    "object": "instagram"
  }
]
```

### story_insights

ストーリーが公開終了時にインサイト情報を取得できる

Webhook Body 例
```.json
[
  {
    "entry": [
      {
        "changes": [
          {
            "field": "story_insights",
            "value": {
              "media_id": "18023345989012587",
              "exits": 1,
              "replies": 0,
              "reach": 17,
              "taps_forward": 12,
              "taps_back": 0,
              "impressions": 28
            }
          }
        ],
        "id": "17841405309211844",  // Instagram Business or Creator Account ID
        "time": 1547687043
      }
    ],
    "object": "instagram"
  }
]
```

# Hashtag

https://developers.facebook.com/docs/instagram-api/reference/hashtag/top-media#returnable-fields

ハッシュタグの検索が可能、usernameを含めるなどはできないが、フィードを取得可能

例:
`<HASHTAG_ID>/top_media?user_id=<IG_USER_ID>&fields=caption,permalink`

手順は
1. IG Hashtag Searchでキーワードに対するHASHTAG_IDを検索 
2. ハッシュタグのフィードを取得
  - recent_media: Get a list of the most recently published photo and video IG Media objects published with a specific hashtag.
  - top_media: Returns the most popular photo and video IG Media objects that have been tagged with the hashtag.


