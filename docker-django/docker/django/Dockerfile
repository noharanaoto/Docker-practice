FROM python:3.8.2
ENV PYTHONUNBUFFERED 1
RUN mkdir /code
# ARG USERNAME
# ARG GROUPNAME
# ARG UID
# ARG GID
# RUN groupadd -g $GID $GROUPNAME && \ useradd -m -s /bin/bash -u $UID -g $GID $USERNAME
WORKDIR /code
ADD ./docker/django/requirements.txt /code/
RUN pip install -r requirements.txt
# USER $USERNAME
ADD . /code/