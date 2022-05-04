FROM ubuntu:20.04
ARG DEBIAN_FRONTEND=noninteractive
ENV LC_ALL=C.UTF-8

# Install basic tools
RUN apt-get update && apt-get install -y \
    software-properties-common \
    curl \
    make \
    supervisor \
    unzip

# Append NGINX and PHP repositories
RUN add-apt-repository ppa:ondrej/php && add-apt-repository ppa:ondrej/nginx

# Install NGINX and required PHP extensions
RUN apt-get update && apt-get install -y \
    nginx \
    php8.0 \
    php8.0-apcu \
    php8.0-common \
    php8.0-cli \
    php8.0-xml \
    php8.0-curl \
    php8.0-gd \
    php8.0-intl \
    php8.0-opcache \
    php8.0-pdo \
    php8.0-ctype \
    php8.0-calendar \
    php8.0-dom \
    php8.0-exif \
    php8.0-xsl \
    php8.0-zip \
    php8.0-fpm \
    php8.0-mysql \
    php8.0-mbstring \
    php8.0-sqlite \
    php8.0-pgsql \
    php8.0-yaml

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename composer

# Cleanup
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/* /usr/share/man/*

# Create directory for php-fpm socket
# Link php-fpm binary file without version
# -p Creates missing intermediate path name directories
RUN ln -s /usr/sbin/php-fpm8.0 /usr/sbin/php-fpm && mkdir -p /run/php

# Initialize config files
COPY .docker/supervisord.conf   /etc/supervisor/conf.d/supervisor.conf
COPY .docker/nginx.conf         /etc/nginx/nginx.conf
COPY .docker/fpm.conf           /etc/php/8.0/fpm/pool.d/www.conf
COPY .docker/php.ini            /etc/php/8.0/fpm/php.ini
COPY .docker/php.ini            /etc/php/8.0/cli/php.ini

WORKDIR /app

EXPOSE 80

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]
