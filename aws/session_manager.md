
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
  
