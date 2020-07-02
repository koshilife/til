# GitHub Actions

2020年6末 [timetree-api-ruby-client](https://github.com/koshilife/timetree-api-ruby-client) にGitHub Actionsを導入した。
その際の備忘録。

- [GitHub Actionsのドキュメント](https://docs.github.com/ja/actions/)

GitHubActionsはGitHubへのプッシュ、プルリクなどの操作トリガー、cronのような定期スケジュールで、特定のプログラムを実行できるCI/CDサービスという理解。
上のレポジトリでは、以下２つのワークフローを定義した。

- プルリクエスト作成をトリガーにテストを実行する [ref](https://github.com/koshilife/timetree-api-ruby-client/blob/master/.github/workflows/test.yml)
- masterへのプッシュをトリガーにRubyGemsの該当パッケージを更新する [ref](https://github.com/koshilife/timetree-api-ruby-client/blob/master/.github/workflows/gem-push.yml)

最初にActionsタブを遷移後に提案されたRuby関連Actionsの内容を少しだけカスマイズしただけで導入できた。
- https://github.com/actions/starter-workflows/blob/be562a72393b0ddb90e69cd3cd6b3ff79aaa8315/ci/ruby.yml
- https://github.com/actions/starter-workflows/blob/be562a72393b0ddb90e69cd3cd6b3ff79aaa8315/ci/gem-push.yml

## カスタマイズ内容メモ

API KEYなど外部に公開できない情報は、Secretという環境変数をセットすることができる。
今回はCodeCoverageのバッジ表示のために利用する CodeCov, RubyGems の２つのトークンを記録している。

コードカバレッジの結果をどこかに保持する必要があるが、GitHub ActionsではCI実行結果は90日という制約があったので、
簡単に連携できる [CodeCov](https://codecov.io/) を利用した。
