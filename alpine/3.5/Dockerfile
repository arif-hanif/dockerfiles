FROM alpine:3.5
MAINTAINER Steven Truesdell <steven@strues.io>

ENV LANG=en_US.UTF-8 \
    LC_ALL=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8 \
    TERM=xterm \
    S6OVERLAY_VERSION=v1.18.1.5 \
    S6_BEHAVIOUR_IF_STAGE2_FAILS=1

RUN apk update \
  && apk upgrade \
  && apk add bash \
        bind-tools \
      	ca-certificates \
      	curl \
      	tar \
  && curl -sSL https://github.com/just-containers/s6-overlay/releases/download/${S6OVERLAY_VERSION}/s6-overlay-amd64.tar.gz | tar xz -C / \
  && apk del tar \
  && rm -rf /var/cache/apk/* /src \
  && rm -rf /tmp/* \
  && rm -rf /var/log/*

COPY rootfs/ /
