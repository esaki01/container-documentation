# Dockerコマンドのメモ

## Dockerイメージをビルドする
```
docker image build -t 名前空間/イメージ名:タグ名 Dockerfile配置ディレクトリパス
```

```
ex) docker image build -t ch02/example1/echo:latest ./ch02/example1
```

**実行時引数ARGに値を渡す**
```
docker image build --build-arg builddate=today -t ch02/example3/others:latest ./ch02/example3
```

## Dockerイメージを確認する
```
docker image ls
```

## Dockerコンテナを確認する
```
docker container ls
```

## Dockerコンテナを実行する
```
docker container run ch02/example1/echo:latest
```

**バックグラウンド実行する**
```
docker container run -d ch02/example1/echo:latest
```

**ENTRYPOINTを利用している場合**
```
docker container run ch02/example2/golang:latest version
```

**ポートフォワーディング**

ホストマシンのポートをコンテナポートに紐付け、コンテナの外から来た通信をコンテナポートに転送する機能.  
ホスト側ポートは省略でき、`docker container ls`の結果からPORTSで確認できる.
```
docker container run -d -p {ホスト側ポート}:{コンテナポート} 名前空間/イメージ名:タグ名
```

```
ex) docker container run -d -p 9000:8080 ch02/example1/echo:latest
```

## Dockerコンテナを止める
```
docker stop $(docker container ls -q)
```

**特定のコンテナのみを止める**
```
docker container stop $(docker container ls --filter "ancestor=ch02/example1/echo" -q)
```
