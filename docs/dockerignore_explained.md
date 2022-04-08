# .dockerignore file explained

It is used to define files and directories that should not take part in building docker image via `docker build` command.
To minimize amount of files available in production image we want to ignore every file and directory that will be not
used in production environments ex. you are not going to run tests in production environment, so why to upload them
to production image.

## Sylius .dockerignore Example
```docker
.github/
etc/
features/
node_modules/
tests/
var/*
vendor/
public/assets/
public/bundles/
public/css/
public/js/
public/media/
```
