## helloworld

Dockerコンテナに入る

~~~
$ docker compose exec web bash
~~~

djangoのプロジェクトを作る

~~~
$ python3 manage.py startapp blog
~~~

exitでコンテナから出てファイル構成が

~~~
$ tree -a
.
├── Dockerfile
├── blog
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── migrations
│   │   └── __init__.py
│   ├── models.py
│   ├── tests.py
│   └── views.py
├── docker-compose.yml
├── manage.py
├── myproject
│   ├── __init__.py
│   ├── __pycache__
│   │   ├── __init__.cpython-310.pyc
│   │   ├── settings.cpython-310.pyc
│   │   ├── urls.cpython-310.pyc
│   │   └── wsgi.cpython-310.pyc
│   ├── asgi.py
│   ├── settings.py
│   ├── urls.py
│   └── wsgi.py
└── requirements.txt
~~~

となっていればOK！ここからhelloworldを表示します。

