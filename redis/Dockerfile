FROM strues/alpine-base
MAINTAINER Steven Truesdell <steven@strues.io>

ENV REDIS_VERSION=3.2.3 \
    REDIS_USER=redis \
    REDIS_DATA_DIR=/var/lib/redis \
    REDIS_LOG_DIR=/var/log/redis \
    REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-3.2.3.tar.gz

RUN build_pkgs="gcc linux-headers make musl-dev" \
  && runtime_pkgs="su-exec>=0.2" \
  && apk --update add ${build_pkgs} ${runtime_pkgs} \
  && addgroup -S redis \
  && adduser -S -G ${REDIS_USER} ${REDIS_USER} \
  && mkdir -p /tmp/src \
  && cd /tmp/src \
  && wget -cq ${REDIS_DOWNLOAD_URL} -O /tmp/src/redis-${REDIS_VERSION}.tar.gz \
  && tar -zxf /tmp/src/redis-${REDIS_VERSION}.tar.gz \
  && cd /tmp/src/redis-${REDIS_VERSION} \
  && make \
  && make install \
  && mkdir -p /etc/redis/

COPY redis.conf /etc/redis/redis.conf

RUN \
    sed 's/^daemonize yes/daemonize no/' -i /etc/redis/redis.conf \
    && sed 's/^bind 127.0.0.1/bind 0.0.0.0/' -i /etc/redis/redis.conf \
    && sed 's/^# unixsocket \/tmp\/redis.sock/unixsocket \/run\/redis\/redis.sock/' -i /etc/redis/redis.conf \
    && sed 's/^# unixsocketperm 700/unixsocketperm 777/' -i /etc/redis/redis.conf \
    && sed '/^logfile/d' -i /etc/redis/redis.conf \
    && chown -R ${REDIS_USER}:${REDIS_USER} /etc/redis/ \
    && mkdir -p /run/redis \
    && chmod -R 0755 /run/redis \
    && chown -R ${REDIS_USER}:${REDIS_USER} /run/redis \
    && mkdir -p ${REDIS_LOG_DIR} \
    && chmod -R 0755 ${REDIS_LOG_DIR} \
    && chown -R ${REDIS_USER}:${REDIS_USER} ${REDIS_LOG_DIR} \
    && apk del ${BUILD_DEPENDENCIES} \
    && rm -rf /tmp/* \
    && rm -rf /var/cache/apk/*

COPY entrypoint.sh /sbin/entrypoint.sh
RUN chmod 755 /sbin/entrypoint.sh

EXPOSE 6379/tcp

VOLUME ["/var/lib/redis"]
WORKDIR /var/lib/redis
ENTRYPOINT ["/sbin/entrypoint.sh"]
