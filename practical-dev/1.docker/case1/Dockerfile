# 基本的な書き方

# Dockerイメージのベースとなるイメージ. Docker Hubというレジストリを参照する
FROM golang:1.9

# Dockerイメージビルド時にDockerコンテナ内で実行するコマンドを定義する
RUN mkdir /echo
# Dockerを動作させているホストマシン上のファイルやディレクトリをDockerコンテナ内にコピーする
COPY main.go /echo

# Dockerコンテナとして実行する際に、コンテナ内で実行するプロセスを指定する
# シェルで書くと、「go run /echo/main.go」のような実行の仕方になる
CMD ["go", "run", "/echo/main.go"]
