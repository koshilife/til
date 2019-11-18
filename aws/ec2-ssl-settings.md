できるだけ維持コストを安く AWS環境で EC2 x SSL環境 を構築したい。
ACMを利用すると ELB 利用必須となり、EC2代金の他にランニングコストが数2-3千円/月のコストがかかるのでELBは利用しない。
選択肢として、Let's Encrypt と [certbot]https://certbot.eff.org/) を使う。

参考: https://qiita.com/sayama0402/items/011644191dfdbde9c646

```
$ sudo curl https://dl.eff.org/certbot-auto -o /usr/bin/certbot-auto
$ sudo chmod 700 /usr/bin/certbot-auto
$ sudo certbot-auto certonly --webroot -w /var/www/html -d hoge.com --email hoge@hoge.com -n --agree-tos --debug
↑コマンド検証中
```
