***Table of Contents:***

- [Dockerfile](#dockerfile)
  - [Multi-Stage Builds](#multi-stage-builds)

# Dockerfile

`RUN` is a command that adds a layer to our image & build process. `RUN` commands are executed during build.
`CMD` command is executed be default when we run the container.
`ENTRYPOINT` command gives you the ability to change the default entry point of `docker run <image>` command.
  for more info see [this link](https://stackoverflow.com/a/34245657).
`USER` changes the user executing the commands.

## Multi-Stage Builds

We can have multi-stage builds that help us cut the size of our final build. We can have multiple stages using `FROM` command, and we can name stages by adding `AS` to `FROM`:

```dockerfile
FROM node:14 AS sass
WORKDIR /example
RUN npm install -g node-sass
COPY example.scss .
RUN node-sass example.scss example.css
 
FROM php:8.0-apache
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer
COPY composer.json .
COPY composer.lock .
RUN composer install --no-dev
COPY --from=sass /example/example.css example.css
COPY index.php .
COPY src/ src
```

We can refer to a stage using its name by adding `--from=[stage_name]` to the command.

another example:

```dockerfile
FROM golang:latest
WORKDIR /go
COPY app.go .
RUN go build -o my-binary
 
FROM alpine:latest
WORKDIR /app
COPY --from=build /go/my-binary .
CMD ["./my-binary"]
```

Example:

```dockerfile
### BEGIN building
FROM node:lts-alpine AS builder

# Setting build environment variables
ARG BASE_URL
ENV BASE_URL=$BASE_URL

# Copy source files insede build container
COPY ["./src", "/tmp/frontend_src"]
WORKDIR /tmp/frontend_src

# Installing npm build dependencies
RUN ["npm", "install"]

# Building application
RUN ["npm", "run", "build"]
### END building


FROM node:lts-alpine

# Copy source files inside container
COPY --from=builder --chown=node:node ["/tmp/frontend_src/.next/standalone", "/app"]
COPY --from=builder --chown=node:node ["/tmp/frontend_src/.next/static", "/app/.next/static"]
COPY --from=builder --chown=node:node ["/tmp/frontend_src/public", "/app/public"]
COPY --from=builder --chown=node:node ["/tmp/frontend_src/package.json", "/tmp/frontend_src/next.config.js", "/app/"]

# Setting workdir
WORKDIR /app

# Setting user and group
USER node:node

ENV PORT=3000
ENV NODE_ENV=production

EXPOSE 3000

# Running migrations and start server
ENTRYPOINT ["node"]
CMD ["server.js"]
```
