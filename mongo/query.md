# MongoDB query に関する学び

## 生のクエリを書く時

- [db.collection.find(query, projection)](https://docs.mongodb.com/manual/reference/method/db.collection.find/)
- [こんな時どうする？ MongoDBクエリ逆引きリファレンス](https://qiita.com/nishina555/items/9e20211e8d6f12fdb7b7)

## Ruby mongoid で利用する場合

- 2019/03/31 クエリ例
  - [Rails4 + Mongoidでデータ取得するあれこれ](https://doruby.jp/users/yokian/entries/Rails4___Mongoid_#object_id)
  - [Rails: mongoid を利用したwhere filter](https://qiita.com/morefun_imoto/items/0d762b9ee07bc743f4a6)
  - 正規表現で検索 `Model.where(body_html: /.*#{keyword}.*/)`
