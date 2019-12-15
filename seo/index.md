# SEOについての学び

## 基本知識


- 学習ソース
  - https://manablog.org/seo/
  - 人づて、ネット
 
## ツール

- 順位確認
  - 有料 RankTracker http://www.ranktracker.jp/
  - 無料 http://checker.search-rank-check.com/

## nofollow

`nofollow` はクローラーに指定リンクに対して追わせない(被リンク扱い)とさせない。
リンク毎もしくは、ページに対して指定が可能。狙いとしては

- ページ内のリンクの重み付けしたい時
- 怪しいサイト、競合サイトに対して、被リンク得点を挙げたくない時

ページ全体
```
<meta name=”robots” content=”nofollow” />
```
個別リンク
```
<a href=”<URL>” rel=”nofollow”>リンクテキスト</a>
```

## noindex

検索エンジンに検索結果として表示させたくないページに指定する。

```
<meta name=”robots” content=”noindex”>
```

### 小ネタ

WordPressのプラグイン `XML Sitemap Generator for WordPress 4.1.0` を使っている場合に
GoogleSearchConsoleから以下のような警告メールが来たことがあり、プラグイン設定の `HTML形式でのサイトマップを含める` をOFFにすることで解決できる。

```
弊社では、貴サイトの「カバレッジ」に関する問題の修正について検証を開始いたしました。具体的には、1 ページに現在影響を及ぼしている「送信された URL に noindex タグが追加されています」について確認しております。
```

参考: https://www.blogging-life.com/how-to-fix-submitted-url-marked-noindex/


## その他 テクニック

- 画像のalt タグ
  - 参考: https://manablog.org/seo-alt/
```
解決策：h2、もしくはh3タグをaltタグにいれておく
```

- meta description
  - 参考: https://manablog.org/meta-description/#03
```
読者への問いかけ
記事の内容
アクションを促す
具体例①：この記事の場合

SEO対策のmeta descriptionに関する網羅的な情報を知りたいですか？
→読者への問いかけ

本記事では、meta descriptionの概要説明から、実装方法、効果的な書き方を解説します。
→記事の内容

meta descriptionをマスターしたい方は必見です。
→アクションを促す（合計：121文字）
```
