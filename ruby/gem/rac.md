# Racについて

https://github.com/rack/rack

Rack, a modular Ruby webserver interface

## Racとは

ウェブサーバ用の標準化されたインターフェースであり
Puma、Unicornなどのアプリケーション・サーバー
Rails、SinatraなどのWebフレームワークも採用している
プロトコルが同じなので、UnicornからPumaへの変更が容易になることがメリットの１つとしてあげられる。
かなりシンプルな作りなので、Rac Middlewareと呼ばれるRacを繋げて汎用性が高いのも広く使われている理由なのではないか。

PythonでのWSGIと同位置付けであり、Racの思想としても影響を受けているとのこと。

## Racの作り方

基本的には、
envを引数で受け取り、HTTPステータス、ヘッダー、ボディの３つの情報を返すcallメソッドを用意するだけで作れる


参考記事より
```.rb
class HelloRackApp
  # callメソッドはenvを受け取り、3つの値(StatusCode, Headers, Body)を配列として返す
  def call(env)
    [200, { "Content-Type" => "text/plain" }, ["Hello Rack!"]]
  end
end

# callメソッドを呼び出せるObjectをrunに渡し、rackアプリを起動する
run HelloRackApp.new
```

## Rac Middleware

参考記事より

>Rack Applicationと違い、responseを直接返すのではなく別の処理を呼び出しデータを加工するために使います

リクエスト情報(env,headers,body)の情報を元に、次のRacにわたす前に
status,headers,body等を加工して引き渡すことが可能となる。Unix,Linuxでいうパイプ的な考え方に似ている？
Racからたくさんの便利Middlewareが提供されており、Railsでこれら標準Rac+独自Racを組み合わせてWebフレームワークを作っている。

認証系ライブラリの [devise](https://github.com/heartcombo/devise)や[OmniAuth](https://github.com/omniauth/omniauth/) もRack basedなライブラリである。

### 参考:

- [Rack入門 Rack Middleware編 (1/3)](https://qiita.com/nishio-dens/items/8011842f50995f46eafe)
- [Rack入門 Rack Middleware編 (2/3)](https://qiita.com/nishio-dens/items/852d7604b34d20514a70)
- [Rack入門 Rack Middleware編 (3/3)](https://qiita.com/nishio-dens/items/e293f15856d849d3862b)
- [メモ: Rack::Builderについて](https://steemit.com/ruby/@okazu/rack-builder)
- [Rails Guide Rails と Rack](https://railsguides.jp/rails_on_rack.html)
