# Rails7 x PostgreSQL


```
$ rbenv local 3.1.3
$ bundle init
$ vim Gemfile
// commentin rails

$ bundle exec rails new . \
  --api \
  --database=postgresql \
  --skip-action-mailer \
  --skip-action-text \
  --skip-action-cable \
  --skip-sprockets \
  --skip-turbolinks \
  --skip-webpack-install \
  --skip-test \
  --skip-system-test
```

