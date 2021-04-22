# Kubernetes on docker-for-desctop

* for Mac
* 開発環境用
* ~/opt/var/lib 以下にmysql, redisのデータフォルダをマウント
* volumeのyaml適用時にenvsubstでホームディレクトリを置換するためenvsubst必須
  ない場合は*-volume.yamlのpathを書き換える

### install envsubst

gettextを入れると一緒にはいるらしい

```
brew install gettext
brew link --force gettext
```

## サービスの登録

* ローカルから接続する場合はnodePortに指定したポートから接続する

### mysql5.7

```
envsubst < mysql-5-7-volume.yaml | kubectl apply -f -
kubectl apply -f mysql-5-7-svc.yaml
kubectl apply -f mysql-5-7-deploy.yaml
```

local接続例

```
mysql -u root -h 127.0.0.1 -P 3306
```

### redis

```
envsubst < redis-volume.yaml | kubectl apply -f -
kubectl apply -f redis-svc.yaml
kubectl apply -f redis-deploy.yaml
```

### memcache

```
kubectl apply -f memcache-svc.yaml
kubectl apply -f memcache-deploy.yaml
```

### web-tools

```
kubectl apply -f adminer-svc.yaml
kubectl apply -f adminer-deploy.yaml
kubectl apply -f maildev-svc.yaml
kubectl apply -f maildev-deploy.yaml
```

#### adminer

```
open http://localhost:8080
```

#### maildev

```
open http://localhost:8025
```

* client setting

```.env
export SMTP_PORT=1025
```

```ruby
Rails.application.configure do
  config.action_mailer.delivery_method = :smtp
  config.action_mailer.smtp_settings = {
    enable_starttls_auto: false,
    address: ENV.fetch('SMTP_ADDRESS') { '127.0.0.1' },
    port:    ENV.fetch('SMTP_PORT') { 25 },
  }
end
```

#### registry

```
envsubst < registry-volume.yaml | kubectl apply -f -
kubectl apply -f registry-svc.yaml
kubectl apply -f registry-deploy.yaml
```

* Docker for Mac setting

Added "0.0.0.0:5000" insecure registry in the Docker for Mac preferences 'Daemon' tab

* docker build

```
docker build --cache-from=0.0.0.0:5000/[repository]/[image]:[tag] -t 0.0.0.0:5000/[repository]/[image]:[tag] .
```

* docker push

```
docker push 0.0.0.0:5000/[repository]/[image]:[tag]
```

## Dashboardを起動する

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/alternative/kubernetes-dashboard.yaml
```

```
kubectl -n kube-system edit service kubernetes-dashboard
```


```
open http://localhost:31665
```
