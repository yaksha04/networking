# Linux Networking Commands for DevOps

## Introduction

Linux provides powerful command-line tools to inspect, troubleshoot, and manage networking. These commands are essential for DevOps engineers working with servers, containers, and cloud infrastructure.

This document covers some of the most commonly used Linux networking commands:

- ip a
- ip route
- ifconfig
- hostname -I

---

# 1. ip a (IP Address Command)

The `ip a` command is used to display all network interfaces and their associated IP addresses.

### Syntax


ip a


### What it Shows

- Network interfaces (eth0, lo, wlan0, etc.)
- IP addresses (IPv4 and IPv6)
- Interface status (UP/DOWN)
- MAC addresses

### Example Output


2: eth0: <BROADCAST,MULTICAST,UP> mtu 1500
inet 192.168.1.10/24


### Use Cases

- Check server IP address
- Verify network interface status
- Debug connectivity issues

---

# 2. ip route (Routing Table)

The `ip route` command is used to display and manage the routing table.

### Syntax


ip route


### What it Shows

- Default gateway
- Network routes
- Interface used for routing

### Example Output


default via 192.168.1.1 dev eth0
192.168.1.0/24 dev eth0 proto kernel scope link


### Key Terms

- **default** → Default gateway
- **via** → Gateway IP
- **dev** → Network interface

### Use Cases

- Check internet routing
- Diagnose network issues
- Verify gateway configuration

---

# 3. ifconfig (Interface Configuration)

The `ifconfig` command is used to display or configure network interfaces.

> Note: `ifconfig` is deprecated in many modern Linux systems. Use `ip` command instead.

### Syntax


ifconfig


### What it Shows

- Interface details
- IP address
- MAC address
- RX/TX statistics

### Example Output


eth0 Link encap:Ethernet
inet addr:192.168.1.10


### Use Cases

- Legacy systems
- Quick interface check
- Network debugging

---

# 4. hostname -I (Get System IP)

The `hostname -I` command is used to display the system’s IP addresses.

### Syntax


hostname -I


### Output


192.168.1.10


### Use Cases

- Quickly get server IP
- Useful in scripts
- Automation tasks

---

# Comparison of Commands

| Command | Purpose | Modern Usage |
|--------|--------|--------------|
| ip a | Show interfaces and IPs | Recommended |
| ip route | Show routing table | Recommended |
| ifconfig | Interface details | Deprecated |
| hostname -I | Show IP only | Useful |

---

# Conclusion

These Linux networking commands are essential for DevOps engineers to monitor and troubleshoot network-related issues.

In real-world scenarios, these commands help in:

- Debugging server connectivity
- Configuring cloud instances
- Working with Docker and Kubernetes networking
- Managing production infrastructure
