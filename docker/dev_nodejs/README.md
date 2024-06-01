# Dockerコンテナ`dev_nodejs`


## 準備
下記ファイルを，コピペして適宜設定値変更する．

####  `.env`
```
SHARED_WORKSPACE_HOST=workspace
SHARED_WORKSPACE_CONTAINER=/opt/workspace
# Dockerコンテナ起動時に使用するツールがあるディレクトリ
# DOCKER_TOOLS_HOST=docker/dev_nodejs/tools

APPNAME=dev_nodejs
NODE_VERSION=20.13.1
```
- `SHARED_WORKSPACE_HOST`：Dockerコンテナがマウントするホスト側のディレクトリ
    - 基本変更不要
    - 例：`workspace`
- `SHARED_WORKSPACE_CONTAINER`：Dockerコンテナがマウントするコンテナ側のディレクトリ
    - 基本変更不要
    - 例：`/opt/workspace`
- `DOCKER_TOOLS_HOST`：Dockerイメージビルド時に使用するツールがある場合に使用するディレクトリ(ホスト・コンテナ共通)
    - 今回は使わないのでコメントアウト
- `APPNAME`：コンテナ名・イメージ名に使用する名称
    - 基本変更不要
- `NODE_VERSION`：NodeJSのバージョン
    - 公式のStable/Newest：https://nodejs.org/ja/download/
    - DockerにおけるNodeJSイメージ一覧：https://hub.docker.com/_/node
    - NodeJSのバージョン一覧：https://nodejs.org/download/release/

#### `.dockerignore`
```
**/node_modules
**/dist
**/outputs
**/.git
```
- 基本上記のコピペでOK

#### `docker-compose.yml`
```
version: "3"

services:
  dev_nodejs:
    build:
      context: .
      dockerfile: ./docker/dev_nodejs/Dockerfile
      args:
        APPNAME: ${APPNAME_DEV}
        SHARED_WORKSPACE_HOST: ${SHARED_WORKSPACE_HOST_DEV}
        SHARED_WORKSPACE_CONTAINER: ${SHARED_WORKSPACE_CONTAINER_DEV}
        # DOCKER_TOOLS_HOST: ${DOCKER_TOOLS_HOST_DEV}
        NODE_VERSION: ${NODE_VERSION}
    image: ${APPNAME_DEV}
    container_name: ${APPNAME_DEV}
    ports:
      # フロントエンドlocalhost用
      - 8001:8001
      # Storybook用
      - 8106:8106
    volumes:
      - ./${SHARED_WORKSPACE_HOST_DEV}:${SHARED_WORKSPACE_CONTAINER_DEV}
    tty: true
```
- 基本上記のコピペでOK
- `ports`のポート番号は必要あれば変更
    - 変更しなくても`npm run dev -- --host 0.0.0.0 --port 8001`のようにアプリ側でポート番号を変更することも可能


## 起動
※`$ ～`はホストで実行するコマンド，`# ～`はコンテナで実行するコマンド．

1. ソースの用意
    ```
    $ cd docker_sandbox/workspace/
    $ git clone {任意のレポジトリ}
    $ git checkout -b {任意のブランチ} origin/{任意のブランチ}
    $ git checkout -b {任意のブランチ}
    ```
1. コンテナのビルド＆起動
    ```
    $ cd ..
    $ docker-compose up -d
    ```
1. コンテナ(のターミナル)に入る
    ```
    $ docker exec -it dev_nodejs bash
    ```
1. NodeJSのコマンドの動作確認
    ```
    # node --version
    # npm --version
    # npx --version
    ```
1. 必要あればアプリディレクトリまでcdコマンドで移動して開発
    - localhostでのViteのテストサーバの実行例下記．
        ```
        # npm install
        # npm run dev -- --host 0.0.0.0 --port 8001
        ```
- コンテナ停止
    ```
    $ docker-compose stop
    ```
    - コンテナ再始動
        ```
        $ docker-compose start
        ```
        - ただしコンテナに必要なnetworkやvolumeが削除されたなどすると起動できないので，`docker-compose up -d`を再度やる必要あり．
- コンテナ削除
    ```
    $ docker-compose down
    $ docker rmi dev_nodejs
    ```
    - Dockerコンテナの削除が難しい場合はdockerコマンドで個別に削除する．
        ```
        $ docker rm {コンテナ名|コンテナID}
        ```
