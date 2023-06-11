***Table of Contents:***

- [Dockerfile](#dockerfile)
  - [Multi-Stage Builds](#multi-stage-builds)

# Dockerfile

`RUN` is a command that adds a layer to our image & build process. `RUN` commands are executed during build.
`CMD` command is executed be default when we run the container.

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