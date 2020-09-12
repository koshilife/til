# Cybozu APIについて

Document
https://developer.cybozu.io/hc/ja

## OAuth認証以外

https://developer.cybozu.io/hc/ja/articles/201941754

ユーザ認証方式はいくつかサポートされている

- パスワード認証

```
// ログイン名が「Administrator」、パスワードが「cybozu」の場合
X-Cybozu-Authorization:QWRtaW5pc3RyYXRvcjpjeWJvenU=
```

- APIトークン認証

```
// APIトークンが「BuBNIwbRRaUvr33nWXcfUZ5VhaFsJxN0xH4NPN92」の場合
X-Cybozu-API-Token:BuBNIwbRRaUvr33nWXcfUZ5VhaFsJxN0xH4NPN92

// API トークンを複数指定（カンマ区切り）する
// API トークンが「BuBNIwbRRaUvr33nWXcfUZ5VhaFsJxN0xH4NPN92」「YuLjjdiOECJjV5ZDbFwh5BZoJJGDx3LtdCE1Dl7E」の場合
X-Cybozu-API-Token:BuBNIwbRRaUvr33nWXcfUZ5VhaFsJxN0xH4NPN92, YuLjjdiOECJjV5ZDbFwh5BZoJJGDx3LtdCE1Dl7E
```

- セッション認証

> セッション認証とは、Webサーバから付与されたセッションIDをCookieに保存する事でユーザを識別する認証方法です。

## OAuth認証

Document https://developer.cybozu.io/hc/ja/articles/360015955171

アクセストークン取得後のアクセス方法

```.sh
$ curl -H "Authorization: Bearer {your_access_token}" "https://{subdomain}.cybozu.com/k/v1/apps.json"
```

