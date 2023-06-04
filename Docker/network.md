***Table of Contents:***

- [Docker Network](#docker-network)
  - [Bridge](#bridge)
  - [User-Defined Bridge](#user-defined-bridge)
  - [Host](#host)


# Docker Network

There are 7 types of network that we can configure with the containers:
- bridge (default)
- overlay
- host
- ipvlan
- macvlan
- none
- user-defined bridge

## Bridge

The default network is **bridge**. If we use 

```bash
$ docker network ls
```

we see two other networks, **host** and **none**.

**Note:** There is a column when we list docker networks called **Driver**. In docker world, driver simply means network type. :blush:

The default docker network interface is called **docker0**. When we create & run containers like

```bash
$ docker run -itd --rm --name test nginx
```

docker creates a virtual ethernet interface & connects it to the default bridge i.e. docker0 that acts like a switch, so containers can talk to each other.

**Note:** We can see a list of created interfaces using `ip add show` command. We can use `bridge link` command to see the connections between bridged interfaces.

The bridge also has DHCP pre-configured. We can inspect the so called network using

```bash
$ docker inspect bridge
```

This network has DNS that takes a copy of `etc/resolv.conf` file in the host.

We can get access to the shell of a specific container using

```bash
$ docker exec -it [name] sh
```

If we run `ip route` inside a container we get the IP address of our default gateway i.e. docker0.

**Note:** In a bridged network, ports are not exposed by themselves, so if you run a nginx container serving a website, you cannot get access to it by typing the IP address of the host on port 80.

**Note:** To expose a port for a container, we have to make sure it's not running. It's done by

```bash
$ docker stop [name]
```

We can expose a port using `-p [host port]:[container port]`:

```
$ docker run -itd --rm -p 80:80 --name [name] [image]
```
The bridge network is good for isolating the containers from the main network & the host.

:exclamation: :exclamation: ***Docker doesn't want us to use bridge.*** :exclamation::exclamation:

## User-Defined Bridge

We can create a network using:

```bash
$ docker network create [name]
```

To add a container to the created network we use the `--network` flag:

```bash
$ docker run -itd --rm --network [network_name] --name [container_name] [image_name]
```

The created network is isolated from the default bridge.

**Note:** We get a DNS service in the created network, so we can ping containers by their names.

## Host