## oauth-xx/oauth2 について

JSON.parse のオプション指定の仕方。

```.rb
faraday_build_proc = proc { |builder|
  builder.response(:json, parser_options: { symbolize_names: true }, content_type: /\bjson$/)
}
opts = {
  site: 'https://example.com',
  connection_build: faraday_build_proc
}
client = OAuth2::Client.new('CLIENT_ID', 'CLIENT_SECRET', opts)
```

access tokenの更新

```
token = OAuth2::AccessToken.new(client, 'ACCESS_TOKEN', refresh_token: 'REFRESH_TOKEN', expires_at: 'EXPIRES_AT(INT)')
new_token = token.refresh # refresh! でも可
```

参考:
https://github.com/oauth-xx/oauth2/blob/master/lib/oauth2/client.rb
