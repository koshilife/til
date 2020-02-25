# Google Maps Place APIについて

Place APIをダッシュボードで有効化し、keyを作ればGetリクエストで場所に関する情報を取得できる。

- [Place Search](https://developers.google.com/places/web-service/search?hl=ja)
  - サマリ
- [Place Details](https://developers.google.com/places/web-service/details?hl=ja)
  - 細かい情報欲しい場合はこちら。

## Place Search

- Required params:
  - key: API KEY
  - input: 検索対象文字列
  - inputtype: 'textquery' or 'phonenumber'
- Optional params:
  - language: 検索結果の表示言語
  - fields: 確認したい項目
  - locationbias: 特定エリアに絞って検索できる
    - ipbias: IP address ベースで指定
    - point:lat,lng 1地点を指定
    - circular: 特定地点からの半径で指定
    - rectangular: 4地点の四角形で指定

Fieldsについては以下参照:

https://developers.google.com/places/web-service/search?hl=ja#PlaceSearchResults


検索結果なしの場合のレスポンス
```
{
   "candidates" : [],
   "status" : "ZERO_RESULTS"
}
```

## Nearby Query

例えば、千代田区役所(35.694087, 139.753436)から3km以内の駅を調べるなどができる。

https://maps.googleapis.com/maps/api/place/nearbysearch/json?location=35.694087, 139.753436&radius=3000&type=train_station&language=ja&key=<API_KEY>

利用できるtypeは以下から参照可

https://developers.google.com/places/web-service/supported_types?hl=ja
