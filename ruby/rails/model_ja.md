# Rails Modelクラスに関する学び

## #new, #build

- build は newの alias で同じ。
  - [railsのnewとbuildの違い]https://qiita.com/sukechansan/items/6bae532b4f678fdcf87d]

## #pluck

- 2019/05/07 mapではなく、メモリに優しい効率的なpluckを使おう
  - [Rails: pluckでメモリを大幅に節約する](https://techracho.bpsinc.jp/hachi8833/2018_09_26/62333)


### #includes

- 2019/02/28 既存プロダクトのソースを読み込みしていたら includes メソッドの挙動を以下で確認した。
  - [関連するテーブルをまとめて取得(includes)](http://railsdoc.com/references/includes)
  - [似ているようで全然違う!?Activerecordにおけるincludesとjoinsの振る舞いまとめ](https://qiita.com/south37/items/b2c81932756d2cd84d7d)
