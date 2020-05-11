thebase.in の APIを使ってみた。

事前にアクセストークンを取得しておくこと。
ペラのomniauth-thebase画面を作って、tokenを取得した。


### refresh token

```.rb
require 'oauth2'
client_id =  'YOUR_CLIENT_ID'
client_secret = 'YOUR_CLIENT_SECRET'
site = 'https://api.thebase.in'
client = OAuth2::Client.new(client_id, client_secret, :site => site)

```
 
```.rb
require 'net/http'

site = 'https://api.thebase.in'
uri = URI.parse "#{site}/1/users/me"

access_token = "HOGEHOGE"
headers = { 'Authorization' => "Bearer #{access_token}"}

https = Net::HTTP.new(uri.host, uri.port)
https.use_ssl = true
req = Net::HTTP::Get.new(uri.request_uri, initheader = headers)
res = https.request(req)
JSON.parse(res.body)
=> {"user"=>{"shop_id"=>"hoge", "shop_name"=>"hoge", "shop_introduction"=>"", "shop_url"=>"http://hoge.thebase.in", "twitter_id"=>"", "facebook_id"=>"", "ameba_id"=>"", "instagram_id"=>"", "background"=>nil, "display_background"=>0, "repeat_background"=>1, "logo"=>nil, "display_logo"=>0}}
```
