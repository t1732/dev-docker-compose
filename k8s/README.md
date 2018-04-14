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
mysql -u root -h 127.0.0.1 -P 32307
```

### redis

```
envsubst < redis-volume.yaml | kubectl apply -f -
kubectl apply -f redis-svc.yaml
kubectl apply -f redis-deploy.yaml
```

## Dashboardを起動する

```
kubectl create -f https://raw.githubusercontent.com/kubernetes/dashboard/master/src/deploy/recommended/kubernetes-dashboard.yaml
```

```
kubectl -n kube-system edit service kubernetes-dashboard
```

```
  spec:
    ports:
      - port: 443
        protocol: TCP
        targetPort: 8443
+       nodePort: 30000
- type: ClusterIP
+ type: NodePort
```

```
open https://localhost:30000
```

* ログインはSKIPでOK
