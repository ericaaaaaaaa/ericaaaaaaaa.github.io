---
layout: post
title: The Internet
subtitle: note from Khan Academy
categories: Internet
tags: note internet
---

<center><h1>The Internet</h1></center>

> A notebook created by $Ericaaaaaaaa$.

## Intro

### The Ingredients of the Internet

The **Internet** is a global network of computing devices communicating with each other in some way, whether they're sending emails, downloading files, or sharing websites.

The Internet is an **open** network: any computing device can join as long as they follow the rules of the game. In networking, the rules are known as **protocols** and they define how each other must communicate with each other. The Internet is powered by many layers of protocols.

**Components**:

* **Wires & wireless**: Physical connections between devices, plus protocols for converting electromagnetic signals into binary data.
* **IP**: A protocol that uniquely identify devices using IP addresses and provides a routing strategy to send data to destination IP address.
* **TCP / UDP**: Protocols that can transport packets of data from one device to another and check for errors along the way.
* **TLS**: A secure protocol for sending encrypted data so that attackers can't view private information.
* **HTTP & DNS**: The protocols powering the World Wide Web, what the browser uses every time you load a webpage.

## Connecting Networks

### Computer Networks

A **computer network** is any group of interconnected computing devices capable of sending or receiving data.

A **computing device** is any device that can run a program.

#### Building a network

**Network topology**: ways of connecting computing devices in a network, including ring, mesh, star, bus, and tree.

#### Types of networks

* **Local Area Network (LAN)**: A network that covers a limited area.
* **Wide Area Network (WAN)**: A network that extends over a large geographic area and is composed of many LANs.
* **Data Center Network (DCN)**: A network used in data centers where data must be exchanged with very little delay.

#### Networking protocols

### Physical Network Connections

#### Copper cables

A type of **twisted pair cable** that's designed for use in computer networks.

Transmitting pulses of electricity that represent binary data.

Follow **Ethernet** standards, also known as Ethernet cables.

Used both in networks as small as a company office (a LAN) or as large as an entire country (a WAN).

#### Fiber-optic cables

A fiber-optic cable contains an optical fiber that can carry light (instead of electricity). The fiber is coated with plastic layers and sheathed in a protective tube to protect it from the environment.

Sending pulses of light that represent binary data.

Follow **Ethernet** standards.

Are capable of transmitting much more data per second than copper cables.

Often used to connect networks across oceans so that data can travel quickly around the world.

#### Wireless

A wireless card inside the computer turns binary data into radio waves and transmits them through the air.

The radio waves can't travel very far.

#### All together now

Our Internet connection might be using a combination of those technologies.

| Type                           | Sends       | Distance | Bandwidth | Issues                      |
| ------------------------------ | ----------- | -------- | --------- | --------------------------- |
| **Wireless**                   | Radio       | 100 ft   | 1.3 Gbps  | Slower in reality           |
| **Twisted pair copper cables** | Electricity | 330 ft   | 1 Gbps    | Susceptible to interference |
| **Fiber-optic cable**          | Light       | 50 miles | 26 Tbps   | Expensive                   |

### Bit Rate , Bandwidth, and Latency

#### Sending streams of 1s and 0s

The process of turning binary data into a time-based signal is known as **line coding**.

#### Bit Rate

**Bit rate**: the number of data that are sent each second. Used to measure the speed of data-transmitting.

> **bps**: bits per second
>
> **mbps**: megabits per second

| Unit        | Number of bits |
| ----------- | -------------- |
| **kilobit** | $1000$         |
| **megabit** | $1000^2$       |
| **gigabit** | $1000^3$       |
| **terabit** | $1000^4$       |
| **petabit** | $1000^5$       |

#### Bandwidth

**Bandwidth**: the maximum bit rate of a system.

> "Boardband Internet": A connection with a minimum bandwidth of 256 Kbps.

#### Latency

**Latency** means how late the bits arrive. Latency is the time between the sending of a data message and the receive of that message, measured in milliseconds.

**depends on**:

* the type of connections
* the distance from the sender to the receiver
* the congestion in the network (how many requests is waiting in line)

**major limitation**

* the speed of light

#### Internet Speed

Speed is a combination of bandwidth and latency.

> Internet providers often support a much faster download speed than upload speed.

## Addressing the Internet

### IP Address

The **Internet Protocol (IP)** is one of the core protocols in the layers of the Internet.

The protocol describes the use of **IP address** to uniquely identify Internet connected devices.

* **IPv4**: the first version ever used on the Internet
* **IPv6**: a backwards-compatible successor

#### IPv4 addresses

Two versions of the Internet protocol in use today.

Each IP address is split into $4$ numbers, and each of those numbers can range from $0$ to $255$:

[0-255].[0-255].[0-255].[0-255]

#### IPv6 address

There are $8$ hexadecimal numbers, and each number is 4 digits long

The IP address can change dynamically--**dynamic IP address**

Each Wi-Fi provider has its own range of addresses that it can give out.

Computers that act as servers often have **static** IP address.

### IP Address Hierarchy

IP addresses has **hierarchy** that makes it easier to route data around the Internet.

#### IPv4 address hierarchy

The first two octets (16 bits) identifies a network administered by the Internet Service Provider.

The last two octets (the final 16 bits) identifies a home computer on that Comcast network.

#### Subnets

Network administrators can break IP address into further subnetworks (subnets) as needed.

#### Splitting Octets

In actuality, IP addresses are often split in the middle of the octets.

#### IPv7 address hierarchy

## Routing with Redundancy

### IP Packets

The network protocols split each message into multiple small **packets**.

Each IP packet contains both a header (20 or 24 bytes long) and data (variable length).

* The header Includes the IP addresses of the source and destination, plus other fields that help to route the packet.
* The data is the actual content.

### Internet Routing Protocol

A **router** is a type of computing device used in computer networks that helps move the packets along.

In the Internet Protocol (IP), computers split messages into packets and those packets hop from router to router on the way to their destination.

#### Step 1: Send packet to router

Computer send the first packet to the nearest router.

#### Step 2: Router receives packet

#### Step 3：Router forwards packet

The goal of the router is to send the packet to a router that's closer to its final destination.

The router has a **forwarding table** that helps it pick the next path based in the destination IP address. (IP address prefixes)

#### Step 4: Final router forwards message

### Redundancy and Fault Tolerance

#### Redundancy in routing

The availability of multiple paths increases the **redundancy** of the network.

#### Fault tolerance

A **fault-tolerant** system is one that experience failure (or multiple failure) in  its components, but still continue operating properly.

## Transporting Packets

### The Problem With Packets

* multiple messages
* message out of order
* package corruption
* package lost
* package duplication

**Transmission Control Protocol (TCP)** is the data transport protocol that's most commonly used on top of IP and it includes strategies for packet ordering, retransmission, and data integrity.

**Using Datagram Protocol (UCP)** is an alternative protocol that solves fewer problems but offer faster data transport.

### User Datagram Protocol (UDP)

The **User Datagram Protocol (UDP)** is a lightweight data transport protocol that works on top of IP.

UDP is known as the **Unreliable Data Protocol** because it only provides a mechanism to detect corrupt data in packets, but it does not attempt to solve other problems that arise with packets.

When sending packets using UDP over IP, the data portion of each IP packet is formatted as a **UDP segment**.

Each UDP segment contains **8-byte header** and **variable length data**.

#### Port number

The first four bytes of the UDP header store the port numbers for the source and destination.

A networked device can receive messages on different virtual ports.

#### Segment Length

The next two bytes of the UDP header store the length (in bytes) of the segment (including the header).

#### Checksum

The final two bytes of the UDP header is the checksum, a field that's used by the sender and receiver to check for data corruption.

Before sending off the segment, the sender:

1. Computes the checksum based on the data in the segment.
2. Stores the computed checksum in the field.

Upon receiving the segment, the recipient:

1. Computes the checksum based on the received segment.
2. Compares the checksums to each other. If the checksums aren't equal, it knows the data was corrupted.

### Transmission Control Protocol (TCP)

The **Transmission Control Protocol (TCP)** is a transport protocol that is used on top of the IP to ensure reliable transmission of packages.

TCP includes mechanisms to solve many of the problems that arise from packet-based-messaging.

#### TCP/IP

Since TCP is the protocol used most commonly on top of IP, the Internet protocol stack is sometimes referred to as **TCP/IP**.

#### Packet format

When sending packets using TCP/IP, the data portion of each IP packet is formatted as a **TCP segment**.

Each TCP segment contains a header and data. The TCP header contains many more fields than the UDP header and can range in size from 20 to 60 bytes, depending on the size of the options field.

The TCP header shares some fields with UDP header: source port number, destination port number, and checksum.

#### From start to finish

##### Step 1: Establish connection

When two computers want to send data to each other over TCP, they need to establish a connection using a **three-way-handshake**.

* The first computer sends a packet with the SYN (synchronize?) bit set to 1.
* The second computer sends back a packet with the ACK (acknowledge!) bit set to 1.
* The first computer replies back with an ACK.

> The SYN and ACK bits are both part of the TCP header.

##### Step 2: Send packets of data

The sequence and acknowledgement numbers are part of the TCP header.

##### Step 3: Close the connection

Either computer can close the connection when they no longer want to send or receive data.

A computer initiates closing the connection by sending a packets with the FIN bit set to 1 (FIN = finish). The other computer replies with an ACK and another FIN. After one more ACK from the initiating computer, the connection is closed.

#### Detecting lost packets

TCP connection can detect lost packets using a timeout.

After sending off a packet, the sender starts a timer and puts the packet in a retransmission queue. If the timer runs out and the sender has not yet received an ACK from the recipient, it sends the packet again.

Files may be duplicated because of sending delay.

#### Handling out of order packets

TCP connections can detect out of order packets by using the sequence and acknowledgement numbers.

The recipient can use the sequence number to reassemble the packet data in the correct order.

## Web Protocols

### The World Wide Web

The **World Wide Web(WWW, Web)**

The Web is a massive network of webpages, programs, and files that are accessible via URLs.

#### Powered by protocols

A web browser loads a webpage using various protocols:

1. It uses **Domain Name System (DNS) protocol** to convert a domain name into a IP address.
2. It uses **HyperText Transfer Protocol (HTTP)** to request the webpage contents from that IP address.

It may also use the **Transport Layer Security (TLS) protocol** to serve the website over a secure, encrypted connection.

The web browser uses these protocols on top of the Internet protocols, so every HTTP request also uses TCP and IP.

### Domain Name System (DNS)

IP addresses are how computers identify other computers on the Internet.

The **Domain Name System (DNS)** gives us humans an easy way to identify where we want to go on the Internet.

A **domain name** is a human-friendly address for a website, something that's easy for us to remember and type in.

#### Anatomy of a domain name

Each domain name is made up of parts:

```
[third-level-domain].[second-level-domain]-[top-level-domain]
```

There are a limited set of *top level domains* (TLDs), and many websites use the most common TLDs, ".com", ".org", and ".edu".

The *second level* domain is unique to the company or organization that registers it.

The *third level* domain is also called **subdomain**, because it's owned by the same group and that URL often directs you to a subset of the website.

#### Domains $\leftrightarrow$ IP address 

Each domain name maps to an IP address. 

#### Step 1: Check the local cache

The computers keep a local cache of domain name to IP mappings.

#### Step 2: Ask the ISP cache

Every ISP (Internet Service Provider) provides a domain name resolving service and keeps its own cache.

#### Step 3: Ask the name servers

There are domain name servers scattered around the world that are responsible for keeping track of a subset of the millions of domain names.

The servers are ordered in a hierarchy:

Root name servers $\rightarrow$ TLD name  servers $\rightarrow$ Host name servers

### Hypertext Transfer Protocol (HTTP)

#### Step 1: Direct browser to URL

The url starts with "http" signal the browser that it needs to use HTTP to fetch the document for that URL. 

#### Step 2: Browser looks up IP

The browser uses a DNS resolver to map the domain to an IP address.

#### Step 3: Browser sends HTTP request

Once the browser identifies the IP address of the computer hosting the request URL, it sends an **HTTP request**.

The HTTP request contains:

* An action: `GET` or `POST`
* The path
* The protocol and it's version
* The domain of the requested URL.

#### Step 4: Host sends back HTTP response

contains:

* The protocol and it's version

* The **http status code**

  * 200: ok
  * 404: file not found

* The **headers**: give the browser additional details and help the browser additional details and help the browser to render the content.

  * **Content-Type**: tells the browser what type of document it's sending back.

    e.g. text/html, image/png, video/mpeg, application/javascript

  * **Content-Length**: the length of the document in bytes, which helps the browser know how long a file will take to download.

* The actual document requested.

#### Step 5: The browser renders the response

#### HTTP and TCP / IP

HTTP is a protocol that's built *on top of* the TCP/IP protocols. 

## Scalable Systems

### Scalable Systems

A **scalable** system is one that can continue functioning well even as it experiences higher usage.

#### Internet scalability

Features that increases the scalability of the Internet:

* Any computing device can send data around the Internet if it follows the protocols. There is no bureaucratic process that blocks a device from joining or prevents a programmer from learning how the protocols work.
* The IPv6 addressing system can uniquely address a *trillion trillion* times the amount of devices currently connected to the Internet.
* Routing is dynamic, so new routers can join a network at any time and help to move data packets around the Internet.

Features that threatens the scalability of the Internet:

* Network connections have limited bandwidth.
* Routers have limited throughput (the amount of data they can forward per second).
* Wireless routers often have a limitation in the number of devices that can be connected to them.

#### Web application scalability

#### Load testing

Engineering teams can prepare for spikes in usage by doing **load testing**: simulating high amounts of traffic in a short period of time to see if the system buckles under the load. Load testing can uncover bottlenecks or hardcoded limits in the system.

#### The spectrum of scalability

## The Internet Protocol Suite

### The Internet Protocol Suite

![](https://cdn.kastatic.org/ka-perseus-images/6a0cd3a5b7e709c2f637c959ba98705ad21e4e3c.svg)

#### Layer by layer

At the **bottom** layer, two computing devices need a physical mechanism to send digital data to each other. They send electromagnetic signals either over a **wired** or **wireless connection** and interpret the signal as bits. The type of physical connection affects the **bit rate** and **bandwidth**.

Every node on the Internet is identified with an **IP address**.

The data must pass from router to router until it finally reaches its destination, a strategy that comes from the **Internet routing protocol**.

Data needs to be broken up into small packets, which are then reassembled at the destination. The **Transmission Control Protocol (TCP)** is used to ensure reliable transport of those packets, with sequencing, acknowledgement, and retries. A faster but less reliable transport protocol is the **User Diagram Protocol (UDP)**.

The most common use of the Internet is the **World Wide Web (WWW)**, with its millions of publicly viewable websites, all made possible due to the **HyperText Transfer Protocols (HTTP)**. We can visit a website by typing a domain name in the browser address bar, since the browser knows how to return the domain into an IP address using the **Domain Name System (DNS)**.

When the data contains private information, it needs to be transported securely from the sender to the destination. The **Transport Layer Security (TLS) protocol** uses algorithms to encrypt the data, while **certificate authorities** help users trust the encryption.

#### A protocol stack

This stack of protocols is used when a DNS request is sent through the Internet.

![](https://cdn.kastatic.org/ka-perseus-images/918dc8144c8813382ea3ffbbf76db1535d97421a.svg)

This protocol stack is used when an HTTP request is sent through the Internet.

![](https://cdn.kastatic.org/ka-perseus-images/49e49dd1e8744a2422215288147e00443fc0916c.svg)

This stack of protocols includes multiple protocols at the application layer (both HTTP and TLS).

![](https://cdn.kastatic.org/ka-perseus-images/de452773728c35833566ddae2f78289ecae61340.svg)

## Open Protocol Development

### Open Protocol Development

#### The need for standardization

**Standardization** allows the computing device to communicate with each other correctly and effectively. Once the protocol is written up in a document and other network administrators agree that it is sensible protocol, that protocol is considered as a **standard**.

#### The importance of being open

An **open (nonproprietary)** protocol is one that is not owned by any particular company and *not* limited to a particular company's products.

#### Open standard specifications

The specifications for the Internet protocols are maintained by the Internet Engineering Task Force (IETF),

The HTML living standard is maintained by the WhatWG community.

CSS has many specifications and is maintained by the W3C.

JavaScript is based on the ECMAScript standard.

## The Digital Divide

### The global digital divide

The difference in access to computing devices and the Internet is referred to as **the digital divide** and is often due to socioeconomic, geographic, or demographic factors.

#### The statistics

### The geographic digital divide

### The socioeconomic digital divide

### The digital use divide