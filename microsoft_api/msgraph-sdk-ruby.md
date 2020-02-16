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


Class の作り方

ClassBuilder 内

```.rb
    def self.create_class!(type)
      superklass = get_superklass(type)
      # 上の例では、MicrosoftGraph モジュール内に、Subscription というクラスを定義
      klass = MicrosoftGraph.const_set(classify(type.name), Class.new(superklass))
      # Subscription に ODATA_TYPE を
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

ruby提供機能のメモ
- const_set: 定数追加
- class_eval: Blockでクラスメソッド定義に利用
- instance_eval: Blockでインスタンスメソッド定義に利用
- define_method: メソッドを作成

メタデータに沿って読み込んだ内容をクラスと関連プロパティを定義する実装がされていた。
create_properties でセッター、ゲッターを定義していたのを確認

## もう少し理解したいこと

```
graph = MicrosoftGraph.new
graph.me
# me のメソッド定義の仕組みを深堀り
# さらに、 https://graph.microsoft.com/v1.0/me/ にアクセスし、返却された値をオブジェクトにマッピングするまでの流れ

me.path
=> "users/xxx-xxx-xxx-xxx-xxx"

graph.me.public_methods
=> [:department=, :mail, :reminder_view, :state, :account_enabled, :account_enabled=, :surname, :preferred_language, :user_principal_name, :company_name, :on_premises_last_sync_date_time, :on_premises_last_sync_date_time=, :on_premises_sync_enabled, :on_premises_sync_enabled=, :postal_code=, :mail=, :mail_nickname, :mail_nickname=, :on_premises_security_identifier, :on_premises_security_identifier=, :proxy_addresses, :postal_code, :proxy_addresses=, :calendars, :navigation_properties, :member_of, :calendar_view, :photo, :state=, :photo=, :events, :send_mail, :display_name=, :city, :messages, :assigned_plans, :assigned_plans=, :business_phones, :business_phones=, :city=, :preferred_language=, :provisioned_plans, :provisioned_plans=, :assigned_licenses, :assigned_licenses=, :company_name=, :job_title, :job_title=, :mobile_phone, :mobile_phone=, :on_premises_immutable_id, :on_premises_immutable_id=, :password_policies, :password_policies=, :password_profile, :password_profile=, :office_location, :office_location=, :surname=, :usage_location, :usage_location=, :user_principal_name=, :user_type, :user_type=, :about_me, :about_me=, :birthday, :birthday=, :hire_date, :hire_date=, :interests, :interests=, :my_site, :my_site=, :past_projects, :past_projects=, :preferred_name, :preferred_name=, :responsibilities, :street_address, :display_name, :schools=, :responsibilities=, :skills, :owned_devices, :country, :schools, :manager=, :registered_devices, :skills=, :direct_reports, :created_objects, :owned_objects, :mail_folders, :calendar_groups, :properties, :contact_folders, :contacts, :calendar=, :calendar, :drive, :country=, :manager, :assign_license, :change_password, :given_name, :street_address=, :department, :given_name=, :drive=, :get_member_groups, :get_member_objects, :check_member_groups, :id, :id=, :save!, :fetch, :parent=, :parental_chain, :reload!, :delete!, :graph=, :parent, :persisted?, :save, :graph, :containing_navigation_property, :path, :as_json, :dirty?, :mark_clean, :to_json, :odata_type, :require_dependency, :instance_values, :instance_variable_names, :presence, :blank?, :html_safe?, :with_options, :to_yaml, :deep_dup, :present?, :acts_like?, :duplicable?, :to_param, :to_query, :in?, :presence_in, :pretty_print_inspect, :pretty_print_instance_variables, :pretty_print, :pretty_print_cycle, :to_v8, :to_ruby, :__union__, :__union_from_object__, :__array__, :__expand_complex__, :regexp?, :__deep_copy__, :__add__, :__add_from_array__, :__intersect__, :__intersect_from_object__, :__intersect_from_array__, :__evolve_object_id__, :__find_args__, :__mongoize_object_id__, :__mongoize_time__, :do_or_do_not, :__setter__, :blank_criteria?, :multi_arged?, :mongoize, :you_must, :remove_ivar, :substitutable, :__to_inc__, :resizable?, :__sortable__, :ivar, :numeric?, :to_bson_normalized_value, :to_bson_key, :to_bson_normalized_key, :try, :try!, :require_or_load, :load_dependency, :unloadable, :instance_variable_defined?, :remove_instance_variable, :instance_of?, :kind_of?, :is_a?, :tap, :instance_variable_set, :protected_methods, :instance_variables, :instance_variable_get, :public_methods, :private_methods, :method, :public_method, :public_send, :singleton_method, :class_eval, :define_singleton_method, :extend, :to_enum, :enum_for, :<=>, :===, :=~, :!~, :gem, :eql?, :respond_to?, :byebug, :debugger, :remote_byebug, :freeze, :inspect, :pretty_inspect, :object_id, :send, :to_s, :display, :nil?, :hash, :class, :singleton_class, :clone, :dup, :itself, :yield_self, :then, :taint, :tainted?, :untaint, :untrust, :untrusted?, :trust, :frozen?, :methods, :singleton_methods, :equal?, :!, :==, :instance_exec, :!=, :instance_eval, :__id__, :__send__]
```

参考: metadataについて
>https://docs.microsoft.com/ja-jp/graph/traverse-the-graph
>メタデータにより、要求および応答パケットが表すリソースを構成するエンティティの種類、複合型、列挙型を含む、Microsoft Graph のデータ モデルを参照し、理解することができます
