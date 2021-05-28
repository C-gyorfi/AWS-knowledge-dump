# AWS Certified Developer Associate - Knowledge dump

## Content

- [IAM & AWS CLI](#iam-&-aws-cli)
  - [Users and Groups](#users-and-groups)
  - [Policies](#policies)
  - [Access](#access)
  - [More about IAM](#more-about-iam)
  - [Responsibilities](#responsibilities)

- [EC2](#ec2)
  - [EC2 Instance types](#ec2-instance-types)
  - [Security Groups](#security-groups)
  - [Purchasing Options](#purchasing-options)

- [EC2 Instance Storage](#ec2-instance-storage)
  - [EBS Volume Types and Use cases](#EBS-Volume-Types-and-Use-cases)
  - [EBS Multi-Attach](#EBS-Multi-Attach)
  - [EFS / Elastic File System](#EFS-/-Elastic-File-System)
- [Load Balancing and Auto Scaling](#Load-Balancing-and-Auto-Scaling)
  - [Load balancer](#Load-balancer)
  - [Types of Load balancer](#Types-of-Load-balancer)
  - [More about load balancing](#More-about-load-balancing)
  - [SSL/TLS](#SSL/TLS)
  - [Connection Draining](#Connection-Draining)
  - [Auto Scaling Group](#Auto-Scaling-Group)
  - [ASG Scaling Policies](#ASG-Scaling-Policies)
- [RDS](#RDS)
  - [RDS Backups](#RDS-Backups)
  - [RDS Autoscaling](#RDS-Autoscaling)
  - [RDS Read Replicas](#RDS-Read-Replicas)
  - [RDS Multi AZ](#RDS-Multi-AZ)
  - [RDS Security and encryption](#RDS-Security-and-encryption)
  - [RDS Network](#RDS-Network)
  - [RDS Access Management](#RDS-Access-Management)
- [Amazon Aurora](#Amazon-Aurora)
  - [Aurora Security](#Aurora-Security)
- [Amazon ElastiCache](#Amazon-ElastiCache)
  - [ElastiCache solutions](#ElastiCache-solutions)
  - [Cache Security](#Cache-Security)
  - [REDIS VS MEMCACHED](#REDIS-VS-MEMCACHED)
  - [More about cacheing](#More-about-cacheing)
- [Route 53](#Route-53)
  - [Routing Policies](#Routing-Policies)

## IAM & AWS CLI
- **IAM** stands for **Identity and Access Management**
- It is a Global service

### Users and groups
- Users (people within the organization)
- Groups -> can contain one or more users(one user can be in multiple group)
- Assigned by policies(JSON format) which gives permissions of the users
- Follows the least privilege principle

### Policies
- Managed policies(created by AWS, e.g IAMReadOnlyAccess-> user can view IAM roles)
- Password Policy - AWS allows setup password policy e.g required chars, allow users to change, expire after a given time or require MFA

### Access
- AWS console
- AWS CLI
- AWS SDK

### More about IAM
- IAM Roles allows AWS services to perform actions
- IAM Credentials Report - report on users and status of their credentials
- IAM Access Advisor - shows the service permissions of a given user and last usage

### Responsibilities
- AWS is responsible for:
> Infrastructure and global network security, configuration/vulnerability analysis and compliance validation

- Organisation/User is responsible for:
>Managing/Monitoring Users, Groups, Roles, Policies, enabling MFA and rotating keys and reviewing permissions

# [Back to content](#content)

## EC2
Elastic Compute Cloud(infrastructure as a service)
Related services:
- Virtual machines (EC2)
- Virtual drives (EBS)
- Load balancer (ELB)
- Auto-scaling group (ASG)

Storage options:
- network-attached(EBS or EFS)
- hardware (EC2 Instance Store)

Can run a bootstrap script at first boot to setup(EC2 User data script)

### EC2 Instance Types
- General Purpose
- Compute Optimized
- Memory Optimized
- Storage Optimized

### Security Groups
- Security Groups control what is allowed into / out of the instances
- Referenced by IP or by name
- Control access to Ports
- Control authorized IP ranges(IPv4, IPv6)

### Purchasing Options
- On-Demand Instances: no long term commitment, pay per min (or hours on windows)
- Reserved: (min 1 or 3 years)
  - Reserved Instances: long workloads
  - Convertible Reserved Instances: long workloads with flexible instances
  - Scheduled Reserved Instances: specific time
- Spot Instances - can be terminated any time
- Dedicated Hosts - entire physical server
- Dedicated Instances - no hardware share

# [Back to content](#content)

## EC2 Instance Storage
EC2 Instance Storage are high-performance hardware disk(EBS are network drives with limited performance)
- ephemeral storage(loses data once they got stopped)
- use-cases: buffer / cache / scratch data / temp storage

### EBS Volume Types and Use cases
- `General Purpose SSD`: cheap and low latency, good for System boot volumes,Virtual desktops, Dev environments
- `Provisioned IOPS (PIOPS) SSD`: sustained IOPS or applications that need more than 16,000 IOPS, e.g databases workloads (sensitive to storage perf and consistency)
- `Hard Disk Drives (HDD)`: cannot be boot volume, throughput optimized HDD (st1)
good for big data, data Warehouses or cold HDD (sc1) data that is infrequently accessed. 

### EBS Multi-Attach
- Allows attaching EBS to multiple EC2 instances(same AZ)
- Requirement: file system that’s cluster-aware

### EFS / Elastic File System
- NFS (network file system) that can be mounted on many EC2
- Can be used multi-AZ
- use-cases: content management, web serving, data sharing
- only works with Linux based AMI

# [Back to content](#content)

## Load Balancing and Auto Scaling
> Vertical Scalability - increasing the size of an instance(hardware limits)

> Horizontal Scalability - increasing the number of instances

> High Availability - running the app in multiple data centers(e.g multi AZ)

### Load balancer:
- Distribute load across multiple instances
- Allows using single endpoint (DNS) 
- Does regular health checks on instances
- Handle failure / Terminates unhealthy instances
- SSL termination (HTTPS)
- Can enforce stickiness (by using cookies to forward request to same instance)

### Types of Load balancer:
- Classic Load Balancer(v1) HTTP, HTTPS, TCP
  - has fixed hostname 
- Application Load Balancer(v2) HTTP, HTTPS, WebSocket
  - load balancing multiple application on same machine or across machines
  - support for HTTP/2 and WebSocket
  - allows routing based on path, hostname or query param
  - great for containerized apps(docker or ECS)
  - fixed hostname
  - client of the IP is in the header `X-Forwarded-For`
- Network Load Balancer(v2) TCP, TLS (secure TCP) & UDP
  - Allows forwarding TCP & UDP traffic
  - Can handle a lot(millions of request per seconds)
  - Low latency (~100 ms vs 400 ms for ALB))
  - Has a single static IP per AZ
  - Supports Elastic IP
  - Not free

### More about load balancing:
- Config LB security group to allow public traffic(in most cases)
- Config application security group to allow traffic from LB
- LB can scale but may take time

### SSL/TLS 
- SSL(Secure Sockets Layer) and TLS(Transport Layer Security)
- allows traffic between clients and load balancer to be encrypted
- can manage certificates with ACM (AWS Certificate Manager) - to upload your own
- LB uses uses an X.509 certificate by default
- Clients can use SNI (Server Name Indication) - allows loading multiple SSL certificates onto one web server - works for ALB & NLB but not for CLB(depreciated)

### Connection Draining
- Connection Draining(CLB) or de-registration Delay(for ALB & NLB)
- Time to complete request while instance is de-registering
- Default 300sec can be set 1-3600(depends on avg request completion time)
- set 0 to disable

### Auto Scaling Group
- Matching demand
- Scale out = add instances
- Scale in = remove EC2 instances) 
- Can set minimum and a maximum number of instances
- Can scale an ASG based on CloudWatch alarms(e.g CPU use)
- Free, you pay for the resources you run
- IAM roles attached to an ASG will get assigned to EC2 instances
- Cooldown period to ensure not to launch or terminate additional instances before the previous scaling activity completed

### ASG Scaling Policies
- Target Tracking Scaling (e.g set target CPU usage)
- Step Scaling (e.g CloudWatch alarm is trigger)
- Scheduled Actions (e.g action based on known usage patterns)

# [Back to content](#content)

## RDS
- Relational Database Service(RDS) is a managed database service
- For SQL databases, such as Postgres, MySQL, MariaDB, Oracle, Microsoft SQL Server
- Aurora (AWS Proprietary database)
- Managed means AWS is responsible for: provisioning, OS patching, continuous backups and restore to specific timestamp (Point in Time Restore), Monitoring dashboards
- Read replicas for improved read performance
- Multi AZ setup
- Maintenance windows for upgrades
- Scaling capability (vertical and horizontal)
- Storage backed by EBS (gp2 or io1)

### RDS Backups:
- Automatically enabled
- Daily full back up
- Transaction logs are backed up in every 5 minutes(can restore to any point in time)
- 7 days retention(up to 35)
- Snapshots are manually triggered by user and can keep for unlimited time

### RDS Autoscaling
- Dynamically scaling instance when running out of storage
- User have to set `Maximum Storage Threshold`
- Supports all RDS types

### RDS Read Replicas
- Up to 5 read replicas, can be cross AZ/Region
- Asynchronous replication(takes time to sync up)
- Can promote to replace master
- App needs to use different connection string to read from replica
- Use-case: keep production db unaffected by other reads(replicas are read only)
- Network Cost applied when data replicated in different AZ

### RDS Multi AZ
- Multi AZ is for disaster recovery, increases availability 
- Failover in case an AZ is down
- In this case the second instance is standby / they have one DNS name
- Not for scaling
- Multi AZ replication is free
- To change single AZ to multi AZ is a zero downtime operation, under the hood: a snapshot is taken of the existing DB and and a new DB is restored in a new AZ

### RDS Security and encryption
- Both master and replicas can be encrypted with AWS KMS(AES-256 encryption)
- Master must be encrypted to replicas be encrypted
- Must be enabled at creation
- Transparent Data Encryption (TDE) available for Oracle and SQL Server
- For "in flight" encryption (SSL certificates), provide SSL options with trust certificate when connecting to database.
- To enforce SSL for PostgreSQL, set trough the AWS RDS Console
- To enforce SSL for MySQL, run query(GRANT USAGE ON *.* TO 'mysqluser'@'%' REQUIRE SSL;)
- Snapshots are un-encrypted if the DB is un-encrypted, but can be enabled encryption for snapshots and restore a new db(then migrate the app to use new db and delete old)

### RDS Network
- usually deployed within a private subnet
- using security groups(just like EC2 it controls which IP / security group can communicate with RDS)

### RDS Access Management
- IAM policies can be used to control who can mange the DB
- Username/Password can be used to login to DB
- MySQL & PostgreSQL allows IAM authentication for login to the DB(token obtained through IAM & RDS API calls and has lifetime of 15 mins) - this is good because centrally manage users instead of in the DB

# [Back to content](#content)

## Amazon Aurora
- not open sourced
- supports Postgres and MySQL
- around 5x better performance than MySQL on RDS and 3x the performance of Postgres
- autoscales(per 10GB up to 64TB)
- can have 15 replicas
- Highly available wih instant failover
- 20% more expensive than RDS
- Can self-heal
- 6 copies of the data across 3 AZ(4 copies needed for writes and 3 for reads)
- allows cross region replicas

### Aurora Security
- similar to RDS(same engine)
- encryption at rest using KMS
- backups, snapshots and replicas are encrypted
- encryption in flight using SSL
- setting up security groups is users responsibility

## Amazon ElastiCache
- AWS managed Redis or Memcached
- in-memory databases (high performance, low latency)
- reduce load off of DB(read intensive work)
- helps making application stateless
- cashing implemented in application source code

### ElastiCache solutions
- DB Cache
  - Applications queries ElastiCache, if not available, get from RDS and store in ElastiCache
  - Cached data must be invalidated(otherwise becomes outdated)
- User Session Store
  - User session stored in ElastiCache, therefore can hit any instance 

### Cache Security
- IAM authentication not supported
- IAM policies on ElastiCache are only used for AWS API-level security
- Redis AUTH -> set credentials when creating Redis cluster, which is an extra level of security on top of the security groups. Also supports SSL in flight encryption
- Memcached -> Supports SASL-based authentication

### REDIS VS MEMCACHED
| REDIS    | MEMCACHED |
| :------- | :------- |
|Multi AZ with Auto-Failover | Sharding (multi-node for partitioning of data) |
|Read Replicas and high availability | No high availability |
|Append Only File persistence| Non persistent|
|Backup and restore|No backup and restore|
||Multi-threaded architecture|

### More about cacheing
- Cache eviction can occur in three ways:
  - deleted explicitly
  - evicted because the memory is full
  - set TTL(time-to-live))
- Lazy Loading / Cache-Aside / Lazy Population (cacheing)
  - Cache data when requested
  - Can be slow if data is not yet cashed(3 queries)
  - Can be bad, stale data(outdated comparing to DB)
- Write Through (cacheing)
  - Cache when database is updated
  - Data in cache is never stale / reads are quick
  - Expensive(writing twice)
  - Best to combine with lazy loading(to avoid missing data while DB is updated)
  - Writing everything into cache may be a wasted effort(never gets used)

# [Back to content](#content)

## Route 53
- Managed Domain Name System(DNS)
- DNS is a collection of rules and records which helps clients understand how to reach a server through URLs:
    - A: hostname to IPv4
    - AAAA: hostname to IPv6
    - CNAME: hostname to hostname(only for root domain)
    - Alias: hostname to AWS resource(works for non-root domain)
- Can use private and public domain names
- DNS Cache: TTL set when request received
- Can do health checks and failover
- Also a `Domain Registrar`: manages the
reservation of domain names(such as GoDaddy), but third party also can be used

### Routing Policies:
- Weighted Routing Policy: controls the % of request that go to an endpoint
- Latency Routing Policy: redirect to a server with the least latency(can be further by geo location)
- Geo Location Routing Policy: based on users location(should set default policy in case no match for location)
- Geoproximity Routing Policy: route traffic to resources based on the geographic location of users and resources(can set bias)
- Multi Value Routing Policy: routing traffic to multiple resources(not a substitute for having an ELB)

# [Back to content](#content)
