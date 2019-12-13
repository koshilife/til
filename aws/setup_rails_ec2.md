スタンドアロンのEC2構成でRailsの output 環境を作ったときのメモ
ディストリビューション: Amazon-Linux2

# AWS

## Network



## EC2 Setup


```
# Essential tools
sudo yum install -y git
```

```
# SSM-Agent
sudo yum install -y https://s3.amazonaws.com/ec2-downloads-windows/SSMAgent/latest/linux_amd64/amazon-ssm-agent.rpm
sudo systemctl status amazon-ssm-agent
```

```
# Nginx
sudo amazon-linux-extras install nginx1
sudo systemctl start nginx.service
sudo systemctl enable nginx.service
```

```
# Certbot

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
```
