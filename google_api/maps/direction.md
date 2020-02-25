Direction APIについて

refs https://developers.google.com/maps/documentation/directions/start?hl=ja

1000回あたり5$程度で利用できるルート検索API

例: 東京駅から渋谷駅まで

https://maps.googleapis.com/maps/api/directions/json?origin=東京駅&destination=渋谷駅&language=ja&key=<API_KEY>
```
{
   "geocoded_waypoints" : [
      {
         "geocoder_status" : "OK",
         "place_id" : "ChIJC3Cf2PuLGGAROO00ukl8JwA",
         "types" : [
            "establishment",
            "point_of_interest",
            "subway_station",
            "train_station",
            "transit_station"
         ]
      },
      {
         "geocoder_status" : "OK",
         "place_id" : "ChIJnxAAO1aLGGARJqvi8d4oczM",
         "types" : [
            "establishment",
            "point_of_interest",
            "subway_station",
            "train_station",
            "transit_station"
         ]
      }
   ],
   "routes" : [
      {
         "bounds" : {
            "northeast" : {
               "lat" : 35.6794281,
               "lng" : 139.7628529
            },
            "southwest" : {
               "lat" : 35.6579522,
               "lng" : 139.7029323
            }
         },
         "copyrights" : "Map data ©2020 Google",
         "legs" : [
            {
               "distance" : {
                  "text" : "6.5 km",
                  "value" : 6477
               },
               "duration" : {
                  "text" : "15分",
                  "value" : 879
               },
               "end_address" : "日本、〒150-0002 東京都渋谷区渋谷２丁目２４ 渋谷駅",
               "end_location" : {
                  "lat" : 35.6583036,
                  "lng" : 139.7029323
               },
               "start_address" : "日本、〒100-0005 東京都千代田区丸の内１丁目９ 東京駅",
               "start_location" : {
                  "lat" : 35.6780551,
                  "lng" : 139.7628529
               },
               "steps" : [
                  {
                     "distance" : {
                        "text" : "0.5 km",
                        "value" : 493
                     },
                     "duration" : {
                        "text" : "2分",
                        "value" : 112
                     },
                     "end_location" : {
                        "lat" : 35.6794281,
                        "lng" : 139.7576794
                     },
                     "html_instructions" : "\u003cb\u003e西\u003c/b\u003e方向に\u003cb\u003e馬場先通り\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003e都道406号\u003c/b\u003eを進んで\u003cb\u003e丸の内仲通り\u003c/b\u003eに向かう\u003cdiv style=\"font-size:0.9em\"\u003eそのまま 都道406号 を進む\u003c/div\u003e",
                     "polyline" : {
                        "points" : "{jwxEyl`tY?@Gh@Ih@?@CLCTGh@Ih@CTE\\G`@AHCLE\\CLE^QtAEXMz@Gh@?BKn@EPGVA@?@Kb@CD[pAMf@y@bD"
                     },
                     "start_location" : {
                        "lat" : 35.6780551,
                        "lng" : 139.7628529
                     },
                     "travel_mode" : "DRIVING"
                  },
                  {
                     "distance" : {
                        "text" : "0.3 km",
                        "value" : 336
                     },
                     "duration" : {
                        "text" : "1分",
                        "value" : 71
                     },
                     "end_location" : {
                        "lat" : 35.676742,
                        "lng" : 139.7559767
                     },
                     "html_instructions" : "\u003cb\u003e二重橋前（交差点）\u003c/b\u003e を\u003cb\u003e左折\u003c/b\u003eして \u003cb\u003e内堀通り\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003e都道301号\u003c/b\u003e に入る",
                     "maneuver" : "turn-left",
                     "polyline" : {
                        "points" : "mswxEol_tYj@Xn@^f@Xd@VdDhBVLj@ZdCrA"
                     },
                     "start_location" : {
                        "lat" : 35.6794281,
                        "lng" : 139.7576794
                     },
                     "travel_mode" : "DRIVING"
                  },
                  {
                     "distance" : {
                        "text" : "0.5 km",
                        "value" : 508
                     },
                     "duration" : {
                        "text" : "2分",
                        "value" : 90
                     },
                     "end_location" : {
                        "lat" : 35.6771033,
                        "lng" : 139.75091
                     },
                     "html_instructions" : "\u003cb\u003e祝田橋（交差点）\u003c/b\u003e を\u003cb\u003e右折\u003c/b\u003eして \u003cb\u003e内堀通り\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003e国道1号\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003e国道20号\u003c/b\u003e に入る (\u003cb\u003e桜田門\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003e半蔵門\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003e新宿\u003c/b\u003e の表示)\u003cdiv style=\"font-size:0.9em\"\u003eそのまま 内堀通り/\u003cwbr/\u003e国道20号 を進む\u003c/div\u003e",
                     "maneuver" : "turn-right",
                     "polyline" : {
                        "points" : "sbwxE{a_tYZLKVO`@g@|AGPEJKTWv@w@`CSp@Kb@GV?HAZ?P@L@RFZDX@HLh@Pt@Pv@?@Nt@TxABTBX"
                     },
                     "start_location" : {
                        "lat" : 35.676742,
                        "lng" : 139.7559767
                     },
                     "travel_mode" : "DRIVING"
                  },
                  {
                     "distance" : {
                        "text" : "73 m",
                        "value" : 73
                     },
                     "duration" : {
                        "text" : "1分",
                        "value" : 8
                     },
                     "end_location" : {
                        "lat" : 35.6768583,
                        "lng" : 139.750188
                     },
                     "html_instructions" : "\u003cb\u003e左折\u003c/b\u003eして \u003cb\u003e六本木通り\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003e都道412号\u003c/b\u003e に向かう",
                     "maneuver" : "turn-left",
                     "polyline" : {
                        "points" : "{dwxEeb~sYJH@@@BDPJp@Lz@"
                     },
                     "start_location" : {
                        "lat" : 35.6771033,
                        "lng" : 139.75091
                     },
                     "travel_mode" : "DRIVING"
                  },
                  {
                     "distance" : {
                        "text" : "0.4 km",
                        "value" : 421
                     },
                     "duration" : {
                        "text" : "1分",
                        "value" : 80
                     },
                     "end_location" : {
                        "lat" : 35.6736884,
                        "lng" : 139.7478109
                     },
                     "html_instructions" : "\u003cb\u003e左折\u003c/b\u003eして\u003cb\u003e六本木通り\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003e都道412号\u003c/b\u003eに入る",
                     "maneuver" : "turn-left",
                     "polyline" : {
                        "points" : "kcwxEu}}sYDDDFBJFXFT@D@@@?DB`@TJFDJB@j@ZRJj@X\\NDB@BZPzAx@f@Xb@Tf@Zj@\\XP\\RXPXP"
                     },
                     "start_location" : {
                        "lat" : 35.6768583,
                        "lng" : 139.750188
                     },
                     "travel_mode" : "DRIVING"
                  },
                  {
                     "distance" : {
                        "text" : "1.0 km",
                        "value" : 1023
                     },
                     "duration" : {
                        "text" : "1分",
                        "value" : 76
                     },
                     "end_location" : {
                        "lat" : 35.6676786,
                        "lng" : 139.7396738
                     },
                     "html_instructions" : "\u003cb\u003e霞が関スマートIC（交差点）\u003c/b\u003e を\u003cb\u003e右折\u003c/b\u003eして \u003cb\u003e東名高速道路\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003e首都高速湾岸線\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003e渋谷\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003e目黒\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003e羽田\u003c/b\u003e 方面の \u003cb\u003e首都高速都心環状線\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003eC1\u003c/b\u003e に入る\u003cdiv style=\"font-size:0.9em\"\u003e有料区間\u003c/div\u003e",
                     "maneuver" : "ramp-right",
                     "polyline" : {
                        "points" : "qovxEyn}sY?H?@@@?@@@^^BB\\\\NPLLXXZZXXf@l@JJ@BDF@@JPBDDFBFBFBD@BH^FRBLHV@FPb@JdA@N@D?BBP@H?FFb@Hh@Fj@BNFTDVFL?BJTHRFHBDDHDFFHDF@BDBPTJJNN@@TVDDFHPPZ\\VXXXXZJJ`@b@`@d@\\\\Z^FDFFFFDFl@j@t@p@DDFDhAbAh@b@JJJJHHBBLHDDHHHHDDh@j@"
                     },
                     "start_location" : {
                        "lat" : 35.6736884,
                        "lng" : 139.7478109
                     },
                     "travel_mode" : "DRIVING"
                  },
                  {
                     "distance" : {
                        "text" : "0.4 km",
                        "value" : 375
                     },
                     "duration" : {
                        "text" : "1分",
                        "value" : 21
                     },
                     "end_location" : {
                        "lat" : 35.6654203,
                        "lng" : 139.7366234
                     },
                     "html_instructions" : "分岐を\u003cb\u003e右\u003c/b\u003e方向へ進み、\u003cb\u003eAH1\u003c/b\u003e を進んで \u003cb\u003e首都高速３号渋谷線\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003e渋谷\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003e首都高速中央環状線\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003e東名高速道路\u003c/b\u003e の標識に従う\u003cdiv style=\"font-size:0.9em\"\u003e有料区間\u003c/div\u003e",
                     "maneuver" : "fork-right",
                     "polyline" : {
                        "points" : "_juxE}{{sY@@?@@DXXHHHHDBBBRP@B^^JHNNBBPVRZNRFLHLd@t@b@t@NTLV\\j@V`@N^BDBDNXHRDHHP@BFF"
                     },
                     "start_location" : {
                        "lat" : 35.6676786,
                        "lng" : 139.7396738
                     },
                     "travel_mode" : "DRIVING"
                  },
                  {
                     "distance" : {
                        "text" : "1.5 km",
                        "value" : 1484
                     },
                     "duration" : {
                        "text" : "1分",
                        "value" : 76
                     },
                     "end_location" : {
                        "lat" : 35.6595315,
                        "lng" : 139.7220255
                     },
                     "html_instructions" : "\u003cb\u003e首都高速３号渋谷線\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003eルート 3\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003eAH1\u003c/b\u003eを進む\u003cdiv style=\"font-size:0.9em\"\u003e有料区間\u003c/div\u003e",
                     "polyline" : {
                        "points" : "{{txE{h{sYBD@DDFDHl@tAj@tAVl@@BBD@DLZLZ~@~Bd@hABHDFBHBHb@`ARd@FPHPBHDHPb@HTZx@N^DHHXJVFNFPBDL^`@jAZx@\\dA@BDJBHL\\L\\L^BDJXb@jATl@f@jAFPHPXr@DHL\\HTFPNh@\\pANj@XhAPp@Nj@BLBL@D|@rDBHBJLl@Pt@FVHd@DRLz@BTBT@F@F?BBV@J@JBLFp@Dd@Fz@B`@Bn@BZ"
                     },
                     "start_location" : {
                        "lat" : 35.6654203,
                        "lng" : 139.7366234
                     },
                     "travel_mode" : "DRIVING"
                  },
                  {
                     "distance" : {
                        "text" : "0.2 km",
                        "value" : 194
                     },
                     "duration" : {
                        "text" : "1分",
                        "value" : 42
                     },
                     "end_location" : {
                        "lat" : 35.6593079,
                        "lng" : 139.7199177
                     },
                     "html_instructions" : "\u003cb\u003e高樹町\u003c/b\u003e 出口を \u003cb\u003e恵比寿\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003e渋谷駅\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003e国道246号\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003e青山通り\u003c/b\u003e 方面に向かって進む\u003cdiv style=\"font-size:0.9em\"\u003e有料区間\u003c/div\u003e",
                     "maneuver" : "ramp-left",
                     "polyline" : {
                        "points" : "awsxEumxsYDF@D@B@PDr@Bl@HfDAF@D@\\@v@@B?B@BBF"
                     },
                     "start_location" : {
                        "lat" : 35.6595315,
                        "lng" : 139.7220255
                     },
                     "travel_mode" : "DRIVING"
                  },
                  {
                     "distance" : {
                        "text" : "1.4 km",
                        "value" : 1419
                     },
                     "duration" : {
                        "text" : "4分",
                        "value" : 257
                     },
                     "end_location" : {
                        "lat" : 35.6580711,
                        "lng" : 139.7043142
                     },
                     "html_instructions" : "\u003cb\u003e六本木通り\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003e都道412号\u003c/b\u003e に入る",
                     "maneuver" : "merge",
                     "polyline" : {
                        "points" : "uusxEo`xsY?L?N?\\@`ABv@Bl@Dr@Dr@HbBFvABj@HdB@XHdBH~AL~CHjBHvAFrAFxADh@@T@PBj@JzBB^Bj@Bj@Bd@?DBj@@j@@B?N@LD~@@NHrADbAHvA@d@Hp@B~A@f@B`A?n@@FDtB?P@TBzC?J?F?NBfAAB?@AB?r@?B?J"
                     },
                     "start_location" : {
                        "lat" : 35.6593079,
                        "lng" : 139.7199177
                     },
                     "travel_mode" : "DRIVING"
                  },
                  {
                     "distance" : {
                        "text" : "0.1 km",
                        "value" : 95
                     },
                     "duration" : {
                        "text" : "1分",
                        "value" : 23
                     },
                     "end_location" : {
                        "lat" : 35.6579731,
                        "lng" : 139.7032727
                     },
                     "html_instructions" : "\u003cb\u003e渋谷署前（交差点）\u003c/b\u003eを直進して、そのまま\u003cb\u003e国道246号\u003c/b\u003eへ進む",
                     "polyline" : {
                        "points" : "}msxE}~tsY?H@`@@h@B^?JBV@RD`@"
                     },
                     "start_location" : {
                        "lat" : 35.6580711,
                        "lng" : 139.7043142
                     },
                     "travel_mode" : "DRIVING"
                  },
                  {
                     "distance" : {
                        "text" : "56 m",
                        "value" : 56
                     },
                     "duration" : {
                        "text" : "1分",
                        "value" : 23
                     },
                     "end_location" : {
                        "lat" : 35.6583036,
                        "lng" : 139.7029323
                     },
                     "html_instructions" : "\u003cb\u003e渋谷駅東口（交差点）\u003c/b\u003e を\u003cb\u003e右折\u003c/b\u003eして \u003cb\u003e明治通り\u003c/b\u003e/\u003cwbr/\u003e\u003cb\u003e都道305号\u003c/b\u003e に入る (\u003cb\u003e新宿\u003c/b\u003e の表示)\u003cdiv style=\"font-size:0.9em\"\u003e目的地は前方左側です\u003c/div\u003e",
                     "maneuver" : "turn-right",
                     "polyline" : {
                        "points" : "imsxEmxtsYBXa@VEBYHCB"
                     },
                     "start_location" : {
                        "lat" : 35.6579731,
                        "lng" : 139.7032727
                     },
                     "travel_mode" : "DRIVING"
                  }
               ],
               "traffic_speed_entry" : [],
               "via_waypoint" : []
            }
         ],
         "overview_polyline" : {
            "points" : "{jwxEyl`tYsAhKm@tEY|AQl@i@xBy@bDj@XvAx@jE`CdFjCmDdK_@tAG`@Al@B`@\\hBr@dDXnBBXJHBDPbALz@DDHRRv@t@`@HL~@f@pAp@bEzBjC~Ar@b@?Jb@d@~@`AvB|BTXXf@Rp@V`APb@JdABTDf@\\hCT~@`@x@~@lAxHlInCfClD|CbAdA@BZ^\\Z`A~@x@fAfAdB~AnCn@lAh@hANVdCzFbG`OzA|DbCbHj@|AhBvEv@lB~@bDrChLf@xBP|@RnAJz@Fr@X`DFpABZDFBHFdALbFD|ADJ?\\DvCXxFbAtUr@~NV~Fd@bKHp@B~ADhBF~DDdEBvAADAfAFtBJxABXa@V_@LCB"
         },
         "summary" : "六本木通り/都道412号",
         "warnings" : [],
         "waypoint_order" : []
      }
   ],
   "status" : "OK"
}
```
