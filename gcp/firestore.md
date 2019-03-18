# Cloud Firestoreについて

## [CloudFirestore](https://firebase.google.com/docs/firestore/?hl=ja)

- 2019/03/18
  - インターネット上のAPIサービスでデータを取得し、教師データとして利用するためにラベル付けの依頼をアウトソースしやすいスプレッドシート形式でアウトプットする案件で用いるデータストレージサービスを選定したい。
  - 使いやすく、安価で、デファクト的なサービスのサービスがどれなのか、検討している
  - Bigtable, Datastore, Firestore, Storage, SQL, Spanner, 等ストレージサービスがあるが、一時的に保存するだけなので注目されているFirestoreを使ってみるで良いと思う。
  - ただし、NoSQLなので横断的な検索とかは弱い部分がストレージ要件として適切なのかは見極めないといけない。
  - [クラウド ストレージ プロダクト](https://cloud.google.com/products/storage/) ここを読むのがよさげ。
  
- Cloud Firestore参考
  - [Cloud Firestoreは進化したFirebase Realtime Database](https://qiita.com/1amageek/items/8179aebe871beb230194)
  - [Firebase RTDB + GCP datastore = Firestoreについて第一印象](https://qiita.com/Yatima/items/54ea22d0cea1acc6cbcb)
