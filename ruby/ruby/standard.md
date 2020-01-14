
### Thread::Mutex


2019/11/12 ActionCable のコードを読んでて遭遇した。

https://docs.ruby-lang.org/ja/2.6.0/class/Thread=3a=3aMutex.html

```
Mutex(Mutal Exclusion = 相互排他ロック)は共有データを並行アクセスから保護するためにあります。

m.synchronize {
  # m によって保護されたクリティカルセクション
}
```

### Monitor

2019/11/12 ActionCable のコードを読んでて遭遇した。

https://ruby-doc.org/stdlib-2.6.3/libdoc/monitor/rdoc/Monitor.html

```
Use the Monitor class when you want to have a lock object for blocks with mutual exclusion.
```

>http://ganmacs.hatenablog.com/entry/2017/04/02/182430

```
 Ruby の Monitor は何度も lock 出来る Mutex とドキュメントに書いてある。
 特になんの意味もない例だけど以下のようにロック中にさらにロックできる
 ```
