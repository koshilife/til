thebase.in の APIを使ってみた。

事前にアクセストークンを取得しておくこと。
ペラのomniauth-thebase画面を作って、tokenを取得した。


### refresh token を使って新しいアクセストークンを取得する

利用ライブラリ
https://github.com/oauth-xx/oauth2/blob/master/lib/oauth2/client.rb
参考
https://qiita.com/mm31/items/b2bfe8d43976fafd6a9b

内部で利用されているHTTP通信ライブラリ `Faraday`について
https://codezine.jp/article/detail/11462
https://gist.github.com/mitukiii/2775321

```.rb
require 'oauth2'
client_id =  'YOUR_CLIENT_ID'
client_secret = 'YOUR_CLIENT_SECRET'
site = 'https://api.thebase.in'
client = OAuth2::Client.new(client_id, client_secret, :site => site, :token_url => '/1/oauth/token')

token = 'YOUR_TOKEN'
refresh_token = 'YOUR_REFRESH_TOKEN'
expires_at = 1589213588

access_token = OAuth2::AccessToken.new(
      client,
      token,
      refresh_token: refresh_token,
      expires_at: expires_at
    )

# refresh token
new_token = access_token.refresh!

# use token
result = new_token.get('/1/users/me')
=> #<OAuth2::Response>
JSON.parse(result.body)
=> {"user"=>{"shop_id"=>"hoge", "shop_name"=>"hoge", "shop_introduction"=>"", "shop_url"=>"http://hoge.thebase.in", "twitter_id"=>"", "facebook_id"=>"", "ameba_id"=>"", "instagram_id"=>"", "background"=>nil, "display_background"=>0, "repeat_background"=>1, "logo"=>nil, "display_logo"=>0}}
```

## use token 標準ライブラリ利用パターン
 
```.rb
require 'net/http'

site = 'https://api.thebase.in'
uri = URI.parse "#{site}/1/users/me"

token = 'YOUR_TOKEN'
headers = { 'Authorization' => "Bearer #{token}"}

https = Net::HTTP.new(uri.host, uri.port)
https.use_ssl = true
req = Net::HTTP::Get.new(uri.request_uri, initheader = headers)
res = https.request(req)
JSON.parse(res.body)
=> {"user"=>{"shop_id"=>"hoge", "shop_name"=>"hoge", "shop_introduction"=>"", "shop_url"=>"http://hoge.thebase.in", "twitter_id"=>"", "facebook_id"=>"", "ameba_id"=>"", "instagram_id"=>"", "background"=>nil, "display_background"=>0, "repeat_background"=>1, "logo"=>nil, "display_logo"=>0}}
```
