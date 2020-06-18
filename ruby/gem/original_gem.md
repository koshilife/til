# Gemの作り方について

- 2020/01 作成Gem
  - https://github.com/koshilife/omniauth-tanita
  - https://github.com/koshilife/tanita-api-ruby-client/
- 2020/05 作成Gem
  - https://github.com/koshilife/omniauth-timetree
  - https://github.com/koshilife/omniauth-cybozu

Gemの作り方の備忘

Gem、システム側のアップデート

```sh
# gemのアップデート
gem update --system

# bundler未インストールの場合はインストール
gem install bundler

# bundlerインストール済の場合はアップデート
gem update bundler
```

雛形の作り方

```sh
# gemのひな形 Rspec付き
bundle gem test_gem -t

# gemのひな形 minitest
bundle gem test_gem --test=minitest
```

```sh
# test実行
bundle exec rspec

# リリース
bundle exec rake release
```

* .gemspec
  * 依存ライブラリはGemfileではなく、gemspecに定義しよう, パイセンのレポジトリには、開発の依存ライブラリは書いているところ、書いていないところがあった rubocop等

```ruby
  spec.add_runtime_dependency 'tanita-api-client', '~> 0.2.3'
  spec.add_runtime_dependency 'omniauth-oauth2', '~> 1.6.0'

  spec.add_development_dependency 'bundler', '~> 2.0'
  spec.add_development_dependency 'rake', '~> 12.0'
  spec.add_development_dependency 'rspec', '~> 3.0'
```

* その他
  * testフレームワークはなんでもよい。rspec or minitest 等。
  * CHANGELOG.md を書いて変更内容をわかりやすくしよう。
  * フォーマッター設定を書いておこう rubocop なら .rubocop.yml を書こう。
  * Gemfile.lock は含めない [ref](https://sanematsu.wordpress.com/2018/07/22/ignore-or-not-ignore/)
　* gem release 直後は ダウンロードできるまで、数分のタイムラグがある。

参考:
- [プライベートgemの作り方](https://qiita.com/nysalor/items/c626e893f6a0d2d3782e)
  - Gemの外からconfigureメソッドで設定値を設定できるテクニック参考になった。
- [【Ruby】gemの作り方から公開まで](https://qiita.com/9sako6/items/72994b8b1c00af4e61fe)
- [RUby Gem作り方](https://morizyun.github.io/blog/ruby-gem-easy-publish-library-rails/index.html)
- [RubyGemはめっちゃ簡単に作れる！](https://morizyun.github.io/blog/ruby-gem-easy-publish-library-rails/index.html)
