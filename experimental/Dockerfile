FROM php:8.0-fpm-alpine

COPY --from=composer:2.3 /usr/bin/composer /usr/bin/composer
COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/bin/install-php-extensions

RUN apk update && apk add \
    supervisor \
    unzip \
    nginx

RUN install-php-extensions \
    calendar \
    ctype \
    curl \
    dom \
    exif \
    gd \
    hash \
    iconv \
    intl \
    mbstring \
    opcache \
    pdo \
    pdo_mysql \
    pdo_pgsql \
    pdo_sqlite \
    pgsql \
    xml \
    xsl \
    zip
# bcmath bz2 calendar ctype curl dba dom enchant exif ffi fileinfo filter ftp gd gettext gmp hash
# iconv imap intl json ldap mbstring mysqli oci8 odbc opcache pcntl pdo
# pdo_dblib pdo_firebird pdo_mysql pdo_oci pdo_odbc pdo_pgsql pdo_sqlite pgsql phar posix pspell
# readline reflection session shmop simplexml snmp soap sockets sodium spl standard sysvmsg sysvsem sysvshm tidy
# tokenizer xml xmlreader xmlwriter xsl zend_test zip

RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* /usr/share/man/* /var/cache/apk/*

RUN ln -s /usr/local/sbin/php-fpm  /usr/sbin/php-fpm
RUN mkdir -p /var/log/php-fpm/

COPY .docker/supervisord.conf   /etc/supervisor/conf.d/supervisor.conf
COPY .docker/nginx.conf         /etc/nginx/nginx.conf
COPY .docker/fpm.conf           /usr/local/etc/php-fpm.conf
COPY .docker/php.ini            /usr/local/etc/php/php.ini

WORKDIR /app

EXPOSE 80

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisor.conf"]
