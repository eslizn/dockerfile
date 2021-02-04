FROM php:7.3-alpine

RUN apk add composer

RUN apk add --no-cache libstdc++ && \
    cd /tmp && \
    apk --update-cache add --virtual build-dependencies ${PHPIZE_DEPS} php7-dev curl-dev && \
    docker-php-ext-install sockets && \
    docker-php-ext-install pdo_mysql && \
    pecl bundle swoole && \
    cd swoole && \
    phpize && \
    ./configure --enable-sockets --enable-swoole --enable-mysqlnd --enable-swoole-json --enable-swoole-curl && \
    make && make install && \
    docker-php-ext-enable swoole && \
    apk del build-dependencies && \
    rm -rf /tmp/*

