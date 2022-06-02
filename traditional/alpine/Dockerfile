FROM node:14-alpine AS node
FROM composer:2.3 AS composer

FROM alpine:3.15

COPY --from=composer /usr/bin/composer /usr/bin/composer

COPY --from=node /usr/lib           /usr/lib
COPY --from=node /usr/local/share   /usr/local/share
COPY --from=node /usr/local/lib     /usr/local/lib
COPY --from=node /usr/local/include /usr/local/include
COPY --from=node /usr/local/bin     /usr/local/bin

RUN apk update --no-cache && apk add --no-cache \
    curl \
    supervisor \
    unzip \
    python2 \
    g++ \
    make \
    nginx \
    yarn \
    php8 \
    php8-apcu \
    php8-calendar \
    php8-common \
    php8-cli \
    php8-common \
    php8-ctype \
    php8-curl \
    php8-dom \
    php8-exif \
    php8-fileinfo \
    php8-fpm \
    php8-gd \
    php8-intl \
    php8-mbstring \
    php8-mysqli \
    php8-mysqlnd \
    php8-opcache \
    php8-pdo \
    php8-pdo_mysql \
    php8-pdo_pgsql \
    php8-pgsql \
    php8-phar \
    php8-session \
    php8-simplexml \
    php8-sqlite3 \
    php8-tokenizer \
    php8-xml \
    php8-xmlwriter \
    php8-xsl \
    php8-zip

RUN rm -rf /var/lib/apk/lists/* /tmp/* /var/tmp/* /usr/share/doc/* /usr/share/man/* /var/cache/apk/*

RUN ln -s /usr/sbin/php-fpm8 /usr/sbin/php-fpm \
    && ln -s /usr/bin/php8 /usr/bin/php \
    && mkdir -p /run/php /var/log/php-fpm

RUN adduser -u 1000 -D -S -G www-data www-data

COPY .docker/supervisord.conf   /etc/supervisor/conf.d/supervisor.conf
COPY .docker/nginx.conf         /etc/nginx/nginx.conf
COPY .docker/php-fpm.conf       /etc/php8/php-fpm.conf
COPY .docker/php.ini            /etc/php8/php.ini

WORKDIR /app

EXPOSE 80

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisor.conf"]
