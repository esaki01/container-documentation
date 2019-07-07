# Docker Swarm（クラスタの構築や管理を担う（主にマルチホスト））
## docker swarm init - managerの設定（Swarmモードの有効化）
クラスタを管理する役割を担うmanagerを設定する. `docker swarm init`の実行時にJOINトークンが発行され標準出力に表示される. DockerホストをSwarmクラスタのworkerとして登録するにはこのJOINトークンが必要になる.

```
ex) docker container exec -it manager docker swarm init
```

## docker swarm join - ノードの登録
Swarmクラスタにノードをworkerとして登録する. JOINトークンが必要.

```
ex) docker container exec -it worker01 docker swarm join --token xxx
```

## docker node ls - ノードの確認
Swarmクラスタの状態がどうなっているのかを確認する.

```
ex) docker container exec -it manager docker node ls
```

## docker image push - registryコンテナへイメージを登録
ホスト上でビルドしたDockerイメージをregistryコンテナに登録する.

```
docker image push push先のレジストリのホスト/リポジトリ名:タグ名
```

上記の書式に合わせるために、`docker image tag`を使う.

```
ex) docker image tag ch02/ex1/echo:latest localhost:5000/ch02/ex1/echo:latest
```

```
ex) docker image push localhost:5000/ch02/ex1/echo:latest
```

## docker image pull - registryコンテナからイメージを取得
Dockerホストにregistryコンテナからイメージを取得する.

```
docker image pull pull先のレジストリのホスト/リポジトリ名:タグ名
```

レジストリはホスト側からはlocalhost:5000でアクセスできたが、Dockerホストからはregistry:5000でアクセスできる.
```
ex) docker container exec -it worker01 docker image pull registry:5000/ch02/ex1/echo:latest
```

# Docker Service（クラスタ内のService（1つ以上のコンテナの集まり）を管理する）
アプリケーションを構成する一部のコンテナを制御するための単位. 1つのアプリケーションイメージを扱う（Dockerfileと同じ粒度？）.

## docker service create - Serviceの作成
managerコンテナ内からサービスを作成する.

```
ex) docker container exec -it manager docker service create --replicas 1 --publish 8000:8080 --name echo registry:5000/ch02/ex1/echo:latest
```

## docker service ls - Serviceの一覧
Serviceの一覧を確認する.

```
ex) docker container exec -it manager docker service ls
```

## docker service scale - コンテナ数のスケール
Serviceのコンテナ数を増減することができる. `docker container run`を繰り返す必要なくスケールアウトできるので有用.

```
ex) docker container exec -it manager docker service scale echo=6
```

## docker service ps - ノードの確認
Swarmクラスタ上で実行されているコンテナを確認する.

```
ex) docker container exec -it manager docker service ps echo | grep Running
```

## docker service rm - サービスの削除
デプロイしたServiceを削除する.

```
ex) docker container exec -it manager docker service rm echo
```

# Docker Stack（複数のServiceをまとめたアプリケーション全体の管理を行う）
複数のサービスをグルーピングした単位（Composeと同じ粒度）. つまり、Swarm上でのスケールイン・スケールアウトが可能になったComposeという位置付け. Dockerホスト間を超えたコンテナ間通信が可能（overlayネットワーク）. 別のoverlayネットワークに存在するService間での通信は不可.

## docker network create - overlayネットワークの構築
overlayネットワークを作成し、Stackから」作成される各Serviceを所属させる.

```
ex) docker container exec -it manager docker network create --driver=overlay --attachable ch03
```

## docker stack deploy - Stackのデプロイ
新規にStackをデプロイ、または更新する.

```
docker stack deploy [options] Stack名
```

```
ex) docker container exec -it manager docker stack deploy -c /stack/ch03-webapi.yml echo
```

オプション
- -c: Stack定義ファイルのパス

## docker stack services - Stackの一覧
Stack内のService一覧を表示する.

```
docker stack services [options] Stack名
```

```
ex) docker container exec -it manager docker stack services echo
```

## docker stack ps - コンテナの一覧
Stackによってコンテナ群がどのようにデプロイされているかを確認する.

```
docker stack ps [options] Stack名
```

```
ex) docker container exec -it manager docker stack ps echo
```

## docker stack rm - Stackの削除

```
ex) docker container exec -it manager docker stack rm echo
```

# まとめ
- Serviceはレプリカ数（コンテナの数）を制御することで容易にコンテナを複製でき、複数のノードに配置できるためスケールアウトへの親和性が高い
- Serviceによって管理される複数のレプリカはService名で解決でき、かつServiceへのトラフィックはレプリカへ分散される
- Swarmクラスタの外からSwarmのServiceを利用するには、Serviceにトラフィックを分散するためのプロキシを用意する
- Stackは複数のServiceをグルーピングでき、複数のServiceで形成されるアプリケーションのデプロイに役立つ
