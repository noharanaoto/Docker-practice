FROM node:16.13.0-alpine3.12
ENV NODE_VERSION 14.18.1
WORKDIR /front
COPY . /front

ARG UID
ARG GID
RUN mkdir /.npm
RUN chown -R $UID:$GID "/.npm"

EXPOSE 8000