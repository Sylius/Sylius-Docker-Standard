FROM node:14-alpine AS node
FROM composer:2.3 AS composer

FROM php:8.2.12RC1-fpm-alpine

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
#    python \
    g++ \
    make \
    nginx \
    yarn \
    php \
#    php-apcu \
    php-calendar \
    php-common \
    php-cli \
    php-common \
    php-ctype \
    php-curl \
    php-dom \
    php-exif \
    php-fileinfo \
    php-fpm \
    php-gd \
    php-intl \
    php-mbstring \
    php-mysqli \
    php-mysqlnd \
    php-opcache \
    php-pdo \
    php-pdo_mysql \
    php-pdo_pgsql \
    php-pgsql \
    php-phar \
    php-session \
    php-simplexml \
    php-sqlite3 \
    php-tokenizer \
    php-xml \
    php-xmlwriter \
    php-xsl \
    php-zip

RUN rm -rf /var/lib/apk/lists/* /tmp/* /var/tmp/* /usr/share/doc/* /usr/share/man/* /var/cache/apk/*

RUN ln -s /usr/sbin/php-fpm8 /usr/sbin/php-fpm \
#    && ln -s /usr/bin/php8 /usr/bin/php \
    && mkdir -p /run/php /var/log/php-fpm

#RUN adduser -u 1000 -D -S -G www-data www-data

COPY .docker/supervisord.conf   /etc/supervisor/conf.d/supervisor.conf
COPY .docker/nginx.conf         /etc/nginx/nginx.conf
COPY .docker/php-fpm.conf       /etc/php8/php-fpm.conf
COPY .docker/php.ini            /etc/php8/php.ini

WORKDIR /app

EXPOSE 80

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisor.conf"]
