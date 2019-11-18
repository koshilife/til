できるだけ維持コストを安く AWS環境で EC2 x SSL環境 を構築したい。
ACMを利用すると ELB 利用必須となり、EC2代金の他にランニングコストが数2-3千円/月のコストがかかるのでELBは利用しない。
選択肢として、Let's Encrypt と [certbot](https://certbot.eff.org/) を使う。

### 必要だったパッケージ

```
sudo yum install -y git
```


### Nginx

```
$ sudo amazon-linux-extras install nginx1
```

にてstableの1.16が入った

```
$ sudo systemctl start nginx.service
$ sudo systemctl enable nginx.service
```

起動と自動起動設定をしておく

### certbot


参考:
  - [Let's Encrypt で Nginx にSSLを設定する](https://qiita.com/HeRo/items/f9eb8d8a08d4d5b63ee9)
  - [Let's Encryptを使ってEC2にSSL証明書の発行から自動更新まで行う](https://qiita.com/sayama0402/items/011644191dfdbde9c646)
  - [光の速さのWEBサーバー(nginx)をlet's encryptでSSL化及びHTTP/2化。ついでにセキュリティ評価をA＋にする。](https://qiita.com/sak_2/items/ff835b669c0a7e110b09)
