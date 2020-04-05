# Rails x Vue.js x TypeScript のセットアップ

Vue.js TypeScript 環境を作ったときのメモ

###  環境

- rails `6.0.2.1`
- @rails/webpacker `4.2.2`

- 参考:
  - https://qiita.com/kyo_nanba/items/d1678fbf8d3e60f913a1
  - http://clash-m45.hatenablog.com/entry/2019/09/21/120215

```.sh
bundle exec rails webpacker:install
bundle exec rails webpacker:install:typescript
```

変更したファイル

```app/javascript/packs/types/vue.d.ts
declare module "*.vue" {
    import Vue from 'vue'
    export default Vue
}
```

```config/webpack/loaders/typescript.js
const PnpWebpackPlugin = require('pnp-webpack-plugin')

module.exports = {
  test: /\.(ts|tsx)?(\.erb)?$/,
  use: [
    {
      loader: 'ts-loader',
      options: PnpWebpackPlugin.tsLoaderOptions({
        appendTsSuffixTo: [/\.vue$/]
      })
    }
  ]
}
```

splitChunks の設定を有効にしておくと、JSの読み込み効率が高められるとのこと

- 参考
  - https://qiita.com/chimame/items/8d3d6f4afea675cffa7d
  - https://qiita.com/soarflat/items/1b5aa7163c087a91877d

```config/webpack/environment.js
const { environment } = require('@rails/webpacker')
const typescript =  require('./loaders/typescript')
const { VueLoaderPlugin } = require('vue-loader')
const vue = require('./loaders/vue')

environment.plugins.prepend('VueLoaderPlugin', new VueLoaderPlugin())
environment.loaders.prepend('vue', vue)
environment.loaders.prepend('typescript', typescript)

// 追加
// + environment.splitChunks()

// オプション指定する場合はこんなふうに追加する
// + environment.splitChunks((config) => Object.assign({}, config, { optimization: { splitChunks: false }}))

module.exports = environment
```

