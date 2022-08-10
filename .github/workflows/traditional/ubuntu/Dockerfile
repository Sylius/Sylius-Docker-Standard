FROM sylius/standard:local

ARG SYLIUS_VERSION=1.12

ENV APP_ENV=prod
ENV APP_DEBUG=0
ENV GULP_ENV=prod
ENV DATABASE_URL="mysql://root@mysql/sylius_prod"
ENV PHP_DATE_TIMEZONE="Europe/Warsaw"
ENV COMPOSER_ALLOW_SUPERUSER=1

WORKDIR /

RUN composer create-project sylius/sylius-standard app "${SYLIUS_VERSION}.x@dev" --no-scripts --no-progress --no-interaction --no-plugins --no-install

WORKDIR /app/

RUN composer install --no-scripts --no-interaction --no-dev --no-progress
RUN php bin/console cache:warmup --no-debug --env=prod # --no-optional-warmers

RUN php bin/console doctrine:database:create --if-not-exists
RUN php bin/console sylius:install --no-interaction
RUN php bin/console sylius:fixtures:load default --no-interaction

RUN yarn install --ignore-scripts && yarn build

RUN mkdir -p public/media/image/
RUN chmod -R 777 var/
