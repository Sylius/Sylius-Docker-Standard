FROM php:8.0-fpm

ARG DEBIAN_FRONTEND=noninteractive
ENV LC_ALL=C.UTF-8

# Install basic tools
RUN apt-get update && apt-get install -y \
    curl \
    supervisor \
    make \
    unzip \
    nginx

COPY --from=composer:2.3    /usr/bin/composer   /usr/bin/composer

# Cleanup
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

# Create directory for php-fpm socket
RUN mkdir -p /run/php && ln -s /usr/local/sbin/php-fpm  /usr/sbin/php-fpm

# Initialize config files
COPY .docker/supervisord.conf   /etc/supervisor/conf.d/supervisor.conf
COPY .docker/nginx.conf         /etc/nginx/nginx.conf
COPY .docker/fpm.conf           /etc/php/8.0/fpm/pool.d/www.conf
COPY .docker/php.ini            /etc/php/8.0/fpm/php.ini
COPY .docker/php.ini            /etc/php/8.0/cli/php.ini

WORKDIR /app

EXPOSE 80

CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]
