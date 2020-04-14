# TODOアプリケーション構築
## 全体像
- データストアとなるMaster/Slave構成のMySQL Serviceの構築
- MySQLとデータのやり取りをするためのAPI実装
- ウェブアプリケーションとAPI間にリバースプロキシとなるNginxを通じてアクセスできるように設定
- APIを利用してサーバーサイドレンダリングをするWebアプリケーションを実装
- フロント側にリバースプロキシ (Nginx) を置く

## overlayネットワークの構築

```
docker container exec -it manager docker network create --driver=overlay --attachable todoapp
```
