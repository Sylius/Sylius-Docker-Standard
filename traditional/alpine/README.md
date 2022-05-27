```yaml
services:
    app:
        image: sylius/standard:1.11-traditional-alpine
        environment:
            APP_ENV: "dev"
            DATABASE_URL: "mysql://root:mysql@mysql/sylius_%kernel.environment%?charset=utf8mb4"
            PHP_DATE_TIMEZONE: "Europe/Warsaw"
            MAILER_URL: "smtp://mailhog:1025"
        volumes:
            - ./:/app:delegated
            - ./.docker/dev/php.ini:/etc/php8/php.ini:delegated
        ports:
            - 80:80
        depends_on:
            - mysql
        networks:
            - sylius
```