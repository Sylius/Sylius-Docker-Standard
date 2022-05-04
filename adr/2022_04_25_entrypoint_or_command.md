# Entrypoint or Command

* Status: accepted
* Date: 2022-04-25
* Related Image: all
* Related Image Version: >=1.11

## Context and Problem Statement

Default action that should be executed when running the docker container (`docker run`) can be defined by command or entrypoint.
The Sylius Standard image uses a supervisor to maintain the all-in-one image. The Supervisor runs NGINX and PHP-FPM processes.
Because of that and not seeing a use case the decision of using entrypoint was made, but after some time and gained experience.
We can see that it may not be the best solution.

## Decision Drivers

* Best developer experience
* Being able to simply run one command by Sylius Docker Standard
* Running cron jobs and workers in Sylius Docker Standard

## Considered Options

### Entrypoint (ENTRYPOINT)

Entrypoint should be used as the default application that is triggered when running the container, and the command then shall extend
application abilities via additional parameters.

```dockerfile
ENTRYPOINT ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]
```

* Good, because it defines default application/process
* Good, it can be simpy run by `docker container run sylius/standard`
* Bad, because you need the `--entrypoint` command to run any other command
* Bad, because you can't use COMMAND to provide additional parameters

### Command (CMD)

The Command should be used with Entrypoint as the ability to provide additional parameters or as a simple override and default application.

```dockerfile
CMD ["/usr/bin/supervisord", "-c", "/etc/supervisor/supervisord.conf"]
```

* Good, because it can be simply overridden
* Good, because the image can be used as a base image

## Decision Outcome

The better approach is to use Command with supervisor instead of Entrypoint as there is no need to provide additional parameters
to the Supervisor's execution.

Chosen option: **Command (CMD)**

Every image will use `CMD` instead of `ENTRYPOINT`.

You will be able to run container with command:

```bash
docker container run -it sylius/standard:1.11-traditional bash
```
instead of
```bash
docker container run -it --entrypoint="" sylius/standard:1.11-traditional bash
```

## References

* [Dockerfile Entrypoint Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#entrypoint)
* [Dockerfile Command Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#cmd)
* [Command Reference](https://docs.docker.com/engine/reference/builder/#cmd)
* [Entrypoint Reference](https://docs.docker.com/engine/reference/builder/#entrypoint)
