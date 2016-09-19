FROM strues/alpine-base
MAINTAINER Steven Truesdell <steven@strues.io>

COPY root/. /
ENV LANG="en_US.UTF-8" \
    LC_ALL="en_US.UTF-8" \
    LANGUAGE="en_US.UTF-8" \
    MARIADB_ROOT_PASSWORD="password" \
    TERM="xterm"

RUN apk update \
    && apk upgrade \
    && apk add mariadb mariadb-client \
    && rm -rf /tmp/src \
    && rm -rf /var/cache/apk/*

VOLUME ["/data"]

EXPOSE 3306
