# Sylius-Docker-Standard

Send FPM to background. Set to 'no' to keep FPM in foreground for debugging. Default value: yes.

docker build --tag experimental-v3 experimental/v3 

docker run -ti --rm experimental-v3
docker run -ti --rm --entrypoint=" experimental-v3 sh

```bash
composer install --no-interaction --no-scripts
bin/console sylius:install --no-interaction
bin/console sylius:fixtures:load default --no-interaction
yarn install --pure-lockfile
yarn build
```
