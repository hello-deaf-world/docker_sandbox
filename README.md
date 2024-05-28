# docker_sandbox
Dockerコンテナを使った開発等ができるように整備する汎用レポジトリ．


## 構成
```
├ docker_sandbox/
    ├ docker/
        ├ {サービス名}/
            ├ Dockerfile
            ├ README.md [`docker-compose.yml`・`.env`・`.dockerignore`のサンプル，起動手順を記載]
        ├ dev_nodejs/ [例：NodeJS(バージョン選択可)開発環境]
            ├ Dockerfile
            ├ README.md
        ├ prd_centos/ [例：CentOS(バージョン選択可)本番想定サーバ]
            ├ Dockerfile
            ├ README.md
        ├ postgresql/ [例：PostgreSQL(バージョン選択可)]
            ├ Dockerfile
            ├ README.md
    ├ workspace/
        ├ README.md
        ├ [git cloneするなどしてソースを用意する]
    ├ docker-compose.yml
    ├ .env
```


## 導入
- 基本的にmainブランチにあるDocker環境整備ツールを使うこと
