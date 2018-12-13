# local development docker-compose.yml

## preparation

* command alias

```bash
vi ~/.zshrc

alias fig=docker-compose
```

* create external network

```bash
$ docker network create skynet
```

## containers

* database

```bash
$ cd database
$ fig up -d postgres
$ fig up -d mysql_5_6
$ fig up -d mysql_5_7
$ fig up -d mysql_8_0
$ fig up -d redis
$ fig up -d mongo
```

| db             | name      | local fowarding port | user | password |
|:---------------|:----------|---------------------:|:-----|:---------|
| postgres 9.6.3 | postgres  | 5432                 | root |          |
| mysql 5.7      | mysql_5_7 | 3307                 | root |          |
| mysql 5.6      | mysql_5_6 | 3306                 | root |          |
| mysql 8.0      | mysql_8_0 | 3380                 | root |          |
| Redis 4.*      | redis     | 6379                 | -    |          |
| MongoDB 3.4    | mongo     | 27017                | -    |          |

各dbのデータは ~/opt/var/lib 以下に保存される

* web-tool

```bash
$ cd web-tool
$ fig up -d adminer
$ fig up -d maildev
$ fig up -d redis-commander
$ fig up -d reddie
```

| name            | url                    | remarks       |
|:----------------|:-----------------------|:--------------|
| adminer         | http://localhost:8080  |               |
| maildev         | http://localhost:1080  | smtp port: 25 |
| redis-commander | http://localhost:8081  |               |
| reddie          | https://localhost:8088 |               |

* adminer, redis-commander, reddieの各host設定はサービス名で名前解決ができる

ex) adminerでmysql5.7に接続する場合

```
サーバ:     mysql_5_7
ユーザ名:   root
パスワード: <empty>
```
