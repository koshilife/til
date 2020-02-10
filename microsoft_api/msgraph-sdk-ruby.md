# [msgraph-sdk-ruby](https://github.com/microsoftgraph/msgraph-sdk-ruby) を読む

msgraph公式のgemを利用するにあたり、
公式READMEでは以下のようにAPIを利用すると案内があるが、
コード内に `def me` が定義されていないのに、プロフィールAPIにアクセスできるのか、わからなかったから。

```.rb
graph = MicrosoftGraph.new(base_url: "https://graph.microsoft.com/v1.0",
                           cached_metadata_file: File.join(MicrosoftGraph::CACHED_METADATA_DIRECTORY, "metadata_v1.0.xml"),
                           api_version: '1.6', # Optional
                           &callback
)

me = graph.me # get the current user
puts "Hello, I am #{me.display_name}."
```

## ざっとメモ

microsoft_graph.rb

```.rb
class MicrosoftGraph
  attr_reader :service
  BASE_URL = "https://graph.microsoft.com/v1.0/"

  def initialize(options = {}, &auth_callback)
    @service = OData::Service.new(
      api_version: options[:api_version],
      auth_callback: auth_callback,
      base_url: BASE_URL,
      metadata_file: options[:cached_metadata_file]
    )
    @association_collections = {}
    unless MicrosoftGraph::ClassBuilder.loaded?
      MicrosoftGraph::ClassBuilder.load!(service)
    end

  end
```

- Graph APIはODataプロトコルを通じてアクセスする、MSが作った？仕様策定に関わっているらしい OData参考 https://qiita.com/tami/items/411a226d1ea6bb25b5f1
  - ODataプロトコルで標準化することで、開発や利用するルールに一貫性ができ、開発生産性、利用の容易化に繋がっていると理解。
  - gem内に含まれる `metadata_v1.0.xml` は指定がない場合は、[metadata](https://graph.microsoft.com/v1.0/$metadata?detailed=true) にアクセスして、APIでアクセス可能なリソースとそのプロパティ情報の定義ファイルを取得する。

class_builder.rb
```.rb
    def self.load!(service)
      if !@@loaded
        @service_namespace = service.namespace
        service.entity_types.each do |entity_type|
          create_class! entity_type
        end

        service.complex_types.each do |complex_type|
          create_class! complex_type
        end
       ...

    def self.create_class!(type)
      superklass = get_superklass(type)
      klass = MicrosoftGraph.const_set(classify(type.name), Class.new(superklass))
      klass.const_set("ODATA_TYPE", type)
      klass.instance_eval do
        def self.odata_type
          const_get("ODATA_TYPE")
        end
      end
      create_properties(klass, type)
      create_navigation_properties(klass, type) if type.respond_to? :navigation_properties
    end

```

- ClassBuilder が冒頭の謎の答え
  - `MicrosoftGraph::ClassBuilder` で、metadataファイル内で定義された内容から entity_type、complex_types、entity_sets、actions、functions、singletonsを順にクラス化して、各リソースへアクセスできるRubyのIFを定義していた(メタプログラミング)
- ClassBuilder#create_class!
  - 例えば、以下 `subscription`エンティティのメタデータは以下。
  - entity_typesでパースされた内容となる。
  

```.xml
<EntityType Name="subscription" BaseType="graph.entity">
  <Property Name="resource" Type="Edm.String" Nullable="false"/>
  <Property Name="changeType" Type="Edm.String" Nullable="false"/>
  <Property Name="clientState" Type="Edm.String"/>
  <Property Name="notificationUrl" Type="Edm.String" Nullable="false"/>
  <Property Name="expirationDateTime" Type="Edm.DateTimeOffset" Nullable="false"/>
  <Property Name="applicationId" Type="Edm.String"/>
  <Property Name="creatorId" Type="Edm.String"/>
</EntityType>
```

OData::Service

```
 def entity_types
      @entity_types ||= metadata.xpath("//EntityType").map do |entity|
        options = {
          name:                  "#{namespace}.#{entity["Name"]}",
          abstract:              entity["Abstract"] == "true",
          base_type:             entity["BaseType"],
          open_type:             entity["OpenType"] == "true",
          has_stream:            entity["HasStream"] == "true",
          service:               self,
        }
        @type_name_map["#{namespace}.#{entity["Name"]}"] = EntityType.new(options)
      end
    end
```

1ループないでは、以下のようにメタデータファイル内容がはまり、OData::EntityTypeクラスが生成され、１つづつ create_class! が呼ばれクラスが生成される

```.rb
options = {
          name:       "microsoft.graph.subscription",
          abstract:   nil,
          base_type:  nil,
          open_type:  false
          has_stream: false,
          service:    self,　# (OData::Service インスタンス)
 }
 @type_name_map["microsoft.graph.subscription"] = EntityType.new(options)
```

## もう少し理解したいこと

```
graph = MicrosoftGraph.new
graph.me
# me のメソッド定義の仕組みを深堀り
# さらに、 https://graph.microsoft.com/v1.0/me/ にアクセスし、返却された値をオブジェクトにマッピングするまでの流れ
```

