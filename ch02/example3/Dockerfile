# その他のインストラクションを使用した書き方

FROM alpine:3.7
# LABELはイメージの作者名記入などに使う
LABEL maintainer="dockertaro@example.com"

# イメージビルドの時だけ使用できる一時的な環境変数
ARG builddate
# Dockerコンテナ内で使える環境変数
ENV BUILDDATE=${builddate}

ENV BUILDFROM="from Alpane"

ENTRYPOINT ["/bin/ash", "-c"]
CMD ["env"]
