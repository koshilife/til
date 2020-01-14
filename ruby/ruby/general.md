# Ruby 一般的

- Rubyならではの引数処理
[ruby引数処理に使えるテクニック](https://qiita.com/metheglin/items/306e81c95f8a5cdea296#symbolize_keys)

- [includeされた時にクラスメソッドとインスタンスメソッドを同時に追加する頻出パターン](https://www.techscore.com/blog/2013/03/01/rails-include%E3%81%95%E3%82%8C%E3%81%9F%E6%99%82%E3%81%AB%E3%82%AF%E3%83%A9%E3%82%B9%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89%E3%81%A8%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%BF%E3%83%B3%E3%82%B9%E3%83%A1/)

## Module include export

- include はインスタンスメソッドとして追加されるが、export はクラスメソッドとして追加される  
どちらも意識せずにimportさせたいときのテクニックとして included というメソッドを使った例を示す
 
 ```
 module HogeModule
    def logger
        Rails.logger
    end
    module_function :logger

    module ClassMethods
        def logger
            CommonModule.logger
        end
    end

    def self.included(base)
        base.extend(ClassMethods)
    end
end
```

参考
[Rails: includeされた時にクラスメソッドとインスタンスメソッドを同時に追加する頻出パターン
](https://www.techscore.com/blog/2013/03/01/rails-include%E3%81%95%E3%82%8C%E3%81%9F%E6%99%82%E3%81%AB%E3%82%AF%E3%83%A9%E3%82%B9%E3%83%A1%E3%82%BD%E3%83%83%E3%83%89%E3%81%A8%E3%82%A4%E3%83%B3%E3%82%B9%E3%82%BF%E3%83%B3%E3%82%B9%E3%83%A1/)

## private メソッド

- インスタンスメソッドは `Module#private` を用いる
  - methodの前に宣言しておくとそれ以降に宣言されたmethodはprivateとなる
  - `private :some_method` で明示的に書いてもOK

- クラスメソッドは `Module#private_class_method` 
