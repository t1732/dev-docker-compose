# kubernetes apply by Helm

### prepare

```
brew install gettext
brew link --force gettext
brew instal helm
```

## アプリのインストール

### mysql 5.7

* volumeはローカルの指定フォルダ(~/opt/varlib/mysql_5.7)に配置したいため、予めpv,pvcを作成しておく

```
envsubst < ../k8s/mysql-5-7-volume.yaml | kubectl apply -f -
```

* install

```
helm install mysql-5-7 -f values/mysql-5.7.yml bitnami/mysql 
```

### redis 5.0

* add repo

```
helm repo add bitnami https://charts.bitnami.com/bitnami
```

* install

```
helm install redis-5 -f values/redis-latest.yml bitnami/redis
```
