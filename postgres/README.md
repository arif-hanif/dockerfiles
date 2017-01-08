# PostgreSQL

Version: 9.5.4

# Build

```bash
$ make build
```

# DockerHub

This image is published on DockerHub as `docker pull strues/postgres`.

[Click here to see it's DockerHub homepage](https://hub.docker.com/r/strues/postgres/)

# Usage

This image works in the same way the official `postgres` docker image work.

It's documented on DockerHub in it's README: [https://hub.docker.com/_/postgres/](https://hub.docker.com/_/postgres/).

For example, you can start a basic PostgreSQL server, protected by a password,
listening on port 5432 by running the following:

```
$ docker run --name awesomepg -e POSTGRES_PASSWORD=mysecretpassword -d strues/postgres
```

Next, you can start you app's container while **linking** it to the PostgreSQL
container you just created giving it access to it.

```
$ docker run --name some-app --link awesomepg:postgres -d application-that-uses-postgres
```

Your app will now be able to access `POSTGRES_PORT_5432_TCP_ADDR` and `POSTGRES_PORT_5432_TCP_PORT` environment variables.

# License

MIT. See `LICENSE` file.
