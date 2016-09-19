FROM strues/alpine-base
MAINTAINER Steven Truesdell <steven@strues.io>

RUN echo "http://nl.alpinelinux.org/alpine/latest-stable/main" > /etc/apk/repositories \
&& echo "http://nl.alpinelinux.org/alpine/edge/testing/" >> /etc/apk/repositories \
&& echo "http://nl.alpinelinux.org/alpine/edge/community/" >> /etc/apk/repositories \
&& echo "nameserver 8.8.8.8" >> /etc/resolv.conf && apk update && apk upgrade \
&& apk add --no-cache \
        curl \
        git \
        ssmtp \
        php7 \
        php7-fpm \
        php7-bcmath \
        php7-curl \
        php7-ctype \
        php7-dom \
        php7-gd \
        php7-iconv \
        php7-json \
        php7-mbstring \
        php7-mcrypt \
        php7-mysqli \
        php7-opcache \
        php7-openssl \
        php7-pcntl \
        php7-pdo \
        php7-pdo_mysql \
        php7-pdo_sqlite \
        php7-pdo_pgsql \
        php7-pdo_dblib \
        php7-pgsql \
        php7-pdo_odbc \
        php7-phar \
        php7-session \
        php7-sockets \
        php7-xml \
        php7-redis \
        php7-xdebug \
        php-redis

RUN rm -rf /var/cache/apk/*

# Ensure www-data user exists
RUN addgroup -g 1000 -S www-data \
	&& adduser -u 1000 -D -S -G www-data www-data

# Copy config
COPY files/php.ini /etc/php7/conf.d/50-setting.ini
COPY files/php-fpm.conf /etc/php7/php-fpm.conf
COPY files/ssmtp.conf /etc/ssmtp/ssmtp.conf

# Create symlink for php
RUN ln -s /usr/bin/php7 /usr/bin/php \
&& php --version

# Install Composer and make it available in the PATH
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer.phar \
  && mv /usr/bin/composer.phar /usr/bin/composer

WORKDIR /app
EXPOSE 9000

CMD ["php-fpm7", "-F"]
