1. VPC (Virtual Private Cloud)
What is a VPC?

A VPC is your own private network inside AWS.

Think of AWS as a huge city.

AWS Account = Your land
VPC = Your private colony
EC2 Instances = Houses inside colony

A VPC provides:

IP Address Range
Subnets
Routing
Security
Internet Access Configuration

Example:

VPC CIDR:
10.0.0.0/16

This means:

10.0.x.x

belongs to this VPC.

Interview Answer

A VPC is a logically isolated virtual network in AWS where we can launch resources such as EC2 instances, RDS databases, load balancers, and configure networking components like subnets, route tables, internet gateways, and security controls.

2. CIDR Notation

CIDR = Classless Inter-Domain Routing

CIDR decides:

Network size
Number of IPs

Example:

10.0.0.0/24

The "/24" means:

255.255.255.0

Available IPs:

10.0.0.0
to
10.0.0.255

Total:

256 IPs

Usable:

251

(AWS reserves 5)

Common CIDR Sizes
CIDR	Total IPs
/16	65,536
/24	256
/25	128
/26	64
/27	32
/28	16
Interview Question
Why do we use CIDR?

Answer:

CIDR defines the IP address range available in a VPC or subnet and helps with efficient IP allocation.

3. Subnets

Subnet = Smaller network inside VPC

Example:

VPC:

10.0.0.0/16

Subnets:

10.0.1.0/24
10.0.2.0/24
10.0.3.0/24

Each subnet belongs to one Availability Zone.

Public Subnet

Has route to internet.

Example:

0.0.0.0/0 → IGW

Resources:

Web Servers
Bastion Hosts
Public Load Balancers
Private Subnet

No direct internet access.

Resources:

Database
Internal Services
Backend Servers
Interview Answer

A subnet is a segment of a VPC IP range. Public subnets have routes to an Internet Gateway, while private subnets do not.

4. Public vs Private Subnet
Public Subnet

Requirements:

Route to IGW
Public IP attached
Internet
    |
   IGW
    |
Public Subnet
    |
  EC2
Private Subnet

No route to IGW.

Internet
   X
Private Subnet
      |
     RDS
Interview Question

How do you identify a public subnet?

Answer:

A subnet becomes public when its route table contains a route to an Internet Gateway and resources have public IPs.

5. Route Table

Route table decides:

"Where should traffic go?"

Example:

Destination	Target
10.0.0.0/16	Local
0.0.0.0/0	IGW
Local Route

Automatically added.

10.0.0.0/16 → Local

Allows communication inside VPC.

Default Route
0.0.0.0/0

Means:

"Anywhere else"

Interview Answer

A route table contains routing rules that determine where network traffic is directed.

6. Internet Gateway (IGW)

IGW connects VPC to internet.

Without IGW:

EC2
 |
VPC
 |
No Internet

With IGW:

Internet
   |
  IGW
   |
  VPC
Features
Highly Available
Managed by AWS
Horizontally Scalable
Interview Question

Can a public subnet work without IGW?

Answer:

No. Without an Internet Gateway, traffic cannot reach the public internet.

7. NAT Gateway vs NAT Instance
Problem

Private servers need internet access for:

Updates
Package downloads
Docker image pulls

But should not be accessible from internet.

NAT Gateway

AWS managed service.

Private EC2
      |
NAT Gateway
      |
Internet
Benefits
Managed
Scalable
Highly Available
NAT Instance

EC2 acting as NAT.

Private EC2
      |
 NAT EC2
      |
 Internet
Comparison
Feature	NAT Gateway	NAT Instance
Managed	Yes	No
HA	Yes	No
Maintenance	AWS	User
Scalability	Auto	Manual
Performance	Better	Lower
Interview Answer

NAT Gateway is AWS-managed and recommended, while NAT Instance is an EC2 instance configured for NAT functionality requiring manual management.

8. Security Groups vs NACL

This is one of the MOST ASKED questions.

Security Group

Acts like firewall on instance level.

Internet
   |
Security Group
   |
 EC2
Characteristics
Stateful
Allow Rules Only

Example:

Allow:

80
443
22

Response traffic automatically allowed.

NACL

Acts at subnet level.

Internet
   |
 NACL
   |
Subnet
   |
EC2
Characteristics
Stateless
Allow and Deny Rules

Must explicitly allow return traffic.

Difference
Feature	SG	NACL
Level	Instance	Subnet
Stateful	Yes	No
Allow Rules	Yes	Yes
Deny Rules	No	Yes
Evaluation	All Rules	Rule Number Order
Interview Answer

Security Groups operate at instance level and are stateful. NACLs operate at subnet level and are stateless and support both allow and deny rules.

9. VPC Peering

Connects two VPCs.

VPC-A  <----->  VPC-B

Private communication.

Limitation

No Transitive Routing

Not allowed:

A -> B -> C

A cannot automatically talk to C.

Interview Answer

VPC Peering provides private connectivity between two VPCs using AWS backbone infrastructure.

10. VPN

Virtual Private Network

Connects:

Office
   |
 Encrypted Tunnel
   |
 AWS VPC

Uses Internet.

Advantages
Cheap
Easy
Disadvantages
Internet latency
11. Direct Connect

Dedicated connection between data center and AWS.

Company
    |
Direct Connect
    |
 AWS

No public internet.

Advantages
Low latency
Consistent performance
High bandwidth
Interview Question

VPN or Direct Connect?

Answer:

VPN uses encrypted tunnels over the internet, whereas Direct Connect provides a dedicated private connection to AWS with lower latency and higher reliability.

12. DNS Basics

DNS converts:

google.com

to

142.x.x.x

Process:

Browser
   |
DNS Resolver
   |
Root Server
   |
TLD Server
   |
Authoritative DNS
   |
IP Address
Important DNS Records
A Record
Domain → IPv4
AAAA Record
Domain → IPv6
CNAME
Alias

Example:

www.example.com
→ example.com
MX

Mail Server

TXT

Verification records

13. Route 53

AWS DNS Service

Amazon Web Services Route 53 performs:

Domain Registration
DNS Routing
Health Checks
Routing Policies
Simple

Single record

Weighted
70% Traffic → Server A
30% Traffic → Server B
Latency Based

Sends users to nearest region.

Failover

Primary down?

Automatically send to backup.

Geolocation

Traffic based on country.

Interview Answer

Route 53 is AWS's highly available DNS service that supports domain registration, DNS management, health checks, and advanced routing policies.

14. Load Balancers

A Load Balancer distributes traffic.

Users
  |
Load Balancer
 /   \
EC2  EC2

Benefits:

High Availability
Fault Tolerance
Scalability
ALB (Application Load Balancer)

Layer 7

Works with:

HTTP
HTTPS

Can route based on:

URL Path
Host Header

Example:

/api → Backend
/images → Storage
Use Cases
Web Applications
Microservices
Kubernetes Ingress
NLB (Network Load Balancer)

Layer 4

Works on:

TCP
UDP
TLS

Very high performance.

Millions of requests/sec.

Use Cases
Gaming
Real-time apps
Financial systems
CLB (Classic Load Balancer)

Older generation.

Supports:

Layer 4
Layer 7

Not recommended for new deployments.

Comparison
Feature	ALB	NLB	CLB
OSI Layer	7	4	4/7
HTTP Routing	Yes	No	Limited
Path Routing	Yes	No	No
Host Routing	Yes	No	No
Performance	High	Very High	Moderate
New Applications	Yes	Yes	No
One Interview Scenario (Very Important)

Question: Design a 3-tier application in AWS.

Answer:

VPC (10.0.0.0/16)

Public Subnets:
  ALB
  NAT Gateway

Private Subnets:
  Application Servers

Private Subnets:
  RDS Database

Security Groups:
  ALB → App
  App → DB

Internet Gateway:
  Public Access

Route Tables:
  Public → IGW
  Private → NAT

Route53:
  DNS

Auto Scaling:
  App Layer
