# Core Networking Concepts for DevOps

This document explains some fundamental networking concepts required for DevOps, cloud computing, and system administration.

---

# 1. TCP vs UDP

TCP (Transmission Control Protocol) and UDP (User Datagram Protocol) are communication protocols used at the **Transport Layer (Layer 4)** of the OSI Model. They define how data is transmitted between devices over a network.

---

## TCP (Transmission Control Protocol)

TCP is a **connection-oriented protocol** that ensures reliable and ordered delivery of data between devices.

### Key Features

- Reliable communication
- Error checking
- Data retransmission if packets are lost
- Ordered data delivery
- Congestion control

### TCP Three-Way Handshake

Before communication begins, TCP establishes a connection using a process called the **three-way handshake**.

Steps:

1. **SYN**
   - Client sends a SYN (synchronize) request to the server.

2. **SYN-ACK**
   - Server responds with SYN-ACK (synchronize acknowledgment).

3. **ACK**
   - Client sends an acknowledgment to establish the connection.

After this process, data transmission begins.

### TCP Use Cases

TCP is used where **data reliability is critical**.

Examples:

- Web browsing (HTTP / HTTPS)
- File transfer (FTP)
- Email services (SMTP)
- Database connections
- SSH remote login

---

## UDP (User Datagram Protocol)

UDP is a **connectionless protocol** that sends data without establishing a connection.

### Key Features

- Faster than TCP
- No connection establishment
- No guarantee of packet delivery
- No packet ordering
- Low latency communication

Because UDP skips reliability checks, it is much faster than TCP.

### UDP Use Cases

UDP is used in applications where **speed is more important than reliability**.

Examples:

- Video streaming
- Online gaming
- VoIP calls
- DNS queries
- Live broadcasting

---

## TCP vs UDP Comparison

| Feature | TCP | UDP |
|------|------|------|
| Connection | Connection-oriented | Connectionless |
| Reliability | Reliable | Unreliable |
| Speed | Slower | Faster |
| Error Checking | Yes | Minimal |
| Data Ordering | Maintained | Not guaranteed |
| Use Cases | Web, Email, File Transfer | Streaming, Gaming, DNS |

---

# 2. IP Addressing

An **IP Address (Internet Protocol Address)** is a unique identifier assigned to a device on a network. It allows devices to locate and communicate with each other over the internet or private networks.

Example of an IP address:


192.168.1.10


---

## IPv4 (Internet Protocol Version 4)

IPv4 is the most widely used version of the Internet Protocol.

### Characteristics

- 32-bit address
- Written in decimal format
- Divided into four octets
- Each octet ranges from **0–255**

Example:


192.168.1.1


Total possible IPv4 addresses:


2^32 = 4.3 billion


Due to the growth of the internet, IPv4 addresses are becoming limited.

---

## IPv6 (Internet Protocol Version 6)

IPv6 was developed to solve the problem of IPv4 address exhaustion.

### Characteristics

- 128-bit address
- Written in hexadecimal format
- Much larger address space

Example:


2001:0db8:85a3:0000:0000:8a2e:0370:7334


Total possible IPv6 addresses:


2^128


This provides an extremely large number of available addresses.

---

## Public vs Private IP Address

### Public IP Address

A **public IP address** is assigned to a device that is directly accessible on the internet.

Characteristics:

- Globally unique
- Assigned by Internet Service Providers (ISP)
- Used for internet communication

Example:


8.8.8.8


---

### Private IP Address

A **private IP address** is used inside local networks and cannot be accessed directly from the internet.

Private IP ranges include:


10.0.0.0 – 10.255.255.255
172.16.0.0 – 172.31.255.255
192.168.0.0 – 192.168.255.255


Example:


192.168.1.5


Private IPs are commonly used in home and office networks.

---

## Static vs Dynamic IP Address

### Static IP Address

A **static IP address** remains fixed and does not change.

Characteristics:

- Manually configured
- Used for servers and infrastructure
- Useful for hosting services

Example use cases:

- Web servers
- Database servers
- DNS servers

---

### Dynamic IP Address

A **dynamic IP address** is automatically assigned by a **DHCP (Dynamic Host Configuration Protocol) server**.

Characteristics:

- Automatically assigned
- Changes periodically
- Common for home internet users

Most devices on modern networks use dynamic IP addresses.

---

# 3. Subnetting

Subnetting is the process of dividing a large network into smaller logical networks called **subnets**.

It improves:

- Network performance
- Security
- Address management
- Routing efficiency

Subnetting is extremely important in **cloud environments such as AWS, Azure, and GCP**.

---

## CIDR Notation

CIDR (Classless Inter-Domain Routing) represents IP addresses along with their network prefix.

Example:


192.168.1.0/24


Explanation:

- `192.168.1.0` → Network address
- `/24` → First 24 bits represent the network

This means the network has **256 total IP addresses**.

---

## Subnet Mask

A subnet mask determines which part of the IP address represents the network and which part represents hosts.

Example:


Subnet Mask: 255.255.255.0


This corresponds to:


/24


Subnet masks help routers determine how to route traffic between networks.

---

## Network Address

The **network address** identifies the entire network.

Example:


Network: 192.168.1.0


This address cannot be assigned to any device.

---

## Broadcast Address

The **broadcast address** is used to send data to all devices in the network.

Example:


Broadcast: 192.168.1.255


This address is reserved and cannot be assigned to devices.

---

# Conclusion

Understanding networking concepts such as **TCP vs UDP, IP addressing, and subnetting** is essential for DevOps engineers. These concepts help in designing scalable infrastructure, troubleshooting connectivity issues, configuring cloud networks, and managing distributed systems.

Networking knowledge is particularly important when working with:

- Cloud platforms (AWS, Azure, GCP)
- Container orchestration (Kubernetes)
- Microservices architecture
- Load balancing and reverse proxies
- Infrastructure automation
