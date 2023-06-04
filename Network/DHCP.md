***Table of Contents:***

- [DHCP (Dynamic Host Configuration Protocol)](#dhcp-dynamic-host-configuration-protocol)
    - [Components](#components)
    - [DHCP Handshake](#dhcp-handshake)


# DHCP (Dynamic Host Configuration Protocol)

DHCP (Dynamic Host Configuration Protocol) is a network management protocol used to dynamically assign an IP address to any device, or node, on a network so it can communicate using IP. DHCP automates and centrally manages these configurations rather than requiring network administrators to manually assign IP addresses to all network devices. DHCP can be implemented on small local networks, as well as large enterprise networks. Versions of DHCP are available for use in IP version 4 (IPv4) and IP version 6 (IPv6).

DHCP runs at the application layer of the TCP/IP stack. It dynamically assigns IP addresses to DHCP clients and allocates TCP/IP configuration information to DHCP clients. This information includes subnet mask information, default gateway IP addresses and domain name system (DNS) addresses.

DHCP is a client-server protocol in which servers manage a pool of unique IP addresses, as well as information about client configuration parameters. The servers then assign addresses out of those address pools. DHCP-enabled clients send a request to the DHCP server whenever they connect to a network.

Clients configured with DHCP broadcast a request to the DHCP server and request network configuration information for the local network to which they're attached. A client typically broadcasts a query for this information immediately after booting up. The DHCP server responds to the client request by providing IP configuration information previously specified by a network administrator. This includes a specific IP address, as well as a time period -- also called a lease -- for which the allocation is valid.

DHCP is not a routable protocol, nor is it a secure one. DHCP is limited to a specific local area network, which means a single DHCP server per LAN is adequate. Depending on the connections between these points and the number of clients in each location, multiple DHCP servers can be set up to handle the distribution of addresses. If network administrators want a DHCP server to provide addressing to multiple subnets on a given network, they must configure DHCP relay services located on interconnecting routers that DHCP requests have to cross. These agents relay messages between DHCP clients and servers located on different subnets.

DHCP lacks any built-in mechanism that enables clients and servers to authenticate each other. Both are vulnerable to deception -- one computer pretending to be another -- and to attack, where rogue clients can exhaust a DHCP server's IP address pool.

### Components

DHCP is made up of numerous components, such as the DHCP server, client and relay.

The DHCP server -- typically either a server or router -- is a networked device that runs on the DHCP service. The DHCP server holds IP addresses, as well as related information pertaining to configuration.

The DHCP client is a device -- such as a computer or phone -- that connects to a network and communicates with a DHCP server.

The DHCP relay manages requests between DHCP clients and servers. Typically, relays are used when an organization has to handle large or complex networks.

Other components include the IP address pool, subnet, lease and DHCP communications protocol.

### DHCP Handshake

There are 4 steps in DHCP handshake that happens between client and server:

1. Discover:
   When a new client is connected to the network, it broadcasts a DHCP discover packet with the source address of `0.0.0.0` & destination address of `255.255.255.255`.
2. Offer:
   When server receives the discovery packet, it broadcasts an offer packet with the source of its own IP address & destination of `255.255.255.255`. This packet contains the IP address, DNS server address, default gateway, etc.
3. Request:
   When the client receives the offer packet, it sends the request to the server since it has the IP address now.
4. Acknowledge:
   As the final packet of the handshake, since server knows the MAC address of the client, it acknowledges that the request is fulfilled.