# Google Maps Place APIについて

Place APIをダッシュボードで有効化し、keyを作ればGetリクエストで場所に関する情報を取得できる。

- [Place Search](https://developers.google.com/places/web-service/search?hl=ja)
  - サマリ
- [Place Details](https://developers.google.com/places/web-service/details?hl=ja)
  - 細かい情報欲しい場合はこちら。


## Place Search

endpoint
```
https://maps.googleapis.com/maps/api/place/findplacefromtext/output?
```

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
    
- Fields
  - formatted_address
  - 
  - geometry 座標

検索結果なしの場合のレスポンス
```
{
   "candidates" : [],
   "status" : "ZERO_RESULTS"
}
```
