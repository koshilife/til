# メタプログラミングについて

OSSのコードを読む上でメタプログラミング使われることが多かったので、書いてみる。
以下、Hogeモジュールのは以下に、任意のプロパティを持つ任意のクラスを作る例

```.rb
module Hoge
  class BaseEntity
  end
  class ClassBuilder
    def self.create_class!(class_name, propety_names=[])
      klass = Hoge.const_set(class_name, Class.new(BaseEntity))
      propety_names.each do |property_name|
        define_getter_and_setter(klass, property_name)
      end
    end

    def self.define_getter_and_setter(klass, property_name)
      klass.class_eval do
        define_method(property_name.to_sym) do
          eval "@#{property_name}"
        end
        define_method("#{property_name}=".to_sym) do |value|
          eval "@#{property_name} = value"
        end
      end
    end
  end
end
```


```.rb
>> Hoge::ClassBuilder.create_class!('Bar', ['b1','b2'])
>> b = Hoge::Bar.new
>> b.b1 = 'b1 value'
>> b.b2 = 'b2 value'
>> p b.b1
"b1 value"
>> p b.b2
"b2 value"
```

msgraph のソース上では、
define_method内でインスタンス変数を参照する際は eval "@#{property_name}=#{value}" といった eval を使う必要があるが、
`cached_property_values` の Hashを持つことで、シンプルにかけるよう工夫していた。

```.rb
module Hoge
  class BaseEntity
    def initialize
      @cached_property_values = {}
    end
  end
  class ClassBuilder
    def self.create_class!(class_name, propety_names=[])
      klass = Hoge.const_set(class_name, Class.new(BaseEntity))
      propety_names.each do |property_name|
        define_getter_and_setter(klass, property_name)
      end
    end

    def self.define_getter_and_setter(klass, property_name)
      klass.class_eval do
        define_method(property_name.to_sym) do
          @cached_property_values[property_name.to_sym]
        end
        define_method("#{property_name}=".to_sym) do |value|
          @cached_property_values[property_name.to_sym] = value
        end
      end
    end
  end
end
```

参考:
- https://github.com/microsoftgraph/msgraph-sdk-ruby/blob/master/lib/microsoft_graph/class_builder.rb
- https://qiita.com/iguchi1124/items/a2a64f27e6dabbac1301

