## 基本
ビルド

~~~
docker-compose build
~~~

コンテナ立ち上げ

~~~
docker-compose up -d
~~~

## 一括系
全コンテナ停止

~~~
docker stop $(docker ps -q)
~~~

全コンテナ削除

~~~
docker rm $(docker ps -q -a)
~~~

全イメージ削除

~~~
docker rmi $(docker images -q)
~~~

## 参考
[コマンドでDockerコンテナを停⽌・削除、イメージの削除をする - Qiita](https://qiita.com/shisama/items/48e2eaf1dc356568b0d7)
