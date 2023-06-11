***Table of Contents:***

- [Docker Network](#docker-network)
  - [Bridge](#bridge)
  - [User-Defined Bridge](#user-defined-bridge)
  - [Host](#host)
  - [MACVLAN](#macvlan)
    - [Bridge Mode](#bridge-mode)
    - [802.1q Mode](#8021q-mode)
  - [IPVLAN](#ipvlan)
  - [Overlay](#overlay)
  - [None](#none)


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

When you add a container to the host network, it inherits the IP & everything from the host, so you don't have to expose any ports. The container operates like all other local apps, so there is no isoloation.

## MACVLAN

This type of network has two modes:
- bridge mode
- 802.1q mode

### Bridge Mode

Using this type of netowrk is like connecting containers directly to the physical network. The containers get their own MAC addresses, and they'll have their own IP addresses on our network. First, let's create a MACVLAN network:

```bash
$ docker network create -d macvlan --subnet [Our_Netowrk_Subnet] --gateway [our_network_gateway] -o parent=[host_network_interface] [network_name]
```

We add the `parent` option to tie the MACVLAN network to the host physical network interface, BUT we have to assign the IP address ourselves. It has no DHCP. If we don't specify the IP address, docker will assign a conflicting IP address like there are two DHCP servers in the network.

```bash
$ docker run -itd --rm --network [network_name] --ip [IP_address] --name [container_name] [image_name]
```

**IMPORTANT:** If we try to ping anything from a MACVLAN container when we have multiple containers in that network, we get nothing. Since each container gets its own MAC address, the network cannot accept multiple MAC addresses on a single switch port. One solution is to activate **promiscuous mode** on all devices, but it's a nightmare!!

**Note:** Promiscuous mode is a mode for a network device that gathers all the traffic to a single processing center.

MACVLAN has DNS resolution & we don't have to expose any ports since each container gets their own IP address.

### 802.1q Mode

In this mode, a VLAN interface is created for each container like they are actual interfaces on the host & each interface can have their own network info e.g. IP address settings.

```bash
$ docker network create -d macvlan --subnet [network_subnet] --gateway [network_gateway] -o parent=[host_interface].[random_num] [network_name]
```

**Note:** For this mode we must have **trunking** set up.

## IPVLAN

This network type has 2 modes:
- L2
- L3
  
**L2** is the same as MACVLAN, but instead of giving each container a unique MAC address, it allows the host to share its MAC address with the containers. They'll also get their own IP addresses on the network.

```bash
$ docker network create -d ipvlan --subnet [subnet_IP] --gateway [gateway_IP] -o parent=[interface_name] [network_name]
```

**Note:** we still have the IP address assigning issue when we run a container in this network mode. We must assign an IP address manually.

**L3** means everything happens in layer 3 of netowrk architecture, so we don't have switching, arp, etc. We have IP addresses. This mode connects containers to the host & it acts like the switch. There is no broadcast traffic.

**Note:** Containers cannot reach anything on the network by default in this mode. Since everything is about routing no one has any routes about the so called netowrk, but you have controll over configuring it.

To solve the reachability issue, we have to set a static route in our main network, and tell it if you want to reach the containers go through the host.

```bash
$ docker network create -d ipvlan --subnet [subnet_IP] -o parent=[interface_name] -o ipvlan_mode=l3 --subnet [second_subnet_IP] [network_name]
```

**Note:** Containers in this mode can ping each other, even by name.

## Overlay

This network is used when there are containers across multiple hosts, and meybe there is orchestration, and these containers want to talk to each other. An overlay comes to play & simplifies the complexities of the real infrastructure.

## None

There is nothing in this network, and when you add a container to this network it only has loopback as its interface.