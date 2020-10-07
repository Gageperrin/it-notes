# Networking and Hybrid

## Table of Contents
* [Border Gateway Protocol](#border-gateway-protocol)
* [AWS Global Accelerator](#aws-global-accelerator)
* [Site to Site VPN](#site-to-site-vpn)
* [Transit Gateway](#transit-gateway)
* [Advanced VPC Routing](#advanced-vpc-routing)
* [Accelerated Site-to-Site VPN](#accelerated-site-to-site-vpn)
* [Direct Connect](#direct-connect)
* [DNS Fundamental](#dns-fundamentals)
* [Route 53 Fundamentals](#route-53-fundamentals)
* [VPC Endpoints](#vpc-endpoints)
* [AWS PrivateLink](#aws-privatelink)
* [VPC IPv6](#vpc-ipv6)
* [Advanced VPC Structure](#advanced-vpc-structure)

## Border Gateway Protocol

Border Gateway Protocol (BGP) is a routing protocol used by Direct Connect and Dynamic Site to Site VPNs. BGP is made up of autonomous systems (AS)--routers controlled by one entity. ASN are unique and allocated by IANA (0-65535), 64512-65534 are private and can be used in private peering relationships. 

BGP is reliable and operates over tcp/179, but it is not automatic and peering has to be manually configured. BGP is a path-vector protocol that exchanges the best path to a destination between peers, the path is called the ASPATH. iBGP is an internal BGP routing within an AS. eBGP is an external BGP routing between multiple AS.

BGP exchanges the shortest ASPATH by default even if a longer fibre would provide better perforamnce. To get around this, AS Path Prepending can artificially make a path look longer to have the BGP route through an alternative higher performance path.


## AWS Global Accelerator

> [AWS Global Accelerator](https://docs.aws.amazon.com/global-accelerator/latest/dg/what-is-global-accelerator.html) is a service in which you create accelerators to improve availability and performance of your applications for local and global users. Global Accelerator directs traffic to optimal endpoints over the AWS global network. This improves the availability and performance of your internet applications that are used by a global audience. Global Accelerator is a global service that supports endpoints in multiple AWS Regions

* Global Accelerator is designed to improve global network performance by offering entry point onto the global AWS transit network as close to customers as possible using Anycast IP addresses
* Anycast IP’s allow a single IP to be in multiple locations. Routing moves traffic to closest location
* Traffic initially uses public Internet and enters a Global Accelerator edge location using anycast IPs. From the edge, data transits globally across the AWS global backbone network to 1+ locations. **Can be used for TCP/UDP (not HTTP/S)** – different from CloudFront. Does not cache anything

## Site to Site VPN

> By default, instances that you launch into an Amazon VPC can't communicate with your own (remote) network. You can enable access to your remote network from your VPC by creating an [AWS Site-to-Site VPN](https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN.html) (Site-to-Site VPN) connection, and configuring routing to pass traffic through the connection.
>
>Although the term VPN connection is a general term, in this documentation, a VPN connection refers to the connection between your VPC and your own on-premises network. Site-to-Site VPN supports Internet Protocol security (IPsec) VPN connections.

* A VPN is a logical connection between a VPC and on-premises networking encryption using IPSec over the public Internet. HA if designed and implemented correctly.
* Can be provisioned in less than an hour.
* [Virtual Private Gateway (VGW)](https://docs.aws.amazon.com/vpn/latest/s2svpn/SetUpVPNConnections.html#vpn-create-target-gateway): a target on one or more route tables.
* [Customer Gateway (CGW)](https://docs.aws.amazon.com/vpn/latest/s2svpn/SetUpVPNConnections.html#vpn-create-cgw): a logical configuration in AWS or the physical device on-premises.
* The VPN connection is between the VGW and CGW.
* Static VPNs uses static routes in the route table and static configuration. No load balancing or multi-connection failover.
* Dynamics VPNs: BGP is configured on both customer and AWS side using ASN. Multiple VPN Connecitons provide HA and traffic distribution. Requires BGP support.
* VPNs and VGWs have a speed cap of 1.25 GB/s.
* Latency can lead to inconsistency, it is over the public Internet.
* Billing: AWS hourly cost, GB cost, data cap (on premises).
* Advantages: Fast set-up, all configuration is software. Can be either a bakcup for Direct Connect (DX).

## Transit Gateway (TGW)
> A [transit gateway](https://docs.aws.amazon.com/vpc/latest/tgw/what-is-transit-gateway.html) is a network transit hub that you can use to interconnect your virtual private clouds (VPC) and on-premises networks.

TGW is a network transit hub that connects VPCs to each other and to on-premises networks. Significantly reduces network complexity. Single network object becomes HA and scalable. Can make attachments to other network types: VPC, Site-to-Site VPN, and Direct Connect Gateway. VPC Attachments are configured with a subnet in each AZ where service is required.

TGW can integrate with Direct Connect gateway using transit VIF and can build peering attachments both cross-region and cross-account. TGW supports multiple route tables and transitive routing to create global networks. Cross-account sharing implemented through AWS RAM.

## Advanced VPC Routing

Subnets are associated with one route table only. IPv4 and IPv6 addresses are handled separately within a route table. Routes send trafic based on destination to a target. All routes are evaluted and the highest priority match is used.

Subnet route tables determine what the VPC router does with IP traffic. Static routes are added manually, and propagated routes are added by VGW. Priority order:
1. Longest prefix
2. Static route
3. Propagated
4. DX
5. VPN Static
6. VPN BGP
7. AS Path

Public subnets have a default route targeting IGW; private subnets target NAT Gateway. A more specific route can take priority for 192.167.10.0/24 using the VGW as a target.

CIDR Overlap can make VPCs unreachable. Can be solved with multiple route tables in different subnets in a single VPC but this prevents subnets from accessing other VPC. Can be solved with a single route table that has two routes, one with a longer prefix one with a shorter, both with the same IP address referent.

Ingress routing assigns a route for incoming traffic. Gateway route tables can forward inbound traffic (e.g. for security inspection).

## Accelerated Site-to-Site VPN

An accelerated Site-to-Site VPN is composed of two resilient public space endpoints.

Traditionally, there are two logical IPSEC tunnels between VGW and CGW but physically it is indirect and inconsistent. With Accelerated S2S VPN, tunnel IPs are global and connections are routed to the closest global accelerator edge location.

Accelerations can be enabled when creating a TGW VPN attachment, but it is not compatible with VGW VPNs.

* Billing: Fixed accelerator cost + transfer fee.
* Use case: Use by default with TGW, preferred over VGW.

## Direct Connect

>[AWS Direct Connect](https://docs.aws.amazon.com/directconnect/latest/UserGuide/Welcome.html) links your internal network to an AWS Direct Connect location over a standard Ethernet fiber-optic cable. One end of the cable is connected to your router, the other to an AWS Direct Connect router. With this connection, you can create virtual interfaces directly to public AWS services (for example, to Amazon S3) or to Amazon VPC, bypassing internet service providers in your network path. An AWS Direct Connect location provides access to AWS in the Region with which it is associated. You can use a single connection in a public Region or AWS GovCloud (US) to access public AWS services in all other public Regions.

Direct Connect links network to AWS with a physical fiber. Customer chooses speed of either 1 GB/s (1000BASE-LX) or 10 GB/s (10GBASE-LR) and a DX location. AWS allocates a DX port in that DX location. It then requests a cross-connect into network.

Virtual Interface (VIF) - VLAN and BGP Session (usually terminated at customer-premises router). Private VIF connect to VPC and Public VIF to Public Zone Services, but not over the public Internet.

* Speed range between 50 MB/s and 10 GB/s.
* Can be ordered through a partner, but there are restrictions.
* A partner hosted connection is a DX connection with **one** VIF.
* A partner hosted VIF is a single VIF (no connection) with shared bandwidth.
* Can be set up in days.
* **No encryption** with DX but can use Public VIF and Site-to-Site VPN for security.
* No sharing Internet data cap or bandwidth.
* No transit over public Internet so low/consistent latency.
* Cheaper data transfer.

### Direct Connect Resilience and HA

DX has no default resilience. A DX location is connected to an AWS region with a single cross-connect DX port and a customer provided router. DX is extended from DX location to customer premises. With this setup there are single points of failure at DX Location, DX Router, Cross Connect, Customer DX Router, Extension, Customer Premises, and the Customer Router.

Decent Resilience architecture: Two AWS DX routers, two customer DX routers, two customer premises routers. However, DX Location, Customer premises, and extension link path can still fail.

Good Resilience architecture: Add two customer premises with two DX locations. Failure if an entire location fails and a router in that location.

Great Resilience architecture: Add two DX routers for each DX location (4 total). Dual extension to each premises (4 total on two premises). Can add Site-to-Site VPN as additional VPN.

### Direct Connect Link Aggregation Groups (DX LAG)

> You can use multiple connections for redundancy. A [link aggregation group (LAG)](https://docs.aws.amazon.com/directconnect/latest/UserGuide/lags.html) is a logical interface that uses the Link Aggregation Control Protocol (LACP) to aggregate multiple dedicated connections at a single AWS Direct Connect endpoint, allowing you to treat them as a single, managed connection. LAGs streamline configuration because the LAG configuration applies to all connections in the group.

Multiple physical connections can have a speed up to 40 GB/s. (Speed x n of connections)

LAG has speed and admin overhead features, but not as much resilience. Active/Active architecture has a **maximum of four connections per LAG**. All connections need to be the same speed and terminate at the same location. A LAG has a 'minimumLinks' attribute--the LAG is active as long as this value or more connections are active.

### Direct Connect Gateway and Transit VIFs

DX is a regional service that bridges customer premises to 1+ DX Locations. Public VIFs can access all AWS Public Regions. Private VIFs can only access VPCs in the same AWS Region via VGWs by pointing to the public Internet then letting more specific routes take priority.

DX without Gateway: Two VIFs, locations, sets of equipment and extensions. Not scalable.

[DX Gateway](https://docs.aws.amazon.com/directconnect/latest/UserGuide/direct-connect-gateways-intro.html): Global network device that is accessible in all regions. DX integrates via one private VIF. DX Gateway uses VGW associations globally. VPC <=> On Premises. No routing to other VPCs on same DX Gateway. 

DX Gateway is a global object in a public region. GVW associates with the DX gateway. Private VIF association between the DX Gateway and DX Location. DX Location connected to customer premises.

DX Gateway + TGW: Create a transit VIF - 1 per DX Connection. Associated with the DX Gateway. Allows 3 TGW's to be attached to the DX Gateway. TGW's can be peered: supports transitive routing. On premises location receives BGP advertisements for all VPCs via the TGW via the DX gateway.

Combinations:
* 10 VGWs per DX Gateway (10 VPCs).
* 1 DX can have many private VIFs -> Many DX Gateways (up to 500 VPCs).
* 5000 VPs per TGW x n of peers x 3 TGW per DX Gateway = many, many VPCs


## DNS Fundamentals

Domain Name Service (DNS) is a discovery service with 4,294,967,296 IPv4 addresses and 340 undecillion IPv6 addresses.

Terms:
* DNS Client: Device that wants the data
* Resolver: Software on device or server that queries DNS
* Zone: A part of the DNS database
* Zonefile: Physical database for a zone
* Nameserver: Where zonefiles are hosted
* Root Hints: Configuration points at the root server's IPs and addresses
* Root Server: Hosts the DNS root zone
* Root Zone: Points at TLD authoritative servers
* gTLD: Generic top level domains (.com, .org)
* ccTLD: Country-code top level domain (.ca, .de, .se)


## DNS Record Types

* Nameserver (NS) - DNS delegation
* A  - Maps host onto IPv4 address
* AAAA - Maps host onto IPv6 address
* MX - Mail record
* TXT - Contains text data used for authentication purposes
* TTL - Time to live. How long DNS records can be cached for. High values = fewer queries, less control. Low values = more queries.

MX records have a priority and a value. Value can point inside the same zone or have a fully qualified domain name to point outside the zone (executes MX Query). Lower values in priority field are higher priority. Uses this priority to find a mail server and send mail through SMTP.


## Route 53 Fundamentals (R53)

>[Amazon Route 53](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/Welcome.html) is a highly available and scalable Domain Name System (DNS) web service. You can use Route 53 to perform three main functions in any combination: domain registration, DNS routing, and health checking. 

Route53 registers domains and host zones with managed nameservers. It is a global service with a single database and is globally resilient.

Zone files in AWS are called hosted zones. They can either be public or private and linked to a VPC. Hosted zone stores records (recordsets).

`enableDnsHostnames` indicates whether the instances launched in the VPC get public DNS hostnames. If this attribute is true , instances in the VPC get public DNS hostnames, but only if the enableDnsSupport attribute is also set to true. `enableDnsSuport` Indicates whether the DNS resolution is supported for the VPC. If this attribute is false, the Amazon-provided DNS server in the VPC that resolves public DNS hostnames to IP addresses is not enabled. If this attribute is true , queries to the Amazon provided DNS server at the 169.254.169.253 IP address, or the reserved IP address at the base of the VPC IPv4 network range plus two ( *.*.*.2 ) will succeed.



### Route 53 Public Hosted Zones

A R53 Hosted Zone is a DNS database for a domain. A public hosted zone is hosted on R54 by provided public DNS servers. Globally resilient because of multiple DNS servers. Created with domain registration via R53.

It hosts DNS records (A, AAAA, MX, NS, TXT), and the hosted zones are authoritative since the DNS system references them. However, Route 53 does not support DNSSEC services. These must be integrated through a third party.

### Route 53 Health Checks

Health checks are separate from but are used by records. Health checkers are located globally and can check anything (even non-AWS) with an IP address over the public Internet. They can be set to check every 10s or 30s or even more frequently at an increased cost. Can check:
* TCP
* HTTP/S
* HTTP/S with String Matching

Types of checks:
* Endpoint
* CloudWatch Alarm
* Check of other checks

### Route 53 Routing Policies

* Simple: Route to a single resource
* Failover: One record is set to be primary, one is set to backup in case of primary failure
* Weighted: Records are given weight value to distribute routing traffic
* Latency-based: Checks database for latency and routes traffic accordingly
* Geo-location: Routes based on continent/country record
* Multi-value: Routing traffic to multiple resources, creating more than one record of the same name and type


## VPC Endpoints

>A [VPC endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints.html) enables you to privately connect your VPC to supported AWS services and VPC endpoint services powered by AWS PrivateLink without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection. Instances in your VPC do not require public IP addresses to communicate with resources in the service. Traffic between your VPC and the other service does not leave the Amazon network.

[Gateway Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-gateway.html):
A gateway that supports:
* Amazon S3
* DynamoDB

For gateway endpoints, **prefix lists are added to a route table**. HA across all AZs in a region by default. Endpoint is used to control what access is permitted. Gateway endpoints are limited by region. Can prevent leaky buckets by setting S3 Buckets to private. Gateway Endpoitns are not accessible outside the VPC.

[Interface Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html):
Interface endpoints are used for every other service. They are an elastic network interface (ENI) with a privaet IP address. It is powered by AWS PrivateLink. No IGW, NAT device, or VGW is needed for interface endpoints.

Interface endpoints are not HA by default; they are added to specific subnets with an ENI. To make it HA, add one endpoint to one subnet per AZ used in the VPC. Network access controlled via Security Groups. Endpoint policies can restrict access.

* TCP and IPv4 traffic only.
* Uses PrivateLink.
* **Uses DNS and Private IPs** instead of a prefix list.
* Endpoint Regional DNS and Endpoint Zonal DNS can both be used by applications.
* PrivateDNS overrides the default DNS for services.

### VPC Endpoint and Bucket Policies

VPC Endpoints allow private VPCs access to an entire service within a region. It limits access via that endpoint and what private VPCs can access. A VPC Endpoint contains a principal and condititons.

* Without an IGW, the gateway endpoint is the only exit point of a VPC.
* An endpoint policy limits what resources or actions the endpoint will allow.
* Can be combined with bucket policies to create private S3 buckets.


## Advanced VPC DNS & DNS Endpoints

.2 is reserved in every subnet and is now called the [Route 53 Resolver](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resolver-getting-started.html). Provides R53 public and associated private zones. Only accessible from within a VPC.

There were historical problems of Hybrid DNS between AWS and On-premises environments.

Before R53 Endpoints:
1. VPC configured to provide the DNS Forwarder via DHCP.
2. Requests arrive and are forwarded to the R53 resolver by default (or selectively as required).
3. On-premises resolver can forward on any queries for non local zones to the DNS Forwarder.
4. The EC2 based DNS forwarder acts as an intermediary for AWS <-> On-premises flow.

Route 53 Endpoints: VPC Interfaces (ENIs) accessible over VPN or DX.

In-bound: On-premises can forward to the R53 Resolver and outbound (conditional fowarders) R53 forwards to On-Presmises. Rules control which requests are forwarded.

Out-bound: Endpoints forward queries to other DNS servers based on defined rules. On-premises is configured to forward queries to forward queries to the R53 inbound interfaces.


## AWS PrivateLink

>[AWS PrivateLink](https://docs.aws.amazon.com/whitepapers/latest/aws-vpc-connectivity-options/aws-privatelink.html) enables you to connect to some AWS services, services hosted by other AWS accounts (referred to as endpoint services), and supported AWS Marketplace partner services, via private IP addresses in your VPC. The interface endpoints are created directly inside of your VPC, using elastic network interfaces and IP addresses in your VPC’s subnets. That means that VPC Security Groups can be used to manage access to the endpoints.

AWS PrivateLink provides private connectivity between VPCs, AWS services, and on-premises applications. A secure endpoint service link between VPC service consumer and VPC service provider. Interface endpoints are secured through security groups and NACLs. Permissions are granted to accounts, IAM users, and IAM roles. 

* It is HA via multiple endpoints.
* IPv4 & TCP traffic only (no IPv6)
* Private DNS is supported
* DX, Site-to-Site VPN, and VPC Peering are supported


## VPC IPv6

Only public IPv4 addresses are routable over the public Internet. Private and public IPv4 and subnets are technically and architecturally distinct in AWS. NAT translates the private IPv4s to public. Services never have public IPv4 addressing in a VPC or EC2, only private addressing configuration. Public addresses are handled by the IGW.

All IPv6 addresses in AWS are publicly routable. NAT is not used with IPv6. Each VPC can be IPv6 enabled with a /56.

IPv4 and IPv6 routing are handled separately. IPv6 can be added while creating a VPC/Subnet or addressing can be migrated to IPv6. Can add VPC or subnet range as well as routes and appropriate gateways. Can configure IPv6 on services.

PrivateLink and Interface Endpoints do not support IPv6.

## Advanced VPC Structure

n of AZs in a region - 1 buffer AZ = n of nominal AZs

Legacy infrastructure uses N-tier architecture. Presentation, logic, an data tiers with their own networking and security. Each tier is isolated with data only crossing tiers through firewalls.

Designing robust VPCs:
* Multiple subnets can be used for HA across AZs, but this needs to be multiplied by the amount of tiers required.
* Ignore HA initially when identifying the number of subnets needed.
* Public and private addressing and security can be controlled with one subnet.
* Different routing needs multiple subnets.
* Internet-facing ALBs can communicate with private instances (needs to be run from public subnet).

Formula: **N of subnets needed = n of application subnets x AZs**

VPC Peering is the best way to route traffic between two VPCs privately. IPSec tunnels do not work between VPCs.
