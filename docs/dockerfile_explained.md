# Dockerfile Explained

## Introduction

[Dockerfile](https://docs.docker.com/engine/reference/builder) is a set of instructions that define Docker Image.

## Headless Dockerfile

```dockerfile
# Ubuntu LTS Base image Specified
FROM ubuntu:20.04

# Docker Argument that defines noninteractive mode used when you need zero interaction while installing
# or upgrading the system via apt
ARG DEBIAN_FRONTEND=noninteractive

# Define all locale settings - just to remove some of the commands warnings 
ENV LC_ALL=C.UTF-8

# Install basic linux tools
# software-properties-common - package provides apt-add-repository command
# curl - required to use php-curl extension
# unzip - required by composer to improve package extracting and php-zip extension
# make - to execute Makefile scripts
# supervisor - to supervise nginx and php-fpm processes
RUN apt-get update && apt-get install -y \
    software-properties-common \ 
    curl \
    unzip \
    make \
    supervisor

# Append NGINX and PHP repositories
# They are recommended to use latest PHP and NGINX LTS versions
RUN add-apt-repository ppa:ondrej/php && add-apt-repository ppa:ondrej/nginx

# Install NGINX and required PHP extensions
RUN apt-get update && apt-get install -y \
    nginx \
    php8.0 \
    php8.0-calendar \
    php8.0-cli \
    php8.0-common \
    php8.0-ctype \
    php8.0-curl \
    php8.0-dom \
    php8.0-exif \
    php8.0-fpm \
    php8.0-gd \
    php8.0-intl \
    php8.0-mbstring \
    php8.0-mysql \
    php8.0-opcache \
    php8.0-pdo \
    php8.0-pgsql \
    php8.0-sqlite \
    php8.0-xml \
    php8.0-xsl \
    php8.0-yaml \
    php8.0-zip

# Link php-fpm binary file without version
# This change is purely semantical as then php-fpm exacutable will be deducted from PHP version
RUN ln -s /usr/sbin/php-fpm8.0 /usr/sbin/php-fpm

# Install Composer Package Manager
# Alternative:
# ARG COMPOSER_VERSION=2
# COPY --from=composer:${COMPOSER_VERSION} /usr/bin/composer /usr/bin/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename composer

# Cleanup
# The image size has big impact on the environment, that's why cleaing some directories will decrease image size
# apt-get clean - removes apt-get related cache files
# /var/lib/apt/lists/ - this contains apt repositories
# /tmp/ - this contains temporary files used by Linux, are stored in RAM
# /var/tmp/ - this contains temporary files used by Linux, are stored in physical storage
# /usr/share/doc/ - documentation
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

# Create directory for php-fpm socket
# This is a known problem of php-fpm that it cannot create anything under /run/ directory, but it requires it to work
RUN mkdir -p /run/php

# Copy config files from local storage to image
COPY .docker/supervisord.conf   /etc/supervisor/conf.d/supervisor.conf
COPY .docker/nginx.conf         /etc/nginx/nginx.conf
COPY .docker/fpm.conf           /etc/php/8.0/fpm/pool.d/www.conf
COPY .docker/php.ini            /etc/php/8.0/fpm/php.ini
COPY .docker/php.ini            /etc/php/8.0/cli/php.ini

# Define working directory as app - the applciation files will be there
WORKDIR /app

# Expose HTTP port to enable communication
EXPOSE 80

# Define supervisor as entrypoint - it will run php-fpn and nginx processes, and restart it in case of any problem
ENTRYPOINT ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]
```
