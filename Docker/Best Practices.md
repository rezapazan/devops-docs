# Best Practices

***Table of Contents:***

- [Best Practices](#best-practices)

Here are some of the best practices for docker in production:

- **use official docker images as base image** e.g. in a node app instead of using an OS image use the official node image.

```dockerfile
FROM node
```

- **use specific docker image versions** because `latest` tag is unpredictable.

```dockerfile
FROM node:17.0.1
```

- **use small-sized official images** & avoid using full-blown OS distro-based images e.g. Ubuntu. They have larger image size &security vulnerabilities. With smaller images we need **less storage** & **transferring them is faster**.

```dockerfile
FROM node:17.0.1-alpine
```

- **optimize caching image layers.** Each command creates a layer in a dockerfile & the base image has layers itself. Docker caches each layer itself, saved on a local filesystem.

**NOTE:** When a layer changes, all following layers are re-created as well, so in node-related projects we need to re-run the `npm install` layer only when `package.json` file changes.

```dockerfile
FROM node:17.0.1-alpine

WORKDIR /app

COPY myapp /app

RUN npm install --production

CMD ["node", "src/index.js"]
```

The result is:

```dockerfile
FROM node:17.0.1-alpine

WORKDIR /app

COPY package.json package-lock.json .

RUN npm install --production

COPY myapp /app

CMD ["node", "src/index.js"]
```

- **use `.dockerignore` file.** We don't need some content to be involved in building process.

```ignore
# ignore .git and .cache folders
.git
.cache

# ignore all markdown files (md)
*.md

# ignore sensitive files
private.key
settings.json
```

- **use multi-staged builds.** There are some content that we need during the build process, but we don't need them in the final image to run the app e.g. `package.json`.

```dockerfile
# Build Stage

FROM maven as build

WORKDIR /app

COPY myapp /app

RUN mvn package


# Run Stage

FROM tomcat

COPY --from=build /app/target/file.war /usr/local/tomcat/...
```

**NOTE:** Only the last two commands will create the layers of our image.

- **use the latest privileged user.** By default, dockerfile uses `root` user & we don't want to use root access. We create a dedicated user & group.

```dockerfile
# create group and user
RUN groupadd -r tom && useradd -g tom tom

# set ownership and permissions
RUN chown -R tom:tom /app

# switch to user
USER tom

CMD node index.js
```

For node apps:

```dockerfile
FROM node:10-alpine

...

# set ownership and permissions
RUN chown -R node:node /app

# switch to node user
USER node

CMD ["node", "index.js"]
```

- **scan images for security vulnerabilities.** We use `docker scan` command to scan our image.
