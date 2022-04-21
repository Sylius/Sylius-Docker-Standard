FROM php:8.0-fpm-alpine

ARG DEBIAN_FRONTEND=noninteractive
ENV LC_ALL=C.UTF-8

COPY --from=composer:2.3 /usr/bin/composer /usr/bin/composer

# Install basic tools
RUN apk update && apk --no-cache add \
    supervisor \
    make \
    unzip \
    nginx

RUN docker-php-ext-install \
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
RUN docker-php-ext-enable \
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

# Cleanup
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

# Create directory for php-fpm socket
RUN ln -s /usr/local/sbin/php-fpm  /usr/sbin/php-fpm

# Initialize config files
COPY .docker/supervisord.conf   /etc/supervisor/conf.d/supervisor.conf
COPY .docker/nginx.conf         /etc/nginx/nginx.conf
COPY .docker/fpm.conf           /etc/php/8.0/fpm/pool.d/www.conf
COPY .docker/php.ini            /etc/php/8.0/fpm/php.ini
COPY .docker/php.ini            /etc/php/8.0/cli/php.ini

WORKDIR /app

EXPOSE 80

ENTRYPOINT ["/usr/bin/supervisord", "-c", "/etc/supervisor/conf.d/supervisor.conf"]
