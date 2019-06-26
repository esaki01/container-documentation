# Docker Command Memo
Dockerコマンドをメモしていく.

# docker image build - イメージのビルド
`docker image build`は、DockerfileをもとにDockerイメージを作成する.

```
docker image build -t 名前空間/イメージ名:タグ名 Dockerfile配置ディレクトリパス
```

```
ex) docker image build -t ch02/example1/echo:latest ./ch02/example1
```

実行時引数ARGに値を渡す例
```
ex) docker image build --build-arg builddate=today -t ch02/example3/others:latest ./ch02/example3
```

オプション
- -f
Dockefile以外の名前のDockerfileを探しにいく.

- --pull
`--pull=true`でビルド時にローカルのキャッシュからではなく、リモートの最新版を参照する.

# docker search - イメージの取得
Docker Hubでは、GitHubと同じようにリポジトリを持つことができる.
`docker search`では、Docker Hubに登録されているリポジトリを検索できる.

```
docker search [options] 検索キーワード
```

```
ex) docker search --limit 5 mysql
```

# docker image pull - イメージの取得
DockerレジストリからDockerイメージをダウンロードする.

```
docker image pull [options] リポジトリ名:タグ名
```

```
ex) docker image pull jenkins:latest
```

# docker image ls - イメージの一覧
Dockerホストに保持されているイメージの一覧を取得する.

```
ex) docker image ls
```

# docker image tag - イメージのタグ付け
Dockerイメージの特定のバージョン（IMAGE ID）にタグ付けを行う.

```
docker image tag 元イメージ名:タグ名 新イメージ名:タグ名
```

```
ex) docker image tag example/echo:latest example/echo:0.1.0
```

# docker image push - イメージの公開
保持しているDockerイメージをDocker Hubなどのレジストリに登録する.

```
docker image push [options] リポジトリ名:タグ名
```

```
ex) docker image push esaki1011/echo:latest
```

# Dockerコンテナを実行する
```
docker container run ch02/example1/echo:latest
```

バックグラウンド実行する
```
docker container run -d ch02/example1/echo:latest
```

ENTRYPOINTを利用している場合
```
docker container run ch02/example2/golang:latest version
```

ポートフォワーディング

ホストマシンのポートをコンテナポートに紐付け、コンテナの外から来た通信をコンテナポートに転送する機能.  
ホスト側ポートは省略でき、`docker container ls`の結果からPORTSで確認できる.
```
docker container run -d -p {ホスト側ポート}:{コンテナポート} 名前空間/イメージ名:タグ名
```

```
ex) docker container run -d -p 9000:8080 ch02/example1/echo:latest
```

# Dockerコンテナを確認する
```
docker container ls
```

# Dockerコンテナを止める
```
docker stop $(docker container ls -q)
```

特定のコンテナのみを止める
```
docker container stop $(docker container ls --filter "ancestor=ch02/example1/echo" -q)
```
