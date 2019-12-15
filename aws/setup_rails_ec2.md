スタンドアロンのEC2構成でRailsの output 環境を作ったときのメモ
ディストリビューション: Amazon-Linux2

# AWS

## Network



## EC2 Setup

```
# UTC->JST 参考: https://qiita.com/nikuruyo/items/19504660c50efe3c24c2
$ sudo vi /etc/sysconfig/clock
------
#ZONE="UTC"
ZONE="Asia/Tokyo"
UTC=true
------
$ sudo ln -sf /usr/share/zoneinfo/Japan /etc/localtime
```

```
# update yum & install essential tools
$ sudo yum update -y
$ sudo yum install -y git
$ sudo yum install -y readline-devel
$ sudo yum groupinstall "Development Tools"
$ sudo yum install ruby-devel
$ sudo yum install sqlite-devel
```

```
# SSM-Agent
$ sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
$ sudo systemctl status amazon-ssm-agent
```

```
# Nginx
$ sudo amazon-linux-extras install nginx1
$ sudo systemctl start nginx.service
$ sudo systemctl enable nginx.service
```

```
# Certbot install (参考: https://qiita.com/MysteriousMonkey/items/4d3d857c0e68d4bfff39)
$ sudo wget -r --no-parent -A 'epel-release-*.rpm' http://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/e/
$ sudo rpm -Uvh dl.fedoraproject.org/pub/epel/7/x86_64/Packages/e/epel-release-*.rpm
$ sudo yum-config-manager --enable epel*
$ sudo yum install certbot python2-certbot-nginx

# 証明書発行
$ sudo certbot --nginx -d example.com -d www.example.com -d dev.example.com -d www.dev.example.com
$ sudo certbot certificates
Saving debug log to /var/log/letsencrypt/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Found the following certs:
  Certificate Name: example.com
    Domains: example.com dev.example.com www.dev.example.com www.example.com
    Expiry Date: 2020-03-06 23:51:10+00:00 (VALID: 84 days)
    Certificate Path: /etc/letsencrypt/live/example.com/fullchain.pem
    Private Key Path: /etc/letsencrypt/live/example.com/privkey.pem
```

```
# rbenv
$ git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
$ echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
$ echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
$ exec $SHELL -l
$ rbenv -v
rbenv 1.1.2-11-gc46a970

$ git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
$ rbenv install --list
$ rbenv install 2.6.5
Downloading ruby-2.6.5.tar.bz2...
-> https://cache.ruby-lang.org/pub/ruby/2.6/ruby-2.6.5.tar.bz2
Installing ruby-2.6.5...
  
BUILD FAILED (Amazon Linux 2 using ruby-build 20191205)

Inspect or clean up the working tree at /tmp/ruby-build.20191213002308.29267.uu7AEY
Results logged to /tmp/ruby-build.20191213002308.29267.log

Last 10 log lines:
 95% [1039/1093]  node.c
 95% [1040/1093]  numeric.c
 95% [1041/1093]  object.c
 95% [1042/1093]  pack.c
 95% [1043/1093]  parse.c
 95% [1044/1093]  parse.y
 95% [1045/1093]  prelude.c
 95% [1046/1093]  prelude.rb
 95% [1047/1093]  proc.c
make: *** [rdoc] Killed

# 失敗したので https://qiita.com/ytsukamoto/items/2367461281087657abcf を参考に再インストール
$ export RUBY_CONFIGURE_OPTS=--disable-install-doc
$ rbenv install 2.6.5
$ rbenv rehash
$ rbenv global 2.6.5
$ ruby -v
ruby 2.6.5p114 (2019-10-01 revision 67812) [x86_64-linux]
```

```
# git,ssh設定 参考: https://qiita.com/shizuma/items/2b2f873a0034839e47ce
# Githubにデプロイキーを登録

$ vim ~/.ssh/github.pem
<<作成した秘密鍵の情報を貼り付け>>

$ vim ~/.ssh/config 
Host github github.com
    HostName github.com
    IdentityFile ~/.ssh/github.pem
    User git
```
