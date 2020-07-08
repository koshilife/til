# omniauth に関するメモ

認証プロバイダを利用してユーザ認証を標準化したOSS
各認証プロバイダ用の仕様差異を似たような形で比較的容易にStrategyを作れる。

# omniauth-oauth2 を継承して独自ストラテジの開発

- OAuth2認証形式のStrategyを作る際の雛形 [omniauth-oauth2](https://github.com/omniauth/omniauth-oauth2) が公開されている
- https://github.com/koshilife/omniauth-tanita を作った際のポイントを備忘する

* omniauth のライフサイクル
  * setup phase: Strategy側のセットアップ、動的にオプションを変更したい時などに利用。デフォルトではOFF
  * request phase: 外部サービスにリダイレクトする, OAuth2だと認可用URLへリダイレクトする
    * omniauth では [oauth-xx/oauth2](https://github.com/oauth-xx/oauth2) を採用しており、redirect URLを生成する際のオプションを `client_options` `authorize_options` を指定しOAuth2ライブラリのオプションに引き渡しているので、oauth-xx/oauth2 の仕様を理解しよう。
  * callback phase
    * 認可コードつきのコールバックを受け取り、トークンを取得するフェーズ。
    * こちらも oauth-xx/oauth2 の機構を使ってやりとりするので、 `token_options` を指定してオプションを引き渡して調整する。
    * もし、プロバイダ側のユーザの基本情報(info)を取得しない場合は、`skip_info` オプションを true に指定。

* oauth2では CSRFのチェックができるが、サービス・プロバイダ側で対応していない場合は、例外が発生するので、 `provider_ignores_state: true` に設定する

>Authentication failure! csrf_detected: OmniAuth::Strategies::OAuth2::CallbackError

参考:

- 概要
  - [OmniAuth OAuth2 を使って OAuth2 のストラテジーを作るときに知っていると幸せになれるかもしれないこと](https://qiita.com/tyamagu2/items/d242b6dd4a20b657bb39)
  - [OmniAuth(OAuth2)処理をキックしたときのパラメータを認証用リクエストにも渡したい](https://qiita.com/t_oginogin/items/dfac4084a1fd1c854006)

- 本家
  - [omniauth/omniauth](https://github.com/omniauth/omniauth)
    - [strategy.rb](https://github.com/omniauth/omniauth/blob/master/lib/omniauth/strategy.rb)
  - 利用されてるライブラリ [oauth-xx/oauth2](https://github.com/oauth-xx/oauth2)
  - [omniauth/omniauth-oauth2](https://github.com/omniauth/omniauth-oauth2)

- 参考にした先輩ライブラリ
  - [omniauth-google-oauth2](https://github.com/zquestz/omniauth-google-oauth2)
  - [omniauth-twitter](https://github.com/arunagw/omniauth-twitter)
  - [omniauth-slack](https://github.com/kmrshntr/omniauth-slack)

## 本家のコードリーディングメモ

Middlewareの登録、Railsアプリでは以下のように行う。

```.rb
Rails.application.config.middleware.use OmniAuth::Builder do
  provider :zoom, zoom_client_id, zoom_client_secret, :scope => 'user:read'
end
```

https://github.com/omniauth/omniauth/blob/master/lib/omniauth/builder.rb#L29:L38

OmniAuth::Builder内で `def provider(klass, *args, &block)` メソッドで、 middleware の登録が行われており、
継承元の `::Rack::Builder#use` メソッド https://www.rubydoc.info/gems/rack/Rack/Builder#use-instance_method
にてミドルウェアを追加登録している。



リクエストフェーズやコールバックフェーズの発動ロジック
https://github.com/omniauth/omniauth/blob/master/lib/omniauth/strategy.rb#L177:L193

```.rb
    # The logic for dispatching any additional actions that need
    # to be taken. For instance, calling the request phase if
    # the request path is recognized.
    #
    # @param env [Hash] The Rack environment.
    def call!(env) # rubocop:disable CyclomaticComplexity, PerceivedComplexity
      unless env['rack.session']
        error = OmniAuth::NoSessionError.new('You must provide a session to use OmniAuth.')
        raise(error)
      end

      @env = env
      @env['omniauth.strategy'] = self if on_auth_path?

      return mock_call!(env) if OmniAuth.config.test_mode
      return options_call if on_auth_path? && options_request?
      return request_call if on_request_path? && OmniAuth.config.allowed_request_methods.include?(request.request_method.downcase.to_sym)
      return callback_call if on_callback_path?
      return other_phase if respond_to?(:other_phase)

      @app.call(env)
    end
```

testmode フラグなら、mock_call!(env)
on_auth_path?, options_request?, on_request_path?, on_callback_path? パスなら、どのロジックを呼び出すか振り分けている。
どこにも引っかからない場合は、親のRacアプリに処理を戻す。

