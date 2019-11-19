
- SessionManager を使えば踏み台サーバが不要になるとか、調べる価値ありそう。
- あとで読む
  - [AWS Session Managerを使用して30分でSSHログインをやめることができた話 〜JAWS DAYS 2019〜](https://qiita.com/jaramon/items/4d2fca7dc53a8fd3e2e4)
  - [1日でSSHをやめることができた話 #jawsdays](https://speakerdeck.com/3utama/1ri-tesshwoyamerukotokatekitahua-number-jawsdays)

### 2019/11/18 導入した

参考: https://dev.classmethod.jp/cloud/aws/ssm-session-manager-release/

- Amazon Linux2 で試す
  - インスタンスに割り当てる EC2 Role に `AmazonEC2RoleforSSM` を追加
  - SSMエージェントを入れる [参考](https://docs.aws.amazon.com/ja_jp/systems-manager/latest/userguide/sysman-manual-agent-install.html#agent-install-al)
  - Sessinon Manager からインスタンスを選ぶ

### 2019/11/19

Macから直ログインも可能らしい [参考](https://dev.classmethod.jp/cloud/ssm-session-manager-from-mac-to-linux-ec2/)

試した。
- iam に policy 追加 [例 4: すべてのセッションにフル (管理) アクセスを許可する](https://docs.aws.amazon.com/ja_jp/systems-manager/latest/userguide/getting-started-restrict-access-examples.html)
- Userにポリシーを追加してインスタンスへアクセスしたが以下エラーに遭遇、権限が足りないらしい

```
An error occurred (AccessDeniedException) when calling the StartSession operation: User: arn:aws:iam::xxx:user/xxx is not authorized to perform: ssm:StartSession on resource: arn:aws:ec2:ap-northeast-1:xxx:instance/i-xxx
````
