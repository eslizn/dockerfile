FROM php:7-cli-alpine

RUN cd /tmp \
    && apk upgrade --update \
    && apk add --no-cache \
    libc-dev \
    libbz2 \
    libstdc++ \
    musl-dev \
    postgresql-dev \
    nghttp2-dev \
    git \
    tzdata \
    && apk add --no-cache --virtual .build-deps \
    linux-headers \
    autoconf \
    gcc \
    g++ \
    make \
    re2c \
    file \
    && docker-php-ext-install sockets pdo_mysql pdo_pgsql \
    && pecl bundle swoole \
    && ( \
        cd swoole \
        && phpize \
        && ./configure --enable-swoole --enable-sockets --enable-openssl --enable-http2 --enable-mysqlnd --enable-coroutine-postgresql \
        && make -j$(nproc) && make install \
        ) \
    && docker-php-ext-enable swoole \
    && rm -rf /var/cache/apk/* \
    && apk del .build-deps \
    && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php \
    && php -r "unlink('composer-setup.php');" \
    && mv composer.phar /usr/bin/composer \
    && chmod +x /usr/bin/composer \
    && rm -r /tmp/*
