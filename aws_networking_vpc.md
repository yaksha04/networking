AWS Networking Deep Dive
1. VPC Peering

VPC Peering allows two VPCs to communicate privately using AWS's internal network.

Features
Private IP communication
No internet gateway required
Low latency
Traffic remains inside AWS network
Example
VPC A (10.0.0.0/16)
        |
    Peering
        |
VPC B (172.16.0.0/16)

A server in VPC A can directly connect to a server in VPC B using private IPs.

Advantages
Simple setup
No bandwidth bottleneck
No single point of failure
Limitations
No transitive routing

Example:

VPC A <--> VPC B
VPC B <--> VPC C

VPC A cannot talk to VPC C
Route tables must be updated manually.
Becomes difficult to manage with many VPCs.
Use Cases
Small environments
Dev and Production VPC communication
Cross-account communication
2. AWS Transit Gateway (TGW)

Transit Gateway acts as a central network hub connecting multiple VPCs and on-premise networks.

Architecture
             Transit Gateway
                   |
   --------------------------------
   |              |              |
 VPC-A          VPC-B         VPC-C
                   |
             Direct Connect
                   |
             On-Premises
Benefits
1. Transitive Routing
VPC A --> TGW --> VPC B
VPC A --> TGW --> VPC C

No need for individual peerings.

2. Simplified Management

Instead of:

10 VPCs = 45 Peering Connections

Use:

10 VPCs = 10 TGW Attachments
Use Cases
Enterprise networking
Multi-account architecture
Hybrid cloud
Interview Question

Why Transit Gateway instead of VPC Peering?

Answer:

Supports transitive routing.
Easier management.
Scales to hundreds of VPCs.
Supports VPN and Direct Connect attachments.
3. VPC Flow Logs

VPC Flow Logs capture network traffic information.

Think of them as CloudTrail for networking.

Captured Information

Source IP

Destination IP

Port

Protocol

Packets

Bytes

Accept/Reject status

Example
10.0.1.10
     |
     | TCP 443
     |
10.0.2.15

Flow log record:

srcaddr=10.0.1.10
dstaddr=10.0.2.15
dstport=443
protocol=TCP
action=ACCEPT
Storage Options
CloudWatch Logs
S3
Kinesis Data Firehose
Troubleshooting Example

Server can't connect to database.

Check Flow Logs:

action = REJECT

Indicates Security Group or NACL issue.

Use Cases
Security auditing
Compliance
Troubleshooting
Traffic analysis
4. VPC Endpoints

Endpoints allow private access to AWS services without Internet Gateway or NAT Gateway.

Gateway Endpoint

Supported Services:

S3
DynamoDB
Architecture
EC2
 |
Private Route
 |
Gateway Endpoint
 |
S3
Benefits
Free
Highly scalable
Route table based
Example Route
Destination:
pl-xxxxxxxx

Target:
vpce-xxxxxxxx
Interface Endpoint

Uses AWS PrivateLink technology.

Supported Services
SNS
SQS
CloudWatch
Secrets Manager
KMS
Hundreds of AWS services
Architecture
EC2
 |
Private IP
 |
ENI
 |
AWS Service
Benefits
Private access
No NAT Gateway needed
Supports third-party services
Drawback

Not free.

Hourly charges apply.

Gateway vs Interface Endpoint
Feature	Gateway Endpoint	Interface Endpoint
Services	S3, DynamoDB	Most AWS Services
Uses ENI	No	Yes
Cost	Free	Charged
Route Table Entry	Yes	No
Private IP	No	Yes
5. AWS PrivateLink

PrivateLink enables private connectivity between VPCs, AWS services, and SaaS applications.

Traditional Access
Consumer VPC
      |
   Internet
      |
Provider Service
PrivateLink Access
Consumer VPC
      |
Interface Endpoint
      |
PrivateLink
      |
Provider NLB
      |
Provider Service
Advantages
No public internet exposure
Secure SaaS access
Simplified networking
No CIDR overlap concerns
Real Example

A company offers a logging platform.

Customers connect using:

PrivateLink

instead of public internet.

Services Using PrivateLink
Snowflake
Datadog
MongoDB Atlas
Many SaaS providers
6. Elastic Network Interface (ENI)

ENI is a virtual network card attached to EC2.

Contains
Private IP
Secondary IPs
Public IP
MAC Address
Security Groups
Example
EC2 Instance
      |
    ENI
      |
Private IP
Multiple ENIs
EC2
 | \
 |  \
ENI1 ENI2

Used for:

Multi-homed servers
Firewalls
Network appliances
Failover Scenario

Detach ENI from failed EC2.

Attach to standby EC2.

Same IP retained.

Interview Question

Why use multiple ENIs?

Answer:

Traffic separation
HA architecture
Network appliances
7. Network ACL vs Security Group

This is one of AWS's most common interview questions.

Feature	Security Group	NACL
Level	Instance	Subnet
Stateful	Yes	No
Allow Rules	Yes	Yes
Deny Rules	No	Yes
Rule Evaluation	All Rules	Number Order
Default Behavior	Deny Incoming	Allow All
Performance	Instance-based	Subnet-based
Stateful Example (Security Group)

Inbound:

Allow TCP 443

Request enters.

Response automatically allowed.

No outbound rule needed.

Stateless Example (NACL)

Inbound:

Allow TCP 443

Outbound:

Allow Ephemeral Port 1024-65535

Must configure both directions.

When to Use Security Groups
Primary access control
Application security
When to Use NACLs
Block malicious IP ranges
Subnet-level security
Additional security layer
8. AWS Direct Connect (DX)

Direct Connect creates a dedicated private connection between your data center and AWS.

Traditional Connection
On-Prem
   |
Internet
   |
AWS

Problems:

Latency variation
Public internet risks
Direct Connect
On-Prem
   |
Direct Connect
   |
AWS
Speeds
50 Mbps
100 Mbps
1 Gbps
10 Gbps
100 Gbps
Benefits
Consistent latency
Higher throughput
Lower transfer cost
Private connection
Use Cases
Large enterprises
Financial institutions
Hybrid cloud
9. BGP (Border Gateway Protocol)

Direct Connect uses BGP for route exchange.

BGP is the routing protocol that tells networks:

"Which path should I take to reach a destination?"

Example
AWS Network
      |
     BGP
      |
Corporate Network
How It Works

AWS advertises:

10.0.0.0/16
172.16.0.0/16

Company advertises:

192.168.1.0/24
192.168.2.0/24

Routes are exchanged automatically.

BGP Key Concepts
ASN (Autonomous System Number)

Unique network identifier.

Examples:

AWS ASN = 64512 (private ASN examples)
Enterprise ASN = 65000
Route Advertisement

BGP announces available networks.

Example:

AWS:
10.0.0.0/16 available through me
Route Selection

BGP chooses best path based on:

Local Preference
AS Path Length
MED
Weight
Direct Connect + BGP Architecture
        AWS VPC
            |
       Virtual Gateway
            |
         BGP Session
            |
      Direct Connect
            |
      Corporate Router
            |
      On-Prem Network
High Availability Design
           AWS
          /   \
     DX1       DX2
      |         |
Router1      Router2

Benefits:

Redundancy
Automatic failover
Better uptime
Interview Questions
Difference Between VPC Peering and Transit Gateway?
Peering is point-to-point.
TGW is hub-and-spoke.
TGW supports transitive routing.
Difference Between Gateway Endpoint and Interface Endpoint?
Gateway = S3/DynamoDB only.
Interface = Most AWS services.
Interface uses ENI and PrivateLink.
Security Group vs NACL?
SG = Stateful, instance level.
NACL = Stateless, subnet level.
Why Use Direct Connect?
Private dedicated connectivity.
Lower latency.
Better reliability.
What Protocol Does Direct Connect Use?
BGP (Border Gateway Protocol).
What Is AWS PrivateLink?
Private access to AWS/SaaS services through Interface Endpoints without exposing traffic to the internet.
