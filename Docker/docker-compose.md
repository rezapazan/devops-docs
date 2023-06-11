***Table of Contents:***

- [docker-compose](#docker-compose)
  - [Common Use Cases](#common-use-cases)

# docker-compose

Compose is a tool for defining and running **multi-container** Docker applications. With Compose, you use a **YAML** file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.

We need to create a `Dockerfile` for our app, so it could be reproduced anywhere. Then we need a `docker-compose.yml` to define services & make them run together in an isolated environment. We can start the entire app using `docker compose up`.

```yml
services:
  web:
    build: .
    ports:
      - "8000:5000"
    volumes:
      - .:/code
      - logvolume01:/var/log
    depends_on:
      - redis
  redis:
    image: redis
volumes:
  logvolume01: {}
```

**Note:** Compose V1 used `docker-compose` as its CLI interface, but in V2 we must use `docker compose`.

## Common Use Cases

- Development Environments
- Automated testing environments (e.g. in CI/CD)
- Single host deployments