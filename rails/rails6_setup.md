# Rails 6

- v6.0.2
- DB: mongo (mongoid)

```
$ mkdir hello_rails6
$ cd hello_rails6
$ bundle init
$ vim Gemfile
rails のみコメントイン
$ bundle update
$ bundle install

$ bundle exec rails new . --skip-turbolinks --webpack=vue --skip-active-record

# 警告で推奨されたので。
$ bundle lock --add-platform x86-mingw32 x86-mswin32 x64-mingw32 java

# Gemに`mongoid`を追加
$ vim Gemfile
$ bundle install
$ rails g mongoid:config
```
