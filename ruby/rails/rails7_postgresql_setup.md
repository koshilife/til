# Rails7 x PostgreSQL

## rails new

```
$ rbenv local 3.1.3
$ bundle init
$ vim Gemfile
// commentin rails

$ bundle exec rails new . \
  --minimal \
  --api \
  --database=postgresql \
  --skip-action-mailer \
  --skip-action-mailbox \
  --skip-action-text \
  --skip-active-storage \
  --skip-action-cable \
  --skip-sprockets \
  --skip-spring \
  --skip-turbolinks \
  --skip-webpack-install \
  --skip-test \
  --skip-system-test
```

## setup RSpec

add `rspec-rails` into Gemfile

```Gemfile
group :development, :test do
  # (..)
  gem 'rspec-rails'
end
```

setup rspec
```
$ bin/rails generate rspec:install
```

generate rspec binstub
```
bundle binstubs rspec-core
```
