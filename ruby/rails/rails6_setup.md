# Rails 6 x Mongoid

- v6.0.2
- Front: Vue
- DB: mongo (mongoid)

```
$ mkdir hello_rails6
$ cd hello_rails6
$ bundle init
$ vim Gemfile
rails のみコメントイン
$ bundle update
$ bundle install

$ bundle exec rails new . --skip-turbolinks --webpack=vue --skip-active-record

# 警告で推奨されたので。
$ bundle lock --add-platform x86-mingw32 x86-mswin32 x64-mingw32 java

# Gemに`mongoid`を追加
$ vim Gemfile
$ bundle install
$ rails g mongoid:config

# vue setup
./bin/rails  webpacker:install
./bin/rails  webpacker:install:typescript
./bin/rails  webpacker:install:vue
```

config/webpack/loaders/typescript.js
内で vue ファイルを読み込み可能にし、vueファイルのスクリプトタグにtsを指定 `<script lang="ts">`

```typescript.js
const PnpWebpackPlugin = require("pnp-webpack-plugin")

module.exports = {
  test: /\.tsx?(\.erb)?$/,
  use: [
    {
      loader: "ts-loader",
      options: PnpWebpackPlugin.tsLoaderOptions({
        appendTsSuffixTo: [/\.vue$/],
      }),
    },
  ],
}
```
