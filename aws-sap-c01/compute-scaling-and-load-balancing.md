# Computer, Scaling & Load Balancing

## Regional and Global AWS Architecture

AWS has global service location and discovery featuring content delivery (CDN) and optimization. There are global health checks and failovers.

* Regional entry pointing
* Scaling and resilience
* Application services and components

DNS is used for service discovery and regional based health checks and request routing. CDNs are used to cache content glboally as close to end user as possible to improve performance.

Tiers:
* Web Tier
* Compute Tier
* Storage Tier
* Database Tier
* Caching Layer
* App Services

## EC2 Purchase Options

### On-Demand

Instances are isolated but multiple customer instances run on shared hardware. Per second billing while an instance is running. Associated resources bill regardless of instance statement. On-Demand is the default option with no interruption, no capacity reservation, no upfront cost, no discount, predictable pricing. Used for short or unknown workloads or where applications cannot be interrupted.

### Spot

The cheapest option. Based on spare capacity at a give moment. Never use spot for workloads which cannot tolerate interruptions. Used for non time critical workloads and anything which can be re-run. Bursty capacity needs cost sensitive workloads. Good for anything stateless.

### Reserved

Long-term consistent usage. No reservation full per/second price. Matching instance, reduced or no per second price. Unused reservations are still billed. Partial coverage of larger instance. Reservations last one to three years.

No-upfront payment: Some savings for agreeing to term.
All-upfront: No per second fee. Greatest discount
Partial-upfront: Reduced per second fee.

Scheduled reserved instances are for long term usage which do not run constantly (e.g. batch processing). It does not support all instance types or regions. 1,200 hours per year and one year term minimum. 100 hours of EC2 per month.

Capacity reservations are regional reservations that provide a billing discount for valid instances launched in an AZ in that region. They do not reserve capacity within an AZ. Zonal reservations only apply to one AZ providing billing discounts and capacity reservation in that AZ.


### EC2 Savings Plan

This is an hourly commitment for a 1 or 3 year term. A reservation of general compute $ amounts or a specific EC2 Savings Plan. Compute Products (EC2, Fargate, Lamba) Products have an on-demand rate and a savings plan rate. Beyond your commitment then on-demand rate is used.

### Capacity Reservations

* On-Demand capacity reservations can be booked to ensure you always have access to capacity in an AZ when you need it – but at full on-demand price. No term limits – but you pay regardless of if you consume it.
* Dedicated Hosts – Pay for host, no instance charges. Licensing based on Sockets/cores. Host affinity links instances to hosts.
* Default/Shared: No Host exposure, per second charges, No capacity management required.
* Dedicated Host: Pay for the host. No instance charges. Capacity management required.
* Dedicated Instances: Don’t own or share the host. No other customers share. Extra charges for instances, but dedicated hardware.


## Advanced EC2 Networking

EC2 instances have a primary ENI which cannot be removed, but additional ENIs can be added and removed from other subnets in the same AZ. Multi-ENI infrastructure offers multiple security zones or traffic types. Security groups attached to ENIs offer different protection rules. Each ENI can also be protected by a NACL around its subnet.

An EC2 has:
* One primary private IPv4 address from the subnet range.
* One or more secondary private IPv4 addresses depending on instance type.
* One public IPv4 address, not configured on the OS but allocated to the instance.
* One elastic IP per private IPv4. If removed, it is gone forever. Elastic IPs are charged if dissociated or instance is not running.
* One or more IPv6 address (all publicly routable)
* One MAC Address
* One or more security groups

Other key features:
* SRC/DST check drops packets if the source or destination address is not on that interface.
* Management of isolated networks
* Software licensing (MAC)
* Security or network applications
* Multi-homed instances with workloads/roles on specific subnets


## Bootstrapping vs AMI Baking

Elastic applications have a scaling event to launch an EC2 instance then has to install, configure, and test software. This can generate either a READY/COMPLETE or a FAIL state.

Launching Needs:
* Base dependencies and app is generic and slow. Needs to be optimized for speed.
* App updates are not fast and mostly generic.
* App config is unique and fast. Needs flexibility.

Bootstrapping: Provisions an EC2 instance. Adds a script to user data, the script runs. It is flexible but takes time, and then the instance is ready for service.

AMI Baking: Launch a master EC2 instance, perform time consuming tasks at this stage. Create an AMI from that instance, customize AMI using User Data.

Combination: Provision a base EC2, perform time consuming activities, create an AMI, customize AMI using User Data


## Elastic Load Balancer (ELB) Architecture
>[Elastic Load Balancing](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/what-is-load-balancing.html) distributes incoming application or network traffic across multiple targets, such as Amazon EC2 instances, containers, and IP addresses, in multiple Availability Zones. Elastic Load Balancing scales your load balancer as traffic to your application changes over time. It can automatically scale to the vast majority of workloads.

ELBs are configured to run in 2+ AZ's, 1+ nodes are placed into a subnet in each AZ and scale with load. Each ELB is configured with an A record DNS name. This resolves to the ELB nodes. ELBs can be Internet-facing or internal; this determines public vs. private IP addresses.

Load balancers are configured with listeners which accept traffic on a port and protocol and communicate with targets. Internet facing load balancing nodes can access public and private EC2 instances and have a public IPv4 address. Load balancers need 8+ free IPs per subnet and /27 or larger subnet to allow for scaling. (/28 technically does work but has less room). Cross-Zone load balancers can send traffic across zones to various instances.

### ELB Evolution

Three types of ELB:
1. [Classic Load Balancer](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/load-balancer-types.html#clb) (v1) - 1 SSL per CLB, lacks many features, not layer 7. Obsolete, avoid whenever possible.
2. [Application Load Balancer](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/load-balancer-types.html#alb) (v2) - HTTP/S/Websocket traffic
3. [Network Load Balancer](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/load-balancer-types.html#nlb) (v2) - TCP, TLS, and UDP traffic


### ALB vs NLB

CLBs do not scale, so every unique https requires a separate CLB. In contrast, ALB and NLB support rules and target groups. Host based rules using SNI and an ALB allows consolidation.

#### Application Load Balancer

ALB layer 7 load balancer that listens on HTTP/S. No other Layer 7 protocols (SMTP, SSH, Gaming) and no TCP/UDP/TLS listeners. SSL/TLS is always termianted on the ALB. No unbroken SSL since a new connection is made to the application. ALBs must have SSL certs if HTTPS is used. ALBs are slower than NLB, more levels of the network stack to process. ALBs can have health checks to evaluate application health at layer 7.

ALBs have rules for direct connections which arrive at a listener and are processed in priority order. The default catch-all is the last rule to be processed. 

Rule conditions:
* `host-header`
* `http-header`
* `http-request-method`
* `path-pattern`
* `query-string`
* `source-ip`

Actions:
* `forward`
* `redirect`
* `fixed-response`
* `authenticate-oidc`
* `authenticate-cognito`

#### Network Load Balancer

NLBs are layer 4 load balancers. No visibility or understanding of HTTP/s. No headers, cookies, session stickiness. Very fast though with millions of requests per second and 25% of ALB's latency. Good for SMTP, SSH, Game Servers, financial apps.

Health checks for NLBs just check ICMP/TCP handshake, so it is not app aware. NLB's can have static IPs, useful for whitelisting. Forward TCP to instances with unbroken encryption. Used with private link to provide services to other VPCs.

Use NLB for unbroken encryption, static IP for whitelisting, fastest performance, operate on non-HTTP/s protocols or use PrivateLink. In other cases, use ALB.


## Auto Scaling Group (ASG)

Automatic scaling and self-healing tools for EC2 that uses launch templates or configurations with minimum, desired, and maximum size (e.g. 1:2:4). ASGs keep running instances at the desired capacity by provisioning or terminating.

Scaling policies automate this based on metrics:
* Manual scaling
* Scheduled scaling (time-based)
* Dynamic scaling: Simple - Two rules, Stepped - Bigger +/- based on difference, Target tracking: desired aggregate CPU = 40%)

Cooldown periods cause ASG to wait between actions before taking the next step. It measures health using EC2 instance checks. ASG instances are automatically added to or removed from the target grouping. ASG can use Load Balancer health checks too. Launch and terminate: `SUSPEND` and `RESUME`.

* `AddtoLoadBalancer` - add to load balancer on launch
* `AlarmNotification` - accept notification from CloudWatch
* `AZRebalance` - Balance instances across AZs
* `HealthCheck` - Toggle health checks on or off
* `ReplaceUnhealthy`
* `ScheduledActions`
* `Standby` Use this for instances on standby

ASGs are free, only the created resources are billed. Cooldowns avoid rapid scaling. Granularity allows for more precision with more, smaller instances. Use with ALBs for elasticity and abstraction. ASG defines when and where, LT defines what.

### ASG Lifecyle Hooks

Lifecycle hooks are custom actions on instances in the middle of an ASG instance launch or termination transition. Instances are paused within the flow, and they wait unitl a timeout then continue or abandon. Lifecycle hooks use EventBridge or SNS Notifications.

Stages: 
* Pending -> Pending: Wait -> Pending: Proceed -> InService
* Terminating -> Terminating: Wait -> Terminating: Proceed -> Terminated


## EC2 Placement Groups

### Cluster Placement Groups

Cluster packs instances close together in **one AZ** for low-latency network performance for tightly-coupled node-to-node communication typical of HPC applications. Cluster has the same rack, sometimes the same host. **10 GB/s per stream** versus the standard 5 GB/s. Lowest latency and max PPS possible in AWS. Little to no resilience. Cluster can span VPC peers but this impacts performance. It requires a supported instance type. Best practice to use the same type of instance at the same time.

### Spread Placement Groups

Spread is used for maximum resilience and availability. Each instance has a distinct rack in multiple AZs. Hard limit of **7 instances per AZ** with isolated infrastructure limit. Each network has its own network and power source. Not supported for dedicated instances or hosts.

### Partition Placement Groups

Need separate partitions for more than 7 instances in an AZ. Divided into a max of 7 partitions per AZ. Each partition group can have as many instances as needed. Partition is used for large parallel running processes. While spread reduces admin overhead, partition allows larger scale.

Instances can be placed in a specific partition or be auto placed. Great for topology aware applications such as HDFS, HBase, and Cassandra. Helps contain impact of failure in an application.

