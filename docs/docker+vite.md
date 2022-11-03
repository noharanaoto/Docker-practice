## 構築前のファイル構成

~~~sh
$ tree
.
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

~~~yml
version: "3.8" 
services:
  front: 
    build: 
      context: . 
      dockerfile: ./Dockerfile
      args:
        - UID=501
        - GID=20
    container_name: node
    volumes:
      - type: bind
        source: .
        target: /front
    working_dir: /front
    command: sh -c "npm run dev"
    ports:
      - 8000:8000
    tty: true
    stdin_open: true
    environment:
      - CHOKIDAR_USEPOLLING=true
~~~

## やること

~~~
$ docker-compose build
~~~

<details>
<summary>ログ</summary>

~~~sh
noharanaoto@noharanaotonoMacBook-Pro docker-vite % docker-compose build
Building front
[+] Building 11.1s (10/10) FINISHED                                                                                                                           
 => [internal] load build definition from Dockerfile                                                                                                     0.1s
 => => transferring dockerfile: 203B                                                                                                                     0.0s
 => [internal] load .dockerignore                                                                                                                        0.0s
 => => transferring context: 2B                                                                                                                          0.0s
 => [internal] load metadata for docker.io/library/node:16.13.0-alpine3.12                                                                               3.3s
 => [internal] load build context                                                                                                                        0.0s
 => => transferring context: 660B                                                                                                                        0.0s
 => [1/5] FROM docker.io/library/node:16.13.0-alpine3.12@sha256:36ff9819642402d43cbd1304c888f8c0fb398c511299e6212ce8f3b6ac03ff9b                         6.8s
 => => resolve docker.io/library/node:16.13.0-alpine3.12@sha256:36ff9819642402d43cbd1304c888f8c0fb398c511299e6212ce8f3b6ac03ff9b                         0.0s
 => => sha256:37052833c7b030aafc773cfb8bd752af5086798b99ed82a9372796ba1847d2a0 34.78MB / 34.78MB                                                         4.6s
 => => sha256:da9436c612e4301f474bf6c2e4abf42336b5c064222c3ea0b69baf4fcfe09ea0 2.35MB / 2.35MB                                                           0.9s
 => => sha256:36ff9819642402d43cbd1304c888f8c0fb398c511299e6212ce8f3b6ac03ff9b 1.43kB / 1.43kB                                                           0.0s
 => => sha256:a21cc07a7dd15d9960d7c771565b8eb6d48aebefe067429f2a170c0c8e1cab07 1.16kB / 1.16kB                                                           0.0s
 => => sha256:d951a5a0fae5d7e7c0dd31613786e4607ac8cef1e3934bc0e422e2262170c527 6.53kB / 6.53kB                                                           0.0s
 => => sha256:8572bc8fb8a32061648dd183b2c0451c82be1bd053a4ea8fae991436b92faebb 2.81MB / 2.81MB                                                           0.4s
 => => extracting sha256:8572bc8fb8a32061648dd183b2c0451c82be1bd053a4ea8fae991436b92faebb                                                                0.7s
 => => sha256:916005d8c1b3c8a6d15a703eb96f70603f39446d38cb2a19c65d8b6bc4946e56 450B / 450B                                                               1.0s
 => => extracting sha256:37052833c7b030aafc773cfb8bd752af5086798b99ed82a9372796ba1847d2a0                                                                1.7s
 => => extracting sha256:da9436c612e4301f474bf6c2e4abf42336b5c064222c3ea0b69baf4fcfe09ea0                                                                0.1s
 => => extracting sha256:916005d8c1b3c8a6d15a703eb96f70603f39446d38cb2a19c65d8b6bc4946e56                                                                0.0s
 => [2/5] WORKDIR /front                                                                                                                                 0.2s
 => [3/5] COPY . /front                                                                                                                                  0.0s
 => [4/5] RUN mkdir /.npm                                                                                                                                0.3s
 => [5/5] RUN chown -R 501:20 "/.npm"                                                                                                                    0.2s
 => exporting to image                                                                                                                                   0.0s
 => => exporting layers                                                                                                                                  0.0s
 => => writing image sha256:6832444b01a9161e9e4dadf6a638e75e9f1891209bc2b334bf54c1d17e4d275e                                                             0.0s
 => => naming to docker.io/library/docker-vite_front                                                                                                     0.0s

Use 'docker scan' to run Snyk tests against images to find vulnerabilities and learn how to fix them
noharanaoto@noharanaotonoMacBook-Pro docker-vite % 
~~~

</details>

~~~
$ docker-compose run --rm --user 501:20 front npm create vite@latest
~~~

<details>
<summary>ログ</summary>

~~~sh
noharanaoto@noharanaotonoMacBook-Pro docker-vite % docker-compose run --rm --user 501:20 front npm create vite@latest
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
noharanaoto@noharanaotonoMacBook-Pro docker-vite % tree
.
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

2 directories, 9 files
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
$ docker-compose run --rm --user 501:20 front npm install
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

