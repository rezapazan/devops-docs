***Table of Contents:***

- [DNS (Domain Name System)](#dns-domain-name-system)
  - [How does DNS work?](#how-does-dns-work)
    - [Step1: Local DNS](#step1-local-dns)
    - [Step2: Recursive DNS](#step2-recursive-dns)
    - [Step3: Root Nameserver](#step3-root-nameserver)
    - [Step4: TLD Nameserver](#step4-tld-nameserver)
    - [Step5: Authoritative Nameserver](#step5-authoritative-nameserver)
  - [DNS Resolution in 10 steps](#dns-resolution-in-10-steps)
  - [DNS Zones](#dns-zones)
  - [DNS Record Types](#dns-record-types)


# DNS (Domain Name System)

DNS stands for Domain Name System (DNS) services. When we access a website, we are using this service to locate the server where the domain's website is located. When browsing the web, we usually type in a domain name like www.google.com into our browser. This is better than trying to remember an IP address linked to a Google server.

Behind the scenes, a conversion happens using this service which converts www.google.com to 172.217.12.46. The IP address designates the location of a server on the Internet. This conversion process is called a query. This is an integral part of how devices connect with each other to communicate over the internet. To understand the query process, let’s review how this query works.

## How does DNS work?

### Step1: Local DNS

Let's visit a website by typing a domain name into a web browser. Our computer will start resolving the hostname, such as www.liquidweb.com. Our computer will then look for the IP address associated with the domain name in its local DNS cache. This cache stores this information that our computer has recently saved.  If it is present locally, then the website will be displayed. If our computer does not have the information, it will perform a DNS query to retrieve the correct information.

### Step2: Recursive DNS

If the information is not in your computer’s local cache, then it will query another server. Recursive DNS servers have their local cache, much like your computer. Many ISP’s use the same recursive DNS servers, it's possible that common domain name is already in its cache. If the domain is cached, the query will end here and the website displayed to the user.

### Step3: Root Nameserver

The DNS lookup process continues at the root nameserver, which is the first to translate human-readable domain names into numerical IP addresses that machines recognize. It doesn’t reference a specific location in the hierarchy, but rather a cluster of them where the desired query destination is to be found.

Keeping in tune with the same librarian and library analogy from the previous section, the root nameserver would be the index pointing to a series of book racks. The customer’s requested book is located on one of them, but the index doesn’t get any more specific than that.

### Step4: TLD Nameserver

The next step in identifying the IP address associated with a particular domain name is the TLD nameserver. TLD stands for top-level domain, and common examples include .com, .org, .net, .edu, or .gov. In the library that is the Internet, it can be associated with the particular bookrack where the volume requested by the customer lies.

### Step5: Authoritative Nameserver

Finally, the authoritative nameserver is the last server engaged in the DNS lookup process. It is the location where DNS records are held and where the translation between the domain name and the IP address occurs. Acting as a dictionary of the online world, it then returns the IP address of the desired webpage to the DNS recursor server that made the request.

## DNS Resolution in 10 steps

1. A user types www.domain.com into their browser.
2. The query is received by a DNS recursor.
3. The DNS recursor further queries the root nameserver.
4. The root server sends the address of a top-level domain (.com here) back to the recursor.
5. The DNS recursor queries the .com TLD nameserver.
6. The TLD nameserver responds with the IP address of the authoritative nameserver.
7. The DNS recursor then queries the domain’s authoritative nameserver.
8. The authoritative nameserver returns the IP address of the desired domain.
9. The DNS recursor feeds the IP address into the browser.
10. The user accesses the webpage they queried for.

## DNS Zones

A DNS zone is an administrative space within the Domain Name System. A zone forms one part of the DNS namespace delegated to administrators or specific entities. Each zone contains the resource records for all of its domain names.

## DNS Record Types

- A (address)
- CNAME (canonical name) 
- MX (mail exchanger)
- TXT (text)
- NS (nameserver)
- SOA (start of authority)
- SRV (service)
- PTR (pointer)

**Note:** to summarize the records see [this link](https://www.liquidweb.com/kb/how-to-demystify-the-dns-process/). 