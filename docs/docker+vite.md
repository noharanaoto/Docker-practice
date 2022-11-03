## 構築前のファイル構成

~~~sh
$ tree
.
├── .env
├── Dockerfile
└── docker-compose.yml
~~~

## ファイルの中身

Dockerfile

~~~Docker
FROM node:16.13.0-alpine3.12
ENV NODE_VERSION 14.18.1
WORKDIR /front
COPY . /front

ARG UID
ARG GID
RUN mkdir /.npm
RUN chown -R $UID:$GID "/.npm"

EXPOSE 8000
~~~

docker-compose.yml
.envを読み込む時に

~~~
ERROR: The Compose file './docker-compose.yml' is invalid because:
services.front.build.args contains {"UID": "xxxx"}, which is an invalid type, it should be a string
~~~

で詰まった→[解決方法](https://qiita.com/rinasan3/items/68ffa55a9cfaef27da6b9)

~~~yml
version: "3.8" 
services:
  front: 
    build: 
      context: . 
      dockerfile: ./Dockerfile
      args:
        UID: ${UID}
        GID: ${GID}
    container_name: node
    volumes:
      - type: bind
        source: .
        target: /front
    working_dir: /front
    command: sh -c "npm run dev"
    env_file:
      - ./.env
    ports:
      - 8000:8000
    tty: true
    stdin_open: true
    environment:
      - CHOKIDAR_USEPOLLING=true
~~~

.env
UIDとGIDは自分のものを入力。調べ方は[こちら](https://atmarkit.itmedia.co.jp/flinux/rensai/linuxtips/095uidgid.html)

~~~
UID=xxxx
GID=xxxx
~~~

## やること

~~~
$ docker-compose build
~~~

~~~
$ docker-compose run --rm --user 自分のUID:自分のGID front npm create vite@latest
~~~

<details>
<summary>ログ</summary>

~~~sh
$ docker-compose run --rm --user 自分のUID:自分のGID front npm create vite@latest
Creating network "docker-vite_default" with the default driver
Creating docker-vite_front_run ... done
Need to install the following packages:
  create-vite@latest
Ok to proceed? (y) y
✔ Project name: … vite-project
✔ Select a framework: › Vanilla
✔ Select a variant: › JavaScript

Scaffolding project in /front/vite-project...

Done. Now run:

  cd vite-project
  npm install
  npm run dev

npm notice 
npm notice New minor version of npm available! 8.1.0 -> 8.19.2
npm notice Changelog: https://github.com/npm/cli/releases/tag/v8.19.2
npm notice Run npm install -g npm@8.19.2 to update!
npm notice 
~~~

</details>

ここでのファイル構成

~~~
$ tree
.
├── .env
├── Dockerfile
├── docker-compose.yml
└── vite-project
    ├── counter.js
    ├── index.html
    ├── javascript.svg
    ├── main.js
    ├── package.json
    ├── public
    │   └── vite.svg
    └── style.css
~~~

ファイル構成を変更

~~~
$ mv vite-project/* vite-project/.gitignore .
$ rm -rf vite-project
$ touch vite.config.js
~~~

vite.config.jsは

~~~js
import { defineConfig } from 'vite';

export default defineConfig({
  server: {
    host: true,
    port: 8000,
  },
});
~~~

runする

~~~
$ docker-compose run --rm --user 自分のUID:自分のGID front npm install
~~~

<details>
<summary>ログ</summary>

~~~
Creating docker-vite_front_run ... done

added 14 packages, and audited 15 packages in 6s

4 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
npm notice 
npm notice New minor version of npm available! 8.1.0 -> 8.19.2
npm notice Changelog: https://github.com/npm/cli/releases/tag/v8.19.2
npm notice Run npm install -g npm@8.19.2 to update!
npm notice 
~~~

</details>

問題なければ

~~~
$ docker-compose up -d
~~~

して`http://localhost:8000/`にアクセスするとwelcomeページが表示される。

![welcom画面](images/vite_welcome_page.png)

## hello worldしてみる

ディレクトリ構成変えて

~~~
$ mkdir -p css/scss/ img/ js/
$ rm -rf public/ counter.js javascript.svg main.js style.css
~~~

index.html

~~~html
<!DOCTYPE html>
<html lang="ja">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>hello world</title>
  </head>
  <body>
    <p>vite hello world!</p>
  </body>
</html>
~~~

![hello_world](images/vite_hello_world.png)
