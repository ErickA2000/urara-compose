# Kong in Docker Compose

The official Docker Compose template for Kong Gateway.

> **Note**
> The Kong Docker Compose file uses the v3.9 compose schema; as such,
> it requires Docker Engine version 20.10.0+.

## What is Kong?

Kong or Kong API Gateway is a cloud-native, platform-agnostic, scalable API 
Gateway distinguished for its high performance and extensibility via plugins.

- Kong's Official documentation can be found at [docs.konghq.com][kong-docs-url].
- You can find the official Docker distribution for Kong on [Docker Hub][kong-docker-url].

## How to use this Compose file

Kong Gateway can be deployed in different ways. This Docker Compose file provides 
support for running Kong in [db-less][kong-docs-dbless] mode, in which only a Kong 
container is spun up, or with a backing database. The default is db-less mode:

```shell
$ docker compose up -d
```

This command will result in a single Kong Docker container:

```shell
$ docker compose ps
NAME                IMAGE               COMMAND                  SERVICE             CREATED             STATUS                    PORTS
compose-kong-1      kong:latest         "/docker-entrypoint.…"   kong                38 seconds ago      Up 29 seconds (healthy)   0.0.0.0:8000->8000/tcp, 127.0.0.1:8001->8001/tcp, 0.0.0.0:8443->8443/tcp, 127.0.0.1:8444->8444/tcp
$ docker ps
```

Kong entities can be configured through the `config/kong.yaml` declarative config
file. Its format is further described [here][kong-docs-dbless-file].

You can also run Kong with a backing Postgres database:

```shell
$ KONG_DATABASE=postgres docker compose --profile database up -d

```

Which will result in two Docker containers running -- one for Kong itself, and
another for the Postgres instance it uses to store its configuration entities:

```shell
$ docker compose ps
NAME                IMAGE               COMMAND                  SERVICE             CREATED              STATUS                        PORTS
compose-db-1        postgres:9.5        "docker-entrypoint.s…"   db                  About a minute ago   Up About a minute (healthy)   5432/tcp
compose-kong-1      kong:latest         "/docker-entrypoint.…"   kong                About a minute ago   Up About a minute (healthy)   0.0.0.0:8000->8000/tcp, 127.0.0.1:8001->8001/tcp, 0.0.0.0:8443->8443/tcp, 127.0.0.1:8444->8444/tcp
```

Kong will be available on port `8000` and `8001`. You can customize the template 
with your own environment variables or datastore configuration.

## Backend URARA

First you have to create the files with environment variables. Look at the example files. The files are called the same as the example files, you just have to delete the ".example".

For execute the applications of backend.

```shell
$ docker compose -f docker-compose.yml up -d
```

The applications will be available on port `3000`, `3001`, `3002`, `3003` and `3004`.

[kong-docs-url]: https://docs.konghq.com/
[kong-docs-dbless]: https://docs.konghq.com/gateway/latest/production/deployment-topologies/db-less-and-declarative-config/#main
[kong-docs-dbless-file]: https://docs.konghq.com/gateway/latest/production/deployment-topologies/db-less-and-declarative-config/#declarative-configuration-format
[kong-docker-url]: https://hub.docker.com/_/kong