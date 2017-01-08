FROM alpine:3.5
MAINTAINER Steven Truesdell <steven@strues.io>

RUN \
  apk update && \
  apk add bash curl rsync supervisor && \
  chmod -R 0777 /var/log /run

COPY files /

ENV SUPERVISORD_LOGLEVEL=info

ENTRYPOINT ["/bin/bash", "-e", "/init/entrypoint"]
CMD ["run"]
