# Rails 7 x MySQL

- Rails v7.0.4
- API mode
- DB: MySQL



```
$ rbenv local 3.1.2
$ bundle init
$ vim Gemfile
// commentin rails
$ touch Gemfile.lock
```

Dockerfile
```Dockerfile
FROM ruby:3.1

RUN mkdir /myapp
WORKDIR /myapp
COPY Gemfile /myapp/Gemfile
COPY Gemfile.lock /myapp/Gemfile.lock
RUN bundle install
COPY . /myapp

# Add a script to be executed every time the container starts.
COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
EXPOSE 3000

# Start the main process.
CMD ["rails", "server", "-b", "0.0.0.0"]
```

entrypoint.sh
```entrypoint.sh
#!/bin/bash
set -e

# Remove a potentially pre-existing server.pid for Rails.
rm -f /myapp/tmp/pids/server.pid

# Then exec the container's main process (what's set as CMD in the Dockerfile).
exec "$@"
```

docker-compose.yml
```docker-compose.yml
version: '3'
services:
  db:
    image: mysql:5.7
    platform: linux/amd64
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: root
    ports:
      - "4306:3306"
    volumes:
      - ./tmp/db:/var/lib/mysql

  web:
    build: .
    command: bash -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"
    volumes:
      - .:/myapp
    ports:
      - "3000:3000"
    depends_on:
      - db
```

database.yml の 一部差し替え
```config/database.yml
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  host: <%= ENV['MYSQL_HOST'] || 'db' %>
  username: <%= ENV['MYSQL_USERNAME'] || 'root' %>
  password: <%= ENV['MYSQL_PASSWORD'] || 'password' %>
  socket: /tmp/mysql.sock```
```

```sh
$ docker-compose run web rails new . --force --no-deps --database=mysql --api
$ docker-compose run web bundle install

$ docker-compose up
$ docker-compose run web rails db:create

// if some error
$ docker-compose build --no-cache

```

### Rspec


rpsecを入れるためにGemfile修正
```Gemfile
group :development, :test do
  # See https://guides.rubyonrails.org/debugging_rails_applications.html#debugging-with-the-debug-gem
  gem "debug", platforms: %i[ mri mingw x64_mingw ]
  gem "rspec-rails" 
  gem "factory_bot_rails" 
end
```

```
$ docker-compose build --no-cache
$ docker-compose run web rails g rspec:install
$ docker-compose run web rspec         
Creating rails7-active-job_web_run ... done
No examples found.


Finished in 0.00027 seconds (files took 0.03759 seconds to load)
0 examples, 0 failures
```


- Refs.
  - https://qiita.com/croquette0212/items/7b99d9339fd773ddf20b
  - https://qiita.com/youta0/items/4bb2b5e50d3f9908f3f8
  - https://qiita.com/tmasuyama/items/5252925a1533adaab723

