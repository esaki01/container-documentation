# Docker Basic
Dockerの基礎知識やコマンドをメモしていく.

# docker image build - イメージのビルド
`docker image build`は、DockerfileをもとにDockerイメージを作成する.

```
docker image build -t 名前空間/イメージ名:タグ名 Dockerfile配置ディレクトリパス
```

```
ex) docker image build -t ch02/ex1/echo:latest ./ch02/ex1_echo
```

実行時引数ARGに値を渡す例
```
ex) docker image build --build-arg builddate=today -t ch02/ex3/others:latest ./ch02/ex3_others
```

オプション
- -f: Dockefile以外の名前のDockerfileを探しにいく.

- --pull: `--pull=true`でビルド時にローカルのキャッシュからではなく、リモートの最新版を参照する.

# docker search - イメージの検索
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
ex) docker image tag ch02/ex1/echo:latest example/echo:0.1.0
```

# docker image push - イメージの公開
保持しているDockerイメージをDocker Hubなどのレジストリに登録する.

```
docker image push [options] リポジトリ名:タグ名
```

```
ex) docker image push esaki01/echo:latest
```

# docker image prune - イメージの破棄
不要なイメージを一括削除する.

```
docker image prune [options]
```

# docker container run - コンテナの作成と実行
Dockerイメージからコンテナを作成、実行する.

```
docker container run [options] イメージ名:タグ名 コマンド コマンド引数
```

```
docker container run [options] イメージID コマンド コマンド引数
```

```
ex) docker container run ch02/ex1/echo:latest
```

バックグラウンド実行する
```
ex) docker container run -d ch02/ex1/echo:latest
```

ENTRYPOINTを利用している場合
```
ex) docker container run ch02/ex2/golang:latest version
```

ポートフォワーディング

ホストマシンのポートをコンテナポートに紐付け、コンテナの外から来た通信をコンテナポートに転送する機能.  
ホスト側ポートは省略でき、`docker container ls`の結果からPORTSで確認できる.
```
docker container run -d -p {ホスト側ポート}:{コンテナポート} 名前空間/イメージ名:タグ名
```

```
ex) docker container run -d -p 9000:8080 ch02/ex1/echo:latest
```

コンテナに名前をつける

```
docker container run --name コンテナ名 イメージ名:タグ名
```

```
ex) docker container run -t -d --name esaki01-echo ch02/ex1/echo:latest
```

頻出オプション

- -it: シェルに入ってコマンド実行を可能にする

- -rm: コンテナ終了時にコンテナを破棄する

- -v: ホストとコンテナ間でディレクトリ、ファイルを共有する

# docker container ls - コンテナの一覧
実行中や終了したコンテナの一覧を表示する.

```
docker container ls [options]
```

```
ex) docker container ls
```

オプション

- -q: コンテナIDだけを抽出する

- --filter: 特定の条件に一致するものだを抽出する

- -a: 終了したコンテナを取得する

# docker container stop - コンテナの停止
実行しているコンテナを終了する.

```
docker container stop コンテナIDまたはコンテナ名
```

```
ex) docker container stop $(docker container ls -q)
```

特定のコンテナのみを止める
```
ex) docker container stop $(docker container ls --filter "ancestor=ch02/ex1/echo" -q)
```

# docker container restart - コンテナの再起動
コンテナを再実行する.

```
docker container restart コンテナIDまたはコンテナ名
```

```
ex) docker container restart esaki01-echo
```

# docker container rm - コンテナの破棄
停止したコンテナをディスクから完全に破棄する.

```
docker container rm コンテナIDまたはコンテナ名
```

```
ex) docker container rm esaki01-echo
```

# docker container logs - 標準出力の取得
Dockerコンテナの標準出力を表示する.

```
docker container logs [options] コンテナIDまたはコンテナ名
```

```
ex) docker container logs -f esaki01-echo
```

オプション

- -f: 標準出力の取得をし続ける

# docker container exec - 実行中コンテナでのコマンド実行
実行しているDockerコンテナの中で任意のコマンドを実行できる.

```
docker container exec [options] コンテナIDまたはコンテナ名 コンテナ内で実行するコマンド
```

```
ex) docker container exec -it esaki01-echo sh
```

# docker container cp - ファイルのコピー
コンテナ間、コンテナ・ホスト間でファイルをコピーできる. DockerのCOPYはイメージビルド時にホストからファイルをコピーするために利用されるが、`docker container cp`は実行中のコンテナ間でのファイルのやり取りをする. コンテナの中で生成されたファイルをホストにコピーして確認するようなデバッグ用途でのユースケースが代表的.

```
docker container cp [options] コンテナIDまたはコンテナ名:コンテナ内のコピー元 ホストのコピー先
```

```
docker container cp [options] ホストのコピー元 コンテナIDまたはコンテナ名:コンテナ内のコピー先
```

# docker container prune - コンテナの破棄
不要なコンテナを一括削除する.

```
docker container prune [options]
```

# docker system prune - リソースの一括削除
利用されていないコンテナやイメージ、ボリューム、ネットワークといった全てのリソースを一括で削除する.

```
docker system prune
```

# docker container stats - 利用状況の取得
コンテナ単位でのシステムリソースの利用状況を取得する.

```
docker container stats [options] 表示するコンテナID...
```
