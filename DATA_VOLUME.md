# Data Volume
Data Volumeについてメモしていく.

# Data Volumeの作成
Dockerコンテナ内のディレクトリをディスクに永続化する.

```
docker container run [options] -v ホスト側ディレクトリ:コンテナ側ディレクトリ リポジトリ名:タグ名 コマンド コマンド引数
```

```
ex) docker container run -v ${PWD}:/workspace gihyodocker/imagemagick:latest convert -size 100x100 xc:#000000 /workspace/gihyo.jpg
```

# Data Volumeコンテナ
コンテナ間でディレクトリを共有する、データを持つためだけのコンテナ. Data VolumeコンテナのVolumeはDockerの管理領域であるホスト側の`/var/lib/docker/volumes/`以下に配置される.
