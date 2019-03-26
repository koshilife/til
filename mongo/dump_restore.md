# mongodb のダンプ、リストア

### Dump

- 全DB
```
$ mongodump -o <OUTPUT_DIR>
```

- コレクション指定
```
$ mongodump -d <DATABASE_NAME> -c <COLLECTION_NAME> --out <OUTPUT_DIR>
```

- 必要に応じて圧縮＆解凍
```
# 圧縮
$ tar -zcvf xxx.tar.gz <DIR>

# 解凍
$ tar -zxvf xxx.tar.gz
```

- フルダンプの例
```
$ mongodump -o dump_`date +"%Y%m%d_%H%M%S"`
```

### Restore

- Dumpファイルからリストア
```
$ mongorestore --drop <DUMP_DIR>
```

参考: https://blog.shimar.me/2017/03/03/mongodb-dump-restore.html
