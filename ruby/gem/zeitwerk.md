# zeitwerkに関するメモ

Rails6から採用されているモジュール、クラス読み込みを簡単にしてくれるライブラリ
[fxn/zeitwerk](https://github.com/fxn/zeitwerk)について備忘。

## Loader#collapse

Gemライブラリ内で読み込み側でネームスペースに含めずに、ディレクトリを作ってクラスをまとめたいケースが有り、
`loader.collapse`の機能を用いた。

[loader.rb](https://github.com/fxn/zeitwerk/blob/master/lib/zeitwerk/loader.rb)

### まず、Gem構成ではなくシンプルなrubyファイルのみで試してみる。

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

メインファイル

```main.rb
require 'zeitwerk'

loader = Zeitwerk::Loader.new
loader.push_dir __dir__
loader.collapse('bar') # bar/hoge.rb に対して適用
loader.collapse('*/bar') # foo1/bar/hoge1.rb, foo2/bar/hoge2.rb に対して適用
# loader.collapse('**/bar') # の１行で bar ディレクトリ はnamespaceから外される。
loader.log! # 確認のため、ログ機能を有効化
loader.setup
```

実行してみる

```.rb
# $ irb
>> require './main'
Zeitwerk@952b8d: autoload set for Foo1, to be autovivified from /Users/koshilife/zeitwerk_sandbox/foo1
Zeitwerk@952b8d: autoload set for Main, to be loaded from /Users/koshilife/zeitwerk_sandbox/main.rb
Zeitwerk@952b8d: autoload set for Foo2, to be autovivified from /Users/koshilife/koshilife/zeitwerk_sandbox/foo2
Zeitwerk@952b8d: autoload set for Hoge, to be loaded from /Users/koshilife/zeitwerk_sandbox/bar/hoge.rb
=> true
>> Hoge.hello
Zeitwerk@952b8d: constant Hoge loaded from file /Users/koshilife/zeitwerk_sandbox/bar/hoge.rb
Hoge

>> Foo1::Hoge1.hello
Zeitwerk@952b8d: module Foo1 autovivified from directory /Users/koshilife/zeitwerk_sandbox/foo1
Zeitwerk@952b8d: autoload set for Foo1::Hoge1, to be loaded from /Users/koshilife/zeitwerk_sandbox/foo1/bar/hoge1.rb
Zeitwerk@952b8d: constant Foo1::Hoge1 loaded from file /Users/koshilife/zeitwerk_sandbox/foo1/bar/hoge1.rb
Foo1::Hoge1

>> Foo2::Hoge2.hello
Zeitwerk@952b8d: module Foo2 autovivified from directory /Users/koshilife/zeitwerk_sandbox/foo2
Zeitwerk@952b8d: autoload set for Foo2::Hoge2, to be loaded from /Users/koshilife/zeitwerk_sandbox/foo2/bar/hoge2.rb
Zeitwerk@952b8d: constant Foo2::Hoge2 loaded from file /Users/koshilife/zeitwerk_sandbox/foo2/bar/hoge2.rb
Foo2::Hoge2
```

### Gemライブラリ構成で collapse


```.sh
$ bundle gem zeitwerk_sandbox_gem
(gemspec 内の TODOを削除するなどbundleが実行できるようにする)
$ echo "gem 'zeitwerk'" >> Gemfile
$ bundle

$ cd lib/zeitwerk_sandbox_gem
$ mkdir -p bar foo1/bar foo2/bar
$ echo "module ZeitwerkSandboxGem class Hoge; def self.hello; puts name; end; end; end" > bar/hoge.rb
$ echo "ZeitwerkSandboxGem module Foo1; class Hoge1; def self.hello; puts name; end; end; end; end" > foo1/bar/hoge1.rb
$ echo "ZeitwerkSandboxGem module Foo2; class Hoge2; def self.hello; puts name; end; end; end; end" > foo2/bar/hoge2.rb
```

```
$ tree
.
├── CODE_OF_CONDUCT.md
├── Gemfile
├── Gemfile.lock
├── LICENSE.txt
├── README.md
├── Rakefile
├── bin
│   ├── console
│   └── setup
├── lib
│   ├── zeitwerk_sandbox_gem
│   │   ├── bar
│   │   │   └── hoge.rb
│   │   ├── foo1
│   │   │   └── bar
│   │   │       └── hoge1.rb
│   │   ├── foo2
│   │   │   └── bar
│   │   │       └── hoge2.rb
│   │   └── version.rb
│   └── zeitwerk_sandbox_gem.rb
├── test
│   ├── test_helper.rb
│   └── zeitwerk_sandbox_gem_test.rb
└── zeitwerk_sandbox_gem.gemspec
```

メインファイル `lib/zeitwerk_sandbox_gem.rb`

```lib/zeitwerk_sandbox_gem.rb
require 'zeitwerk'
loader = Zeitwerk::Loader.for_gem
loader.collapse('lib/zeitwerk_sandbox_gem/bar')
loader.collapse('lib/zeitwerk_sandbox_gem/foo1/bar')
loader.collapse('lib/zeitwerk_sandbox_gem/foo2/bar')
# loader.collapse('**/bar') # この１行でも可
loader.log! # 確認のため、ログ機能を有効化
loader.setup

puts "[autoloads]"
loader.autoloads.each {|item| puts "  - #{item}"}
puts "[collapse_dirs]"
loader.collapse_dirs.each {|item| puts "  - #{item}"}
```

