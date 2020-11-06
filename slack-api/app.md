# slack app に関するメモ

## 外部ワークスペースへの公開

自分のワークスペース以外に対してアプリを公開するには、外部公開の設定が必要
レビューが通っていない場合は、承認されていないことは表示されるが、OAuthトークンは取得が可能となる。

- `Manage Distribution` -> `Share Your App with Other Workspaces` から設定できる

## Interactive

Slackのメッセージ内ボタン、モーダル内のアクション、チャンネルへの参加などをトリガーに
アクション情報をWebhookで受け取り、いくつかの応答ができる。

The flow of user-triggered interactions
Preparing your app for user interactions
Handling interaction payloads
Responding to users

Action付きメッセージのとくり方
https://api.slack.com/messaging/interactivity

Webhook応答パターン
https://api.slack.com/interactivity/handling

### Acknowledgment response

3秒以内に200 OK を返却しないと、エラーメッセージを表示する
### Message responses

payload発生から30分以内に5回まで利用できる

```
curl -X POST -H 'Content-type: application/json' --data \
'{"text": "Thanks for your request, we will process it and get back to you."}' \
<RESPONSE_URL>
```

Publishing ephemeral
メッセージを他の人でも見れるよう永続化する場合は、`"response_type": "ephemeral"` を指定
```
curl -X POST -H 'Content-type: application/json' --data \
'{"text": "Thanks for your request, we will process it and get back to you.", "response_type": "ephemeral"}' \
<RESPONSE_URL>
```

Updating a source message in response メッセージの編集もできる
```

```
