FROM strues/alpine-base
MAINTAINER Steven Truesdell <steven@strues.io>
# Set default node, npm versions and timezone
ARG BUILD_NODE_VERSION=7.3.0
ARG BUILD_NPM_VERSION=3
ARG BUILD_TIMEZONE=America/Denver

# Set environment variables
ENV NODE_VERSION=${BUILD_NODE_VERSION}
ENV NPM_VERSION=${BUILD_NPM_VERSION}
ENV TIMEZONE=${BUILD_TIMEZONE}
ENV PKGNAME=graphicsmagick
ENV PKGVER=1.3.25
ENV PKGSOURCE=http://downloads.sourceforge.net/$PKGNAME/$PKGNAME/$PKGVER/GraphicsMagick-$PKGVER.tar.lz
# Let's roll
RUN apk upgrade --update \
  && apk add --update tzdata \
  && cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
  && echo "${TIMEZONE}" > /etc/timezone \
  && apk add curl bash --virtual build-deps \
                       make gcc g++ python paxctl \
                       musl-dev openssl-dev zlib-dev \
                       linux-headers binutils-gold \
                       wget ffmpeg libjpeg-turbo-dev \
                       libpng-dev libtool lzip libgomp \
    && \
    wget $PKGSOURCE && \
    lzip -d -c GraphicsMagick-$PKGVER.tar.lz | tar -xvf - && \
    cd GraphicsMagick-$PKGVER && \
    ./configure \
      --build=$CBUILD \
      --host=$CHOST \
      --prefix=/usr \
      --sysconfdir=/etc \
      --mandir=/usr/share/man \
      --infodir=/usr/share/info \
      --localstatedir=/var \
      --enable-shared \
      --disable-static \
      --with-modules \
      --with-threads \
      --with-gs-font-dir=/usr/share/fonts/Type1 \
      --with-quantum-depth=16 \
    && make -j$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) \
    && make install \
    && cd / \
    && rm -rf GraphicsMagick-$PKGVER \
    && rm GraphicsMagick-$PKGVER.tar.lz \
  && mkdir -p /root/nodejs \
  && cd /root/nodejs \
  && curl -sSL -o node-${NODE_VERSION}.tar.gz http://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}.tar.gz \
  && curl -o SHASUMS256.txt.asc -sSL https://nodejs.org/dist/v${NODE_VERSION}/SHASUMS256.txt.asc \
  && grep node-${NODE_VERSION}.tar.gz SHASUMS256.txt.asc | sha256sum -c - \
  && tar -zxf node-${NODE_VERSION}.tar.gz \
  && cd node-v${NODE_VERSION} \
  && ./configure \
    --prefix=/usr \
    --without-snapshot \
  && make -j$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) \
  && make install \
  && paxctl -cm /usr/bin/node \
  && npm cache clean \
  && apk del build-deps \
  && apk add \
    bash curl \
  && rm -rf /usr/share/man /var/cache/apk/* \
    /root/.npm /root/.node-gyp /usr/lib/node_modules/npm/man \
    /usr/lib/node_modules/npm/doc /usr/lib/node_modules/npm/html \
  && npm i -g npm@latest \
  && npm cache clean \
  && rm -rf /root/nodejs
