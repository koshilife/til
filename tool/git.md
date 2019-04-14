# Git に関する覚書

## push

- 名前をつけてローカルブランチと別名のブランチ名でpushする
```
$ git push origin local_branch_name:remote_branch_name
```

## merge

- `--no-ff` コマンドをつけると fast-forwardの関係であっても、必ずマージコミットを作ります。
- `fast-forward` はコミットA,Bがどちらかの履歴が含まれている関係
>Fast-forwardとは
>2つのコミットAとBとがあるときに、コミットAにいたる歴史がコミットBにいたる歴
史にすべて含まれている場合、2つのコミットはファーストフォワードの関係にある、と
か、コミットAはコミットBにファーストフォワードする、といいます。

- 参考
  - [ブランチ切って更新してマージするまでの流れ](https://qiita.com/shuntaro_tamura/items/6c8bf792087fe5dc5103)
  - [図で分かるgit-mergeの--ff, --no-ff, --squashの違い](http://d.hatena.ne.jp/sinsoku/20111025/1319497900)
