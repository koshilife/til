# zeitwerkに関するメモ

Rails6から採用されているモジュール、クラス読み込みを簡単にしてくれるライブラリ
[fxn/zeitwerk](https://github.com/fxn/zeitwerk)について備忘。

## Loader#collapse

Gemライブラリ内で読み込み側でネームスペースに含めずに、ディレクトリを作ってクラスをまとめたいケースが有り、
`loader.collapse`の機能を用いた。

[loader.rb](https://github.com/fxn/zeitwerk/blob/master/lib/zeitwerk/loader.rb)

### まず、Gemではなくシンプルなrubyファイルで試してみる。

ファイル構成
```.sh
$ mkdir zeitwerk_sandbox
$ cd zeitwerk_sandbox
$ bundle init
$ echo "gem 'zeitwerk'" >> Gemfile
$ bundle

$ mkdir -p bar foo1/bar foo2/bar
$ echo "class Hoge; def self.hello; puts name; end; end" > bar/hoge.rb
$ echo "module Foo1; class Hoge1; def self.hello; puts name; end; end; end" > foo1/bar/hoge1.rb
$ echo "module Foo2; class Hoge2; def self.hello; puts name; end; end; end" > foo2/bar/hoge2.rb

$ tree
.
├── Gemfile
├── Gemfile.lock
├── bar
│   └── hoge.rb
├── foo1
│   └── bar
│       └── hoge1.rb
└── foo2
    └── bar
        └── hoge2.rb
```

