FROM strues/alpine-base:3.0.0
MAINTAINER Steven Truesdell <steven@strues.io>

ENV NPM_CONFIG_LOGLEVEL=info \
        NODE_VERSION=7.2.0 \
        PATH=/root/.yarn/bin:$PATH

VOLUME /root/.yarn-cache

RUN apk upgrade --update-cache \
    && apk add --virtual .build-deps \
                          gcc \
                          g++ \
                          libc-dev \
                          make \
                          python \
                          linux-headers \
                          zlib-dev \
                          paxctl \
                          binutils-gold \
                          gnupg \
                          curl \
                          icu-dev \
                          openssl-dev \
                          pax-utils \
                          tar \
    && curl -sSL https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION.tar.gz | tar -xz \
    && cd /node-v$NODE_VERSION \
    && export GYP_DEFINES="linux_use_gold_flags=0" \
    && ./configure --prefix=/usr --shared-zlib --shared-openssl \
    && NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) \
    && make out/Makefile \
    && make -j${NPROC} -C out mksnapshot \
    && paxctl -cm out/Release/mksnapshot \
    && make -j${NPROC} \
    && make install \
    && paxctl -cm /usr/bin/node \
    && cd / \
    && curl -o- -L https://yarnpkg.com/install.sh | ash \
    && runDeps="$( \
        scanelf --needed --nobanner --recursive /usr/bin/node /usr/lib/node* \
            | awk '{ gsub(/,/, "\nso:", $2); print "so:" $2 }' \
            | sort -u \
            | xargs -r apk info --installed \
            | sort -u )" \
    && apk add --virtual .node-rundeps $runDeps \
    && apk del --virtual .build-deps \
    && rm -rf \
        /node-v7.0.0 \
        /usr/src/* \
        /usr/share/doc \
        /usr/share/man \
        /usr/share/doc \
        /tmp/* \
        /var/cache/apk/* \
        /root/.npm \
        /root/.node-gyp \
        /usr/lib/node_modules/npm/man \
        /usr/lib/node_modules/npm/doc \
        /usr/lib/node_modules/npm/html

CMD [ "node" ]
