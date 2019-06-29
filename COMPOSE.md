# Docker Compose
Docker Composeについてメモしていく.

# docker-compose up - コンテナ群の起動
docker-compose.ymlを作成したディレクトリで、定義をもとにコンテナ群を起動する.

```
ex) docker-compose up -d
```

オプション

- --build: `docker-compose up`の際に必ずビルドする

# docker-compose down - コンテナ群の停止
コンテナ群を停止する.

```
docker-compose down
```
