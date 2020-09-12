# Development environment construction(Rails6 + Postgres)

## Setup development

1. リポジトリをクローンして、新規 rails プロジェクトを作成
2. `docker-compose build`で Gemfile から`bundle install`する

```sh
mkdir app_name
cd app_name
git clone this repository
docker-compose run web rails new . --force --no-deps --database=postgresql --skip-bundle
docker-compose build
```

3. database.yml を以下のように変更

```database.yml
default: &default
  adapter: postgresql
  encoding: unicode
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  host: db
  username: postgres
  passowrd:

development:
  <<: *default
  database: myapp_development

test:
  <<: *default
  database: myapp_test

production:
  <<: *default
  database: myapp_production
  username: myapp
  password: <%= ENV['MYAPP_DATABASE_PASSWORD'] %>
```

4. コンテナの起動を確認後、DB を作成

```
# 起動を確認
docker compose up
docker-compose down

# dbを作成
docker-compose run web rake db:create
docker-compose up
```

5. `http://localhost:3000`への接続を確認
