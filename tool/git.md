# Git に関する覚書

## push

- 名前をつけてローカルブランチと別名のブランチ名でpushする
```
$ git push origin local_branch_name:remote_branch_name
```

- リモートブランチの削除

```
git push origin :feature/a
```

- 参考
  - [ブランチを指定して git push する方法](http://www-creators.com/archives/5206)

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

## rebase


- 別ブランチの内容を分岐したところの頭から取り込む。commit log がキレイになるのでレビューしやすいし、後から見たときにわかりやすい。
- rebaseでやっていることは図にした以下がわかりやすかった。
  - rebase中にコンフリクトした場合は、手動でマージして続けることが可能。
  - rebaseで複数コミットをまとめることが可能。
- 複数の以前のコミットコメントを変更したい時 例 `git rebase -i HEAD~3` `git rebase -i <COMMIT_ID>`
  - https://www.granfairs.com/blog/staff/git-commit-fix
  - http://www-creators.com/archives/1116#3

- 参考
  - [git rebaseを初めて使った際のまとめ](https://qiita.com/panti310/items/e0ec74b47c6c219f2a8b)
  
 ## revert
 
- 変更を打ち消すコマンド
- Githubはマージしたものを revert する機能があり、revert 用のマジリクを自動で作れる
- revert後に 問題のあった feature/xxx ブランチは差分が出ないため、rebase等で最新状態を取り込んでから、revertしたコミットをrevertして修正をはじめる、Github では revert 用に作成したマジリクに対するrevert は作れなかった。のでコマンドで。

```
$ git revert -m 1 <REVERT COMMIT>
```
- `-m オプション` は `1` or `2` を取れる。参考 https://blog.toshimaru.net/git-revert-mainline/
 
