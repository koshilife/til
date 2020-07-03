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

### キーワード検索例

キーワード: `Shibuya`

```
https://maps.googleapis.com/maps/api/place/findplacefromtext/json?inputtype=textquery&input=Shibuya&fields=photos,formatted_address,name,geometry&key=<API_KEY>
```

```
{
   "candidates" : [
      {
         "formatted_address" : "日本、東京都渋谷区",
         "geometry" : {
            "location" : {
               "lat" : 35.6619707,
               "lng" : 139.703795
            },
            "viewport" : {
               "northeast" : {
                  "lat" : 35.692129,
                  "lng" : 139.723862
               },
               "southwest" : {
                  "lat" : 35.64162,
                  "lng" : 139.6613389
               }
            }
         },
         "name" : "渋谷区",
         "photos" : [
            {
               "height" : 3024,
               "html_attributions" : [
                  "\u003ca href=\"https://maps.google.com/maps/contrib/101483591465545349363\"\u003eAlejandra Wachenfeld\u003c/a\u003e"
               ],
               "photo_reference" : "CmRaAAAAbNJXA-yA0RA74hX65zEwUUe8lcsE_QiOi_oY3Ka_h1rW_JLwBugI9FJTmtTKC6Nh_5rcmzkEGmjFakgBEyTDNaYWLfeYNBcTG-3yy5WhAucb_MMayBiJr7AN-CGPf4h1EhBS8eGJnbdo8jELtBsqNQ2sGhS04kFAVCWy00bD12WNlH-OKG2upg",
               "width" : 4032
            }
         ]
      }
   ],
   "status" : "OK"
}
```

### 電話番号で場所検索

電話番号: `+81334651111` (NHK)

```
https://maps.googleapis.com/maps/api/place/findplacefromtext/json?inputtype=phonenumber&input=%2B81334651111&fields=formatted_address,name,geometry&key=<API_KEY>
```

```.json
{
   "candidates" : [
      {
         "formatted_address" : "日本、〒150-0041 東京都渋谷区神南２丁目２−１",
         "geometry" : {
            "location" : {
               "lat" : 35.66500200000001,
               "lng" : 139.6953695
            },
            "viewport" : {
               "northeast" : {
                  "lat" : 35.66635098029151,
                  "lng" : 139.6967184802915
               },
               "southwest" : {
                  "lat" : 35.66365301970851,
                  "lng" : 139.6940205197085
               }
            }
         },
         "name" : "NHK"
      },
      {
         "formatted_address" : "日本、〒150-0041 東京都渋谷区神南２丁目２−１",
         "geometry" : {
            "location" : {
               "lat" : 35.6647596,
               "lng" : 139.6956746
            },
            "viewport" : {
               "northeast" : {
                  "lat" : 35.66610858029149,
                  "lng" : 139.6970235802915
               },
               "southwest" : {
                  "lat" : 35.6634106197085,
                  "lng" : 139.6943256197085
               }
            }
         },
         "name" : "NHK World Radio Japan"
      }
   ],
   "status" : "OK"
}
```


## Nearby Query

例えば、千代田区役所(35.694087, 139.753436)から3km以内の駅を調べるなどができる。
hl=ja

https://maps.googleapis.com/maps/api/place/nearbysearch/json?location=35.694087, 139.753436&radius=3000&type=train_station&language=ja&key=<API_KEY>

なお、利用できるtypeは以下から参照可 https://developers.google.com/places/web-service/supported_types?

実際のレスポンス
```
{
   "html_attributions" : [],
   "results" : [
      {
         "geometry" : {
            "location" : {
               "lat" : 35.68534940000001,
               "lng" : 139.7632784
            },
            "viewport" : {
               "northeast" : {
                  "lat" : 35.6913753,
                  "lng" : 139.77455305
               },
               "southwest" : {
                  "lat" : 35.67929329999999,
                  "lng" : 139.75764285
               }
            }
         },
         "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/generic_business-71.png",
         "id" : "c4f09fe1eb26384bca2636a227edb99b6aa03cd1",
         "name" : "大手町駅",
         "photos" : [
            {
               "height" : 3024,
               "html_attributions" : [
                  "\u003ca href=\"https://maps.google.com/maps/contrib/102356555223867748482\"\u003e乙名丹次郎\u003c/a\u003e"
               ],
               "photo_reference" : "CmRaAAAAU4dBB47xssSSVxDZ-MVZxSVZnCdHSZe3QYQKSZahbZerDG7A1g0auAJmTeRFViYdvBIdGGZotCCXlRDd9pyjjYpkz7wAlE_akUaDFsIv0RIU6JF0iOVY9eldJXYCH12VEhD0e84FGKIKmywdy2NgoJgPGhSmvaTXOcEB3advOJ2h9QKfQ9nW5Q",
               "width" : 4032
            }
         ],
         "place_id" : "ChIJnUcMGviLGGAROCgXeJNX4xg",
         "plus_code" : {
            "compound_code" : "MQP7+48 日本、東京都 東京",
            "global_code" : "8Q7XMQP7+48"
         },
         "rating" : 3.5,
         "reference" : "ChIJnUcMGviLGGAROCgXeJNX4xg",
         "scope" : "GOOGLE",
         "types" : [
            "transit_station",
            "train_station",
            "subway_station",
            "point_of_interest",
            "establishment"
         ],
         "user_ratings_total" : 343,
         "vicinity" : "千代田区大手町２丁目１−１"
      },
      {
         "geometry" : {
            "location" : {
               "lat" : 35.7020484,
               "lng" : 139.7535016
            },
            "viewport" : {
               "northeast" : {
                  "lat" : 35.70552985,
                  "lng" : 139.7573788
               },
               "southwest" : {
                  "lat" : 35.70056245000001,
                  "lng" : 139.7501596
               }
            }
         },
         "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/generic_business-71.png",
         "id" : "500d511811afffb6442380da58780660e832e7eb",
         "name" : "水道橋駅",
         "photos" : [
            {
               "height" : 3024,
               "html_attributions" : [
                  "\u003ca href=\"https://maps.google.com/maps/contrib/114089086785527837934\"\u003eあのもとゆき\u003c/a\u003e"
               ],
               "photo_reference" : "CmRaAAAAHVEOdNoWYRbgn23xyWSClPGeYfjQLeyhbF9cZW-grj_42wa4oJvsbGqKxTerrNDQj0yTtj5N47n6kGe9kiWis2o2nv_25F8KeL6-I7V2aYXX5bBOMDecKDlkyZzAYR7tEhA6fZYLdUQSohnWq1ZTZv9cGhQwnwxcW3NAXllGCM_EY6aR4ltiyQ",
               "width" : 4032
            }
         ],
         "place_id" : "ChIJYUI5cz-MGGARbRXLo384IWQ",
         "plus_code" : {
            "compound_code" : "PQ23+RC 日本、東京都 東京",
            "global_code" : "8Q7XPQ23+RC"
         },
         "rating" : 3.4,
         "reference" : "ChIJYUI5cz-MGGARbRXLo384IWQ",
         "scope" : "GOOGLE",
         "types" : [
            "transit_station",
            "train_station",
            "subway_station",
            "point_of_interest",
            "establishment"
         ],
         "user_ratings_total" : 925,
         "vicinity" : "千代田区神田三崎町２丁目２２−１"
      },
      {
         "geometry" : {
            "location" : {
               "lat" : 35.7020837,
               "lng" : 139.7450232
            },
            "viewport" : {
               "northeast" : {
                  "lat" : 35.70563164999999,
                  "lng" : 139.74995415
               },
               "southwest" : {
                  "lat" : 35.69861545000001,
                  "lng" : 139.73986915
               }
            }
         },
         "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/generic_business-71.png",
         "id" : "dbe311af42efd2e70241578562ce949f04aa160a",
         "name" : "飯田橋駅",
         "photos" : [
            {
               "height" : 3616,
               "html_attributions" : [
                  "\u003ca href=\"https://maps.google.com/maps/contrib/106603790792247199501\"\u003eYung-sunce Kiyah\u003c/a\u003e"
               ],
               "photo_reference" : "CmRaAAAA7VPyQ2eGGOxyiO56QYGK1pxrPcUaHdKKrVUPbOUPkv0w5vSJyiThid2pVFg-_pzMQGiHnOlNTwqq5-txubFWQGAuFSjq4FnssQwIS6DWZM2zsQFYF9F9NfRbAW6nHcF1EhCMqKixH8i8kbKaDVsLtRwIGhR19MvJklhT-er6-hAlW2hRtM7-qw",
               "width" : 5424
            }
         ],
         "place_id" : "ChIJdamjxkSMGGARJTMHewW_ink",
         "plus_code" : {
            "compound_code" : "PP2W+R2 日本、東京都 東京",
            "global_code" : "8Q7XPP2W+R2"
         },
         "rating" : 3.5,
         "reference" : "ChIJdamjxkSMGGARJTMHewW_ink",
         "scope" : "GOOGLE",
         "types" : [
            "transit_station",
            "train_station",
            "subway_station",
            "point_of_interest",
            "establishment"
         ],
         "user_ratings_total" : 403,
         "vicinity" : "千代田区飯田橋４丁目９−５"
      },
      {
         "geometry" : {
            "location" : {
               "lat" : 35.6993854,
               "lng" : 139.7652479
            },
            "viewport" : {
               "northeast" : {
                  "lat" : 35.7009181302915,
                  "lng" : 139.766176
               },
               "southwest" : {
                  "lat" : 35.6982201697085,
                  "lng" : 139.7624636
               }
            }
         },
         "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/train-71.png",
         "id" : "74d0f27e12cef124447e2bd5ca0bdd58e4f9de3b",
         "name" : "御茶ノ水駅",
         "photos" : [
            {
               "height" : 3712,
               "html_attributions" : [
                  "\u003ca href=\"https://maps.google.com/maps/contrib/105088801807714473130\"\u003eMasataka Nakashima\u003c/a\u003e"
               ],
               "photo_reference" : "CmRaAAAArZwLt44lRyii2Yps3DDbkAKwszsMfr8TB43u3m1o3k2ZDHgq9JwcouOwG-RhA-GV5Bj_4kub-Dy4taOe_-pt0H9pQyDhScVWMeFyW5b5T98K9AYZgx-P1Gwn150dEikDEhDsIn7tHJM4431ncvh6WOnQGhRFEZebD-fV1x2gpYX4f9I1utBNVg",
               "width" : 5568
            }
         ],
         "place_id" : "ChIJebf-6hmMGGARH8tTXnEen_Y",
         "plus_code" : {
            "compound_code" : "MQX8+Q3 日本、東京都 東京",
            "global_code" : "8Q7XMQX8+Q3"
         },
         "rating" : 3.6,
         "reference" : "ChIJebf-6hmMGGARH8tTXnEen_Y",
         "scope" : "GOOGLE",
         "types" : [
            "train_station",
            "transit_station",
            "point_of_interest",
            "establishment"
         ],
         "user_ratings_total" : 401,
         "vicinity" : "千代田区神田駿河台２丁目"
      },
      {
         "geometry" : {
            "location" : {
               "lat" : 35.6918216,
               "lng" : 139.7709318
            },
            "viewport" : {
               "northeast" : {
                  "lat" : 35.6974268,
                  "lng" : 139.7721908802915
               },
               "southwest" : {
                  "lat" : 35.6888472,
                  "lng" : 139.7694929197085
               }
            }
         },
         "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/generic_business-71.png",
         "id" : "0ebfa77e5271ba729cae444829c0544e6d17df49",
         "name" : "神田駅",
         "photos" : [
            {
               "height" : 1356,
               "html_attributions" : [
                  "\u003ca href=\"https://maps.google.com/maps/contrib/117352006655415748002\"\u003et7 s\u003c/a\u003e"
               ],
               "photo_reference" : "CmRaAAAA4OOfxlQV3pamZhvmH3bqwa4E_RGwZbluos33YDkSRWoxWgfvzNCXjAHofnhy8LApdpJoBzNEyz1QwesjCaK2ZS4iD5UENuKi959nE2nQ9GH3Qh5ASYmQ2rAaX8YquQoNEhA9a2SrrSCJyPqpZevkp5jUGhS7cNNKYK-avhlQ3VMU6ZJ0fieFjA",
               "width" : 2048
            }
         ],
         "place_id" : "ChIJvQ0WmgGMGGAR7GCzQU6OtF0",
         "plus_code" : {
            "compound_code" : "MQRC+P9 日本、東京都 東京",
            "global_code" : "8Q7XMQRC+P9"
         },
         "rating" : 3.7,
         "reference" : "ChIJvQ0WmgGMGGAR7GCzQU6OtF0",
         "scope" : "GOOGLE",
         "types" : [
            "transit_station",
            "train_station",
            "subway_station",
            "point_of_interest",
            "establishment"
         ],
         "user_ratings_total" : 457,
         "vicinity" : "千代田区鍛冶町２丁目"
      },
      {
         "geometry" : {
            "location" : {
               "lat" : 35.6910121,
               "lng" : 139.7355674
            },
            "viewport" : {
               "northeast" : {
                  "lat" : 35.69524275,
                  "lng" : 139.74057335
               },
               "southwest" : {
                  "lat" : 35.68954295,
                  "lng" : 139.73249795
               }
            }
         },
         "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/generic_business-71.png",
         "id" : "5db04ff4966c77b702a9637b0c517e99fcdf387f",
         "name" : "市ケ谷駅",
         "photos" : [
            {
               "height" : 1836,
               "html_attributions" : [
                  "\u003ca href=\"https://maps.google.com/maps/contrib/112289023250129654135\"\u003eSetivan\u003c/a\u003e"
               ],
               "photo_reference" : "CmRaAAAAuWfVcwHEHO1jx3yaTKfS2BW1TFP4nmrfILKhEGznraF6tA0G6QAtpgompT3gBhXc7aqOrePvzyWdNK42SWsfnfXKQ1Ta1Cg9DaW3A1p_ZuqQ45OeZXG0IwabFfcspbHtEhAh_l3204dFnkve7dHtp74RGhRNuipCSb6jaWETHyokGlr0Ikk1zA",
               "width" : 3264
            }
         ],
         "place_id" : "ChIJ1xUrqmCMGGARYExMLJwfiMc",
         "plus_code" : {
            "compound_code" : "MPRP+C6 日本、東京都 東京",
            "global_code" : "8Q7XMPRP+C6"
         },
         "rating" : 3.6,
         "reference" : "ChIJ1xUrqmCMGGARYExMLJwfiMc",
         "scope" : "GOOGLE",
         "types" : [
            "transit_station",
            "train_station",
            "subway_station",
            "point_of_interest",
            "establishment"
         ],
         "user_ratings_total" : 292,
         "vicinity" : "千代田区五番町 千代田 区 九段 南 ４ 丁目 新宿 区 市谷 田町 １ 丁目"
      },
      {
         "geometry" : {
            "location" : {
               "lat" : 35.69838299999999,
               "lng" : 139.7730717
            },
            "viewport" : {
               "northeast" : {
                  "lat" : 35.69992128029149,
                  "lng" : 139.7772372
               },
               "southwest" : {
                  "lat" : 35.6972233197085,
                  "lng" : 139.7697948
               }
            }
         },
         "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/generic_business-71.png",
         "id" : "e4bf3ac1f9778f8ddc92c0605c46f7e6812d0b3d",
         "name" : "秋葉原駅",
         "photos" : [
            {
               "height" : 3024,
               "html_attributions" : [
                  "\u003ca href=\"https://maps.google.com/maps/contrib/100103596046424591155\"\u003eしもだちゃんねる\u003c/a\u003e"
               ],
               "photo_reference" : "CmRaAAAA0x2M-Bo0MC1xzSZ3iBHWIGcsEOdzEJH6Bd9t0IEbXzw1nZmaO9r0B-ANp6fCW64xVc2850nXkdwK8_JaD3koLY3vH4RV4vrCqvaCjZObakZXoXtZJR42LJhbHE3WZt4dEhAQL6gfIhk9Pz8bs0alyssrGhTZMwLj9scxmGdEBDrNrPpAX-VZLQ",
               "width" : 4032
            }
         ],
         "place_id" : "ChIJKTP54qeOGGARsZf1fyU2jxU",
         "plus_code" : {
            "compound_code" : "MQXF+96 日本、東京都 東京",
            "global_code" : "8Q7XMQXF+96"
         },
         "rating" : 3.8,
         "reference" : "ChIJKTP54qeOGGARsZf1fyU2jxU",
         "scope" : "GOOGLE",
         "types" : [
            "transit_station",
            "train_station",
            "subway_station",
            "point_of_interest",
            "establishment"
         ],
         "user_ratings_total" : 4394,
         "vicinity" : "千代田区外神田１丁目"
      },
      {
         "geometry" : {
            "location" : {
               "lat" : 35.68123620000001,
               "lng" : 139.7671248
            },
            "viewport" : {
               "northeast" : {
                  "lat" : 35.68639564999999,
                  "lng" : 139.7723041
               },
               "southwest" : {
                  "lat" : 35.67433665,
                  "lng" : 139.7596813
               }
            }
         },
         "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/generic_business-71.png",
         "id" : "d1a0bd7e4e66d58c9dbe34524ea7157b2c6b6256",
         "name" : "東京駅",
         "photos" : [
            {
               "height" : 3840,
               "html_attributions" : [
                  "\u003ca href=\"https://maps.google.com/maps/contrib/104953319675631864082\"\u003eLin BoJiun\u003c/a\u003e"
               ],
               "photo_reference" : "CmRaAAAAMc6Vwc1wtszaSdTMQdtj-1jzIAoASr-SHyC9Ofpa0cgc4q7OxOw1Gu_w6qRSRPHMmIYBlonoM6PERSx6g-X5YYINCS_ogZEJDotxDS7p9UKxtUHQz8vOtHwmZWBYUYkeEhADudus5aPJRrdWiKVXfwY4GhQ2GnNi7lxAfnuptqJo1R9ZPefuzA",
               "width" : 5120
            }
         ],
         "place_id" : "ChIJC3Cf2PuLGGAROO00ukl8JwA",
         "plus_code" : {
            "compound_code" : "MQJ8+FR 日本、東京都 東京",
            "global_code" : "8Q7XMQJ8+FR"
         },
         "rating" : 4.3,
         "reference" : "ChIJC3Cf2PuLGGAROO00ukl8JwA",
         "scope" : "GOOGLE",
         "types" : [
            "transit_station",
            "train_station",
            "subway_station",
            "point_of_interest",
            "establishment"
         ],
         "user_ratings_total" : 6591,
         "vicinity" : "千代田区丸の内１丁目"
      },
      {
         "geometry" : {
            "location" : {
               "lat" : 35.6890164,
               "lng" : 139.774265
            },
            "viewport" : {
               "northeast" : {
                  "lat" : 35.6902720302915,
                  "lng" : 139.7773753
               },
               "southwest" : {
                  "lat" : 35.6875740697085,
                  "lng" : 139.7709313
               }
            }
         },
         "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/train-71.png",
         "id" : "47940377e72a9e45eded7f3cfc3ab49c482139c3",
         "name" : "新日本橋駅",
         "photos" : [
            {
               "height" : 3024,
               "html_attributions" : [
                  "\u003ca href=\"https://maps.google.com/maps/contrib/105990351650248810127\"\u003eHisataka Goto\u003c/a\u003e"
               ],
               "photo_reference" : "CmRaAAAAOfzw7qIU1kaOu7mpmbveABAFxETRmO7qBPPT907KnxQYt6FKyWSI6EkJgQjQTt2XoiQenG3b4ROzrsWgysvHOXkj4T5b8w7nj5x-vzmLqtIqVsyWVswFpgOwEneyoTf8EhC31pgMOu29qrnIV6fdRkYkGhTL77N-pl4x73grr2Iu-_WVuIsGvg",
               "width" : 4032
            }
         ],
         "place_id" : "ChIJ5Z-EU1WJGGARum7jE_nCY3U",
         "plus_code" : {
            "compound_code" : "MQQF+JP 日本、東京都 東京",
            "global_code" : "8Q7XMQQF+JP"
         },
         "rating" : 3.3,
         "reference" : "ChIJ5Z-EU1WJGGARum7jE_nCY3U",
         "scope" : "GOOGLE",
         "types" : [
            "train_station",
            "transit_station",
            "point_of_interest",
            "establishment"
         ],
         "user_ratings_total" : 79,
         "vicinity" : "Japan"
      },
      {
         "geometry" : {
            "location" : {
               "lat" : 35.6740083,
               "lng" : 139.7511011
            },
            "viewport" : {
               "northeast" : {
                  "lat" : 35.67676080000001,
                  "lng" : 139.75587595
               },
               "southwest" : {
                  "lat" : 35.66990879999999,
                  "lng" : 139.7481465500001
               }
            }
         },
         "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/generic_business-71.png",
         "id" : "c311eb37db990c12503848896cd18386e83ad9d5",
         "name" : "霞ヶ関駅",
         "photos" : [
            {
               "height" : 2432,
               "html_attributions" : [
                  "\u003ca href=\"https://maps.google.com/maps/contrib/108839327085409911755\"\u003eYuichi Hoshino\u003c/a\u003e"
               ],
               "photo_reference" : "CmRaAAAApLYuEPY7r2rTkWP0SSbg8uIVzjCYWddUVZP_zi3NrthDRahrAulLEvmWftfI97lr6WV7OSWRdjpUydOPOPFPQIKkr5_81avVk9CvXLXA-x5UazJ3mvIyuig9vqcczpmdEhAczGfo6dRI7xHu-Gp0SUEKGhRo9Kv1EUzJ1DEPzqH-Rq-KvDH22g",
               "width" : 3648
            }
         ],
         "place_id" : "ChIJY1UVOo2LGGAR5yIdN1AWGNA",
         "plus_code" : {
            "compound_code" : "MQF2+JC 日本、東京都 東京",
            "global_code" : "8Q7XMQF2+JC"
         },
         "rating" : 3.4,
         "reference" : "ChIJY1UVOo2LGGAR5yIdN1AWGNA",
         "scope" : "GOOGLE",
         "types" : [
            "transit_station",
            "train_station",
            "subway_station",
            "point_of_interest",
            "establishment"
         ],
         "user_ratings_total" : 112,
         "vicinity" : "千代田区霞が関２丁目１−２"
      },
      {
         "geometry" : {
            "location" : {
               "lat" : 35.67491869999999,
               "lng" : 139.7628199
            },
            "viewport" : {
               "northeast" : {
                  "lat" : 35.67737395,
                  "lng" : 139.76653115
               },
               "southwest" : {
                  "lat" : 35.67277855,
                  "lng" : 139.75947095
               }
            }
         },
         "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/generic_business-71.png",
         "id" : "f188dcddfe3409c513620272658b38e33fa0f1bf",
         "name" : "有楽町駅",
         "photos" : [
            {
               "height" : 3265,
               "html_attributions" : [
                  "\u003ca href=\"https://maps.google.com/maps/contrib/107804304792480228126\"\u003eさへきひろのぶ\u003c/a\u003e"
               ],
               "photo_reference" : "CmRaAAAAY33-82JM_IMnclawUr5sPfUJbg8jW45Xnm5HtHllWfkhZHFOn8qKkUrHoEBwqzNog6bZ3gylxsqrnKLHYWThRwwAFvu9zF2DgYMJiGRYNUY3OFHDNe8bPmtrqZLVLNvVEhBceQH5qwtYuIGdSCP0VdRYGhQM42P-xTKqsY-2IgIKbbEHJeVPnw",
               "width" : 4898
            }
         ],
         "place_id" : "ChIJ1UxPReWLGGARagE2MSfXh7g",
         "plus_code" : {
            "compound_code" : "MQF7+X4 日本、東京都 東京",
            "global_code" : "8Q7XMQF7+X4"
         },
         "rating" : 3.7,
         "reference" : "ChIJ1UxPReWLGGARagE2MSfXh7g",
         "scope" : "GOOGLE",
         "types" : [
            "transit_station",
            "train_station",
            "subway_station",
            "point_of_interest",
            "establishment"
         ],
         "user_ratings_total" : 530,
         "vicinity" : "千代田区有楽町２丁目"
      },
      {
         "geometry" : {
            "location" : {
               "lat" : 35.7075185,
               "lng" : 139.7748564
            },
            "viewport" : {
               "northeast" : {
                  "lat" : 35.7086186802915,
                  "lng" : 139.7761065802915
               },
               "southwest" : {
                  "lat" : 35.7059207197085,
                  "lng" : 139.7734086197085
               }
            }
         },
         "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/train-71.png",
         "id" : "7d1cf4be1730bb0463f868c2823f93346203cb6b",
         "name" : "御徒町駅",
         "photos" : [
            {
               "height" : 3024,
               "html_attributions" : [
                  "\u003ca href=\"https://maps.google.com/maps/contrib/107431292072794184698\"\u003eArjun Sarup\u003c/a\u003e"
               ],
               "photo_reference" : "CmRaAAAAHLJ39eNcOLvWHfZaxsMNHqwr_JBXYIPx7IHOHes7BlVpCKEk4Hx21avg5d6z4D3ShBFGVtu3WEiCehmvZANHUIjjOVfh-qiEPGAsMplp4yRjJY8l-W5ooQlWvX_mLqDKEhDjYr66GVmyRM9lQxcHdbVLGhR0TibhaXaEPN1VcnVEQIErEr0gIg",
               "width" : 4032
            }
         ],
         "place_id" : "ChIJc-deL6COGGARRENyB-YX2eI",
         "plus_code" : {
            "compound_code" : "PQ5F+2W 日本、東京都 東京",
            "global_code" : "8Q7XPQ5F+2W"
         },
         "rating" : 3.7,
         "reference" : "ChIJc-deL6COGGARRENyB-YX2eI",
         "scope" : "GOOGLE",
         "types" : [
            "train_station",
            "transit_station",
            "point_of_interest",
            "establishment"
         ],
         "user_ratings_total" : 376,
         "vicinity" : "台東区上野５丁目２７"
      },
      {
         "geometry" : {
            "location" : {
               "lat" : 35.7111705,
               "lng" : 139.7738941
            },
            "viewport" : {
               "northeast" : {
                  "lat" : 35.7124614302915,
                  "lng" : 139.7750058302915
               },
               "southwest" : {
                  "lat" : 35.70976346970851,
                  "lng" : 139.7723078697085
               }
            }
         },
         "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/train-71.png",
         "id" : "5e059651983df4bd05a9ac798457bb7b71e5ecdb",
         "name" : "京成上野駅",
         "photos" : [
            {
               "height" : 2576,
               "html_attributions" : [
                  "\u003ca href=\"https://maps.google.com/maps/contrib/106889869776847980008\"\u003eLaxic Hsiao\u003c/a\u003e"
               ],
               "photo_reference" : "CmRaAAAA_QKK1j7gq3Az9wvsBXXsos-O9rLMjHawo9Q0XZWq96LU_SNolEwsuplJ9PT6FbC-4_JQbPeqtyDNkeHFY6AbU5OKLkhJ4aZ4VHy0563kC_7--ZLnhezTediTro_LFK3wEhBp09zaaPFFZzAo1FC8Q3hLGhRhnIY1uope-43gxXoEqy6RBbCOAA",
               "width" : 3864
            }
         ],
         "place_id" : "ChIJ0c5jEJ6OGGARBwiwjODYrb0",
         "plus_code" : {
            "compound_code" : "PQ6F+FH 日本、東京都 東京",
            "global_code" : "8Q7XPQ6F+FH"
         },
         "rating" : 4.1,
         "reference" : "ChIJ0c5jEJ6OGGARBwiwjODYrb0",
         "scope" : "GOOGLE",
         "types" : [
            "train_station",
            "transit_station",
            "point_of_interest",
            "establishment"
         ],
         "user_ratings_total" : 344,
         "vicinity" : "Japan"
      },
      {
         "geometry" : {
            "location" : {
               "lat" : 35.6938839,
               "lng" : 139.7831098
            },
            "viewport" : {
               "northeast" : {
                  "lat" : 35.6958355,
                  "lng" : 139.785753
               },
               "southwest" : {
                  "lat" : 35.69161589999999,
                  "lng" : 139.7800398
               }
            }
         },
         "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/train-71.png",
         "id" : "33f1f968c380d8e4fa1adf511570a12efe93b570",
         "name" : "馬喰町駅",
         "photos" : [
            {
               "height" : 2368,
               "html_attributions" : [
                  "\u003ca href=\"https://maps.google.com/maps/contrib/101164554818357823277\"\u003e郭代瑋\u003c/a\u003e"
               ],
               "photo_reference" : "CmRaAAAAeqY8xFvnP0vlDNqR5V5d0fpfJriYG2AG62gWEil7Dwjlxlw9diZ_0e_HjTipK3_21bfCOcBU6k1NAziYaC9rdbrCWwehXjqp1uq0ElQNvAGRjT0yhTpBc5ReKWIA4bdeEhDv0B75mhlJ3rIwVf5e0_QOGhS5FlmMqojiExPt9t6MA0sj6Qs6hg",
               "width" : 3568
            }
         ],
         "place_id" : "ChIJ1XqfRK2OGGARskk4jgTbxtI",
         "plus_code" : {
            "compound_code" : "MQVM+H6 日本、東京都 東京",
            "global_code" : "8Q7XMQVM+H6"
         },
         "rating" : 3.5,
         "reference" : "ChIJ1XqfRK2OGGARskk4jgTbxtI",
         "scope" : "GOOGLE",
         "types" : [
            "train_station",
            "transit_station",
            "point_of_interest",
            "establishment"
         ],
         "user_ratings_total" : 98,
         "vicinity" : "中央区日本橋馬喰町１丁目７−３"
      },
      {
         "geometry" : {
            "location" : {
               "lat" : 35.6861525,
               "lng" : 139.7302183
            },
            "viewport" : {
               "northeast" : {
                  "lat" : 35.68747038029149,
                  "lng" : 139.73191365
               },
               "southwest" : {
                  "lat" : 35.68477241970849,
                  "lng" : 139.72884665
               }
            }
         },
         "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/generic_business-71.png",
         "id" : "a032a1e14a2b0a59e35a7a13cf83ac15653c2538",
         "name" : "四ツ谷駅",
         "photos" : [
            {
               "height" : 2048,
               "html_attributions" : [
                  "\u003ca href=\"https://maps.google.com/maps/contrib/102706240140819389363\"\u003eYamagishi Kunio\u003c/a\u003e"
               ],
               "photo_reference" : "CmRaAAAAcuIVihPmYYs8WqdHN5FmLBSTtWmcwSz25MATwcAKnH4nLpxhYGUEwyT_fID7JNujpI9yDXLWFG379gASXlBZgrQnwnZDqJ6equpMTyRzNHcpTE3zNImsERh6rh4w1Vx2EhCmu1EpNE7BEd8Ef5p1oVwvGhR4PCiKtFl7y3f8w7BPoQiwwY-Oyw",
               "width" : 1536
            }
         ],
         "place_id" : "ChIJjdaPIoqMGGAR8KRCEorAAFQ",
         "plus_code" : {
            "compound_code" : "MPPJ+F3 日本、東京都 東京",
            "global_code" : "8Q7XMPPJ+F3"
         },
         "price_level" : 1,
         "rating" : 3.6,
         "reference" : "ChIJjdaPIoqMGGAR8KRCEorAAFQ",
         "scope" : "GOOGLE",
         "types" : [
            "transit_station",
            "train_station",
            "subway_station",
            "point_of_interest",
            "establishment"
         ],
         "user_ratings_total" : 457,
         "vicinity" : "Japan"
      },
      {
         "geometry" : {
            "location" : {
               "lat" : 35.70706210000001,
               "lng" : 139.7819476
            },
            "viewport" : {
               "northeast" : {
                  "lat" : 35.70848638029149,
                  "lng" : 139.78337935
               },
               "southwest" : {
                  "lat" : 35.70578841970849,
                  "lng" : 139.78024315
               }
            }
         },
         "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/generic_business-71.png",
         "id" : "9313aba103e017eddae6f8301ef54fbef0e589d8",
         "name" : "新御徒町駅",
         "photos" : [
            {
               "height" : 2448,
               "html_attributions" : [
                  "\u003ca href=\"https://maps.google.com/maps/contrib/102404423481426432387\"\u003e西武ライナー-SEIBU LINER-\u003c/a\u003e"
               ],
               "photo_reference" : "CmRaAAAAywrb-24w9B4OXDeqNHFAUX6Ff_xPY92h1PfBb_BXHnqj2r-ixOzyK0cRi6oqlscYZ12jnPPoQh_92-QVCm3FGuRZB05w1Jp2YqdOYCN5lmPZsAxmodYP7uDBEC5sMtnrEhChvXcBxBCOsY1jcqurX_7jGhQEeqMUFFd7EhAyOBgRb-VHEgNN5g",
               "width" : 3264
            }
         ],
         "place_id" : "ChIJB9q8QaOOGGARcJe_Ln1GKIs",
         "plus_code" : {
            "compound_code" : "PQ4J+RQ 日本、東京都 東京",
            "global_code" : "8Q7XPQ4J+RQ"
         },
         "rating" : 3.7,
         "reference" : "ChIJB9q8QaOOGGARcJe_Ln1GKIs",
         "scope" : "GOOGLE",
         "types" : [
            "transit_station",
            "train_station",
            "subway_station",
            "point_of_interest",
            "establishment"
         ],
         "user_ratings_total" : 90,
         "vicinity" : "Japan"
      },
      {
         "geometry" : {
            "location" : {
               "lat" : 35.69749520000001,
               "lng" : 139.7859834
            },
            "viewport" : {
               "northeast" : {
                  "lat" : 35.6992401,
                  "lng" : 139.788709
               },
               "southwest" : {
                  "lat" : 35.6959069,
                  "lng" : 139.7820822
               }
            }
         },
         "icon" : "https://maps.gstatic.com/mapfiles/place_api/icons/generic_business-71.png",
         "id" : "fb7f348c08718c40dde147fcb1a8c458d87e6569",
         "name" : "浅草橋駅",
         "photos" : [
            {
               "height" : 2260,
               "html_attributions" : [
                  "\u003ca href=\"https://maps.google.com/maps/contrib/101164554818357823277\"\u003e郭代瑋\u003c/a\u003e"
               ],
               "photo_reference" : "CmRaAAAARWCiyggtoZUQP8cBBm2AYqpoeCxC_njen-Bn0vtnMom3Kco_HawpLucurLNj6FwwGLjmHWTYeFIZ70tq4t-umkSjspkQu2r_3jrLd7XOfhCQ0AWXUqKXFUH9vFTDMgqtEhC166iM7SIo1xd2gtc66AtWGhREeoW9QvjlFgAVsHFsAMQn9iYKRg",
               "width" : 3406
            }
         ],
         "place_id" : "ChIJr-AfyrOOGGARWBazD9OfYeg",
         "plus_code" : {
            "compound_code" : "MQWP+X9 日本、東京都 東京",
            "global_code" : "8Q7XMQWP+X9"
         },
         "rating" : 3.6,
         "reference" : "ChIJr-AfyrOOGGARWBazD9OfYeg",
         "scope" : "GOOGLE",
         "types" : [
            "transit_station",
            "train_station",
            "subway_station",
            "point_of_interest",
            "establishment"
         ],
         "user_ratings_total" : 273,
         "vicinity" : "Japan"
      }
   ],
   "status" : "OK"
}
```
