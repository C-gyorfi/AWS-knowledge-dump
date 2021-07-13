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
- [EC2 Storage](#ec2-storage)
  - [EC2 Instance Storage](#ec2-instance-storage)
  - [Elastic Block Store](#Elastic-Block-Store)
  - [Elastic File System](#Elastic-File-System)
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
- [VPC fundamentals](#VPC-fundamentals)
- [S3](#S3)
  - [Versioning](#Versioning)
  - [Encryption](#Encryption)
  - [Security](#Security)
  - [CORS](#CORS)
  - [MFA Delete](#MFA-Delete)
  - [S3 Access Logs](#S3-Access-Logs)
  - [S3 Replication](#S3-Replication)
  - [S3 Pre-Signed URLs](#S3-Pre-Signed-URLs)
  - [S3 Storage Classes](#S3-Storage-Classes)
  - [S3 Lifecycle Rules](#S3-Lifecycle-Rules)
  - [S3 Performance](#S3-Performance)
  - [S3 Select and Glacier Select](#S3-Select-and-Glacier-Select)
  - [S3 Event Notifications](#S3-Event-Notifications)
  - [Locks](#Locks)
- [AWS Athena](#AWS-Athena)
- [CloudFront](#CloudFront)
  - [CloudFront VS S3 Cross Region Replication](#CloudFront-VS-S3-Cross-Region-Replication)
  - [CF Signed URL VS S3 Pre-Signed URL](#CF-Signed-URL-VS-S3-Pre-Signed-URL)
- [ECS](#ECS)
  - [ECS Clusters](#ECS-Clusters)
  - [Task Definitions](#Task-Definitions)
  - [ECS Service](#ECS-Service)
  - [ECR](#ECR)
  - [Fargate](#Fargate)
  - [IAM Roles](#IAM-Roles)
  - [Tasks Placemen](#Tasks-Placemen)
  - [Service Auto Scaling](#Service-Auto-Scaling)
  - [Cluster Capacity Provider](#Cluster-Capacity-Provider)
  - [ECS Data Volumes](#ECS-Data-Volumes)
- [Elastic Beanstalk](#Elastic-Beanstalk)
  - [Beanstalk Deployment Options](#Beanstalk-Deployment-Options)
- [CICD](#CICD)
  - [CodeCommit](#CodeCommit)
  - [CodePipeline](#CodePipeline)
  - [CodeBuild](#CodeBuild)
  - [CodeDeploy](#CodeDeploy)
  - [CodeStar](#CodeStar)
- [CloudFormation](#CloudFormation)
  - [CF template](#CF-template)
  - [Updating stacks](#Updating-stacks)
- [CloudWatch](#CloudWatch)
  - [Metics](#Metrics)
  - [Logs](#Logs)
  - [Events](#Events)
  - [Alarms](#Alarms)
- [X-Ray](#X-Ray)
- [CloudTrail](#CloudTrail)
- [SQS](#SQS)
- [SNS](#SNS)

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

## EC2 Storage
### EC2 Instance Storage
EC2 Instance Storage are high-performance hardware disk(EBS are network drives with limited performance)
- ephemeral storage(loses data once they got stopped)
- use-cases: buffer / cache / scratch data / temp storage

### Elastic Block Store
- Raw block-level storage that can be attached to EC2 instances
- EBS Volume Types and Use cases
  - `General Purpose SSD`: cheap and low latency, good for System boot volumes,Virtual desktops, Dev environments
  - `Provisioned IOPS (PIOPS) SSD`: sustained IOPS or applications that need more than 16,000 IOPS, e.g databases workloads (sensitive to storage perf and consistency)
  - `Hard Disk Drives (HDD)`: cannot be boot volume, throughput optimized HDD (st1)
  good for big data, data Warehouses or cold HDD (sc1) data that is infrequently accessed. 
- EBS Multi-Attach
  - Allows attaching EBS to multiple EC2 instances(same AZ)
  - Requirement: file system that’s cluster-aware
![image](https://user-images.githubusercontent.com/22743709/125519494-41f9b4b5-f0dd-423e-8696-a818c9282398.png)

### Elastic File System
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

## VPC fundamentals
- Virtual Private Cloud(VPC)
- Subnets: Tied to an AZ, network partition of a VPC
- Internet Gateway: at the VPC level, provide Internet Access
- NAT Gateway(AWS managed) / NAT Instances(self managed): give internet access to private subnets while remaining private
- NACL: Firewall which controls traffic from and to
subnet, stateless, subnet rules for inbound and outbound
- Security Groups: Firewall that controls traffic to and from an instance(e.g EC2), stateful, operate at the EC2 instance level or ENI
- VPC Peering: Connect two VPC with non overlapping IP ranges, non transitive(all VPC needs explicit peering to connect)
- VPC Endpoints: Provide private access to AWS Services within VPC(enhanced security and low latency)
- VPC Flow Logs: network traffic logs
- Site to Site VPN: VPN over public internet between on-premises DC and AWS 
- Direct Connect: (physical)direct private connection to a AWS

# [Back to content](#content)

## S3
- ”infinitely scaling” storage
- stores object in "buckets
- each bucket has to have a globally unique name, but they defined at region level
- Objects are files, the key to an object is the path (e.g s3://some-bucket/`this/is/thepath.txt`)
- max object size is 5TB, however max upload size is 5GB, but can use "multi-part upload"
- can have Metadata(system or user metadata), Tags(useful for security / lifecycle), Version ID (if versioning is enabled)
- can be used for static website hosting(public reads needs to ALLOW)

### Versioning
- enabled at the bucket level
- when key is overwritten will increment the version
- best practice to enable versioning(can roll back, protect against unintended deletes)
- file that is not versioned prior to enabling versioning will have version “null”
- suspending versioning does not delete the previous versions
- deleting versioned item adds delete marker(but keeps the file until they are permanently deleted)

### Encryption
- Methods of encrypting objects:
  - SSE-S3: encrypts S3 objects using keys handled & managed by AWS(server side encryption), using AES-256 encryption, by using the header `"x-amz-server-side-encryption": "AES256"`
  - SSE-KMS: leverage AWS Key Management Service to manage encryption keys, good because user control and audit trail(also server side encryption), set `"x-amz-server-side-encryption": ”aws:kms"`
  - SSE-C: server-side encryption, keys managed and provided by customer, Amazon S3 does not store the key, must use HTTPS because key provided in request header.
  - Client Side Encryption: encryption happens client side using client library(e.g Amazon S3 Encryption Client), encrypted data sent to S3, decrypted client-side when retrieved from S3
- Encryption in transit (SSL/TLS):
  - S3 exposes HTTP endpoint(non encrypted) and HTTPS endpoint (encryption in flight)
  - HTTPS is mandatory for SSE-C but recommended in every case(most client would use the HTTPS endpoint by default)

### Security
- User based: IAM policies 
- Resource Based: Bucket Policies(bucket wide rules) / Object Access Control List / Bucket Access Control List
- IAM principal can access an S3 object if IAM permission or resource policy ALLOWS and there’s no explicit DENY
- Block Public Access if the bucket should never be public
- S3 supports VPC endpoints
- Can log access(stored in another S3) or API calls can be logged via CloudTrail
- Can set MFA to delete
- Pre-signed URLs valid for limited time 

### CORS
- CORS means Cross-Origin Resource Sharing
- allow requests to other origins while visiting the main origin
- if the origins are are different, the requests won’t be fulfilled unless the other origin allows for the requests, using CORS Headers (ex: Access-Control-Allow-Origin)
- if a client does a cross-origin request on an S3 bucket, CORS headers needs to enabled to allow the request(can allow a specific origin or all using wildcard)

### MFA Delete
- to use MFA-Delete, versioning must be enabled on the S3 bucket
- you will need MFA to permanently delete an object version or suspend versioning on the bucket
- you won't need MFA to list deleted versions or enabling versioning
- root account can enable MFA Delete and only trough the CLI

### S3 Access Logs
- For audit purpose
- any request made to S3, from any account will be logged into another S3 bucket
- logging bucket must be a different bucket otherwise it creates and infinite loop

### S3 Replication
- Must enable versioning on both source and destination
- Cross Region Replication (CRR) - potential use-cases: compliance, lower latency access, replication across accounts
- Same Region Replication (SRR) - potential use-cases: log aggregation, live replication between production and test accounts
- Can replicate in different account
- asynchronous process
- when activating new objects are replicated
- delete markers not replicated

### S3 Pre-Signed URLs
- generate using CLI or SDK
- inherit the permissions of the person who generated

### S3 Storage Classes
- Amazon S3 Standard - General Purpose
  - High durability / Multi AZ
  - 99.99% Availability
  - Sustain 2 concurrent facility failures
  - Use-Cases: Data analytics, content distribution
- Amazon S3 Standard-Infrequent Access (IA) 
  - For less frequently accessed data, but rapid access when needed
  - High durability / Multi AZ
  - High Availability
  - Lower cost than standard
  - Use-Cases: disaster recover, backups
- Amazon S3 One Zone-Infrequent Access
  - data is stored in a single AZ
  - High durability, data lost when AZ is destroyed
  - 99.5% Availability
  - Lower cost than IA
  - Use-Cases: Storing backup of on-prem data or storing data that can be recreated
- Amazon S3 Intelligent Tiering
  - low latency and high throughput performance of S3 Standard
  - small monthly monitoring and auto-tiering fee
  - move objects between tiers based on changing usage patters
  - multi AZ 
- Amazon Glacier
  - low cost storage for archiving
  - for long term data storage
  - cost $0.004 / GB per month + retrieval cost
  - items called `Archive` in `Vaults`
  - retrieval options: 
    - Expedited (1 to 5 minutes)
    - Standard (3 to 5 hours)
    - Bulk (5 to 12 hours)
  - Minimum storage duration of 90 days
- Amazon Glacier Deep Archive
  - retrieval options: 
    - Standard (12 hours) 
    - Bulk (48 hours)
  - Minimum storage duration of 180 days

### S3 Lifecycle Rules
- Transition actions: To move objects to another storage class after a given time
- Expiration actions: To delete after some time

### S3 Performance
- Auto-scales to hight request rates (latency 100-200 ms)
- can achieve at least 3,500 request per prefix("`/example/of/prefix/`file")
- can achieve 22,000 requests per second across prefixes
- when using SSE-KMS, KMS limitation may impact this performance
- Multi-Part upload: 
  - must use for files greater than 5GB
  - parallelize uploads(faster)
- S3 Transfer Acceleration:
  - to increase transfer speed: `S3` ->>> `AWS edge location` ->>> `S3 bucket in the target region`
  - can be used along multi-part upload
- S3 Byte-Range Fetches:
  - parallelize GETs request by requesting specific byte ranges
  - resilience in case of failures
  - speed up downloads
  - can retrieve part of a file

### S3 Select and Glacier Select
- Using SQL for server side filtering
- filter by rows & columns
- less network transfer, less CPU cost client-side

### S3 Event Notifications
- S3 operations can trigger SNS, SQS and Lambda events
- Object name filtering possible
- Versioned buckets can be more accurate(otherwise 2 writes in the same time may trigger 1 event)

### Locks
- S3 Object Lock
  - Block an object version deletion for a specified amount of time
  - for Write Once Read Many(WORM) model
- Glacier Vault Lock
  - WORM model
  - Lock the policy for future edits
  - For compliance and data retention

# [Back to content](#content)

## AWS Athena
- Serverless service to perform analytics directly against S3 files
- Uses SQL language to query the files
- Charge per query(amount of data scanned)
- Use-cases: Business intelligence / analytics / reporting / CloudTrail trails
- Allows analyzing data directly on S3 

# [Back to content](#content)

## CloudFront
- Content Delivery Network(CDN)
- Cached content at edge locations to improve read performance
- Can expose external HTTPS or talk to internal HTTPS
- Web application firewall and DDos protection
- Origins:
  - S3 (distributing files, enhanced security Origin Access Identity, CF can be used to upload files to S3(as an ingress))
  - HTTP: ABL / EC2 instance / S3 website / or any HTTP service
- Geo Restriction(restrict access by geo location):
  - Whitelist: allow countries (by 3rd party Geo-IP database)
  - Blacklist: ban countries (by 3rd party Geo-IP database)
- Caching:
  - at each CloudFront Edge Location
  - ideally maximise the cache hit rate / minimise origin requests
  - TTL (0 seconds to 1 year) - set by origin (`Cache-Control` / `Expires` headers)
  - invalidate using `CreateInvalidation API`
  - separating static and dynamic distributions can maximise cache hits 
- HTTPS protocol policies: 
- [Client]<=`viewer protocol policy`=>[EdgeLocation]<=`origin protocol policy`=>[Origin]
  - Viewer Protocol Policy:
    - HTTP redirected to HTTPS / or HTTPS only
  - Origin Protocol Policy
    - HTTPS only 
    - or match HTTP => HTTP & HTTPS => HTTPS
    - s3 served websites doesn't support HTTPS
- Signed URL and Signed Cookies
  - Control the time frame of URL validity
  - Signed URL to allow access individual files
  - Signed Cookies to allow access multiple files 

## CloudFront VS S3 Cross Region Replication
| CloudFront | S3 Cross Region Replication |
| :------- | :------- |
|Global Edge network | Need setup for each region |
|Objects cached for a TTL | Updated in near real-time |
|good for static content| Read only |
||dynamic content / low latency(ideal for content for a few regions)|

## CF Signed URL VS S3 Pre-Signed URL
| CloudFront Signed URL | S3 Pre-Signed URL |
| :------- | :------- |
|Allow access to a path, no matter the origin | Inherit permission of the person who pre-signed the URL |
|Account wide key-pair(only root can manage) | Uses the IAM key of the signing IAM principal |
|Can filter by IP, path, date, expiration | Limited lifetime |
|Can leverage caching features ||

# [Back to content](#content)

## ECS
- Elastic Container Service
- To run containerized applications(e.g docker) 
- Docker Containers Management platforms:
  - ECS
  - Fargate (serverless)
  - EKS (AWS managed Kubernetes)

### ECS Clusters
- logical grouping of EC2s
- EC2 instances running ECS agent (Docker container)
- ECS agent registers instance to cluster

### Task Definitions
- JSON with config how to run a docker container
  - Port Binding for Container and Host
  - Memory and CPU requirement
  - Env vars
  - Networking info
  - IAM Role
  - Logging

### ECS Service
- define how many tasks should run and how
- ensure task are running as desired
- can be linked to load-balancer(ELB/NLB/ALB)
- ALB allow dynamic port mapping

### ECR
- ECR is for hosting private images
- IAM to control access

### Fargate
- Serverless - no need to provision EC2 instances
- just create task definitions - AWS will run the docker containers
- scale -> increase task numbers

### IAM Roles
| EC2 Instance Profile | ECSTask Role |
| :------- | :------- |
|Used by ECS agent | Allow tasks to have their specific role |
|Makes API calls to ECS service | Use different roles for different ECS Services |
|Send container logs to CloudWatch| Defined in the task definition |
|Pull image from ECR||

### Tasks Placemen
- Only applies to ECS with EC2(not for Fargate)
- To decide where to place new tasks based on CPU, memory, and available port
- Or to determine which task to terminate
- Defined in `task placement strategy` and `task placement constraints` 
- Process of placing new tasks:
  - 1. Find instances that satisfy requirements(CPU, memory, port)
  - 2. Identify instances that meet `task placement constraints`
  - 3. Identify instances that meet `task placement strategy`
  - 4. Select the instance
- Task Placement Strategies:
  - **Binpack**: cost saving, minimises the number of instances in use by placing tasks based on least available amount of resources(*CPU/memory*)
  - **Random**: place randomly
  - **Spread**: Distribute tasks evenly (based on specified value, e.g: `availability-zone`)
  - !Can mix strategies(e.g **spread** by availability zone by **binpack** memory)
- Task Placement Constraints:
  - `distinctInstance`: each tasks on different container
  - `memberOf`: place task on container that satisfies the expression:

    ```node
    "placementConstraints": [
      {
        "expression": "attribute.ecs.instance-type == g2.*",
        "type": "memberOf"
      }
    ]
    ```

### Service Auto Scaling
- CPU / RAM monitored by CloudWatch at ECS service level
- Types:
  - **Target Tracking**: based on a specific CloudWatch metric
  - **Step Scaling**: based on CloudWatch alarms
  - **Scheduled Scaling**: based on predictable changes
- Service Auto Scaling(task level) is NOT THE SAME as EC2 Auto Scaling (instance level)

### Cluster Capacity Provider
- To determine the infrastructure the task is running on
- For `Fargate` -> FARGATE and FARGATE_SPOT capacity providers added automatically
- For `EC2` -> need to associate the capacity provider with an auto-scaling group

### ECS Data Volumes
- EBS volume is mounted onto the EC2, but task moved from one EC2 instance to another, won't be able to access the same volume
- EFS File Systems 
  - works for both EC2 Tasks and Fargate tasks
  - Use-case: persistent multi-AZ shared storage for containers
- Bind Mounts Sharing data between containers
  - works for both EC2 Tasks and Fargate tasks
  - to share an ephemeral storage between containers

# [Back to content](#content)

## Elastic Beanstalk
- AWS's PaaS solution using EC2, ASG, ELB, RDS, etc. under the hood
- application code is the responsibility of the dev
- uses Cloud Formation under the hood
- architecture models:
  - Single Instance
  - LB + ASG (good for pre-prod and prod)
  - ASG only(good for non-web apps in prod such as workers)
- Elastic Beanstalk components: Application, Application version, Environment
- Supports many language, but you can write your custom platform or just run Docker
- Has its own CLI: `EB cli` 
- Deployment process: 
  - Describe dependencies
  - Zip code 
  - Create new app version
  - EB will do the rest(deploy the app to EC2s and run the app)
- Lifecycle Policy: max 1000 application versions, but can set to remove unused old versions by time or space
- Extensions: 
  - define in root folder in .ebextensions/ 
  - YAML / JSON format
  - .config extensions (example: logging.config)
  - can add AWS resources such as RDS, ElastiCache, DynamoDB(deleted when environment deleted)
- Elastic Beanstalk Cloning: new env with with the same config
- Migration / Change load balancer type:
  - create new env with same config(not cloning)
  - deploy app
  - perform CNAME swap or Route 53 update
- RDS can be provisioned with Beanstalk which is good for dev/test but not for prod because the lifecycle of the RDS is tied to Beanstalk env
- Migration / Decouple RDS: 
  - create snapshot(for safety)
  - disable deletion on RDS console
  - create new Beanstalk app and point to existing RDS
  - perform CNAME swap or Route 53 update
  - terminate the old env
  - delete the `DELETE_FAILED` CloudFormation stack
- Single Docker: does not use ECS, runs the provided docker container:
  - `Dockerfile`: will build and run
  - `Dockerrun.aws.json` (v1): describe where *already built* image is
- Multi Docker Container:
  - multiple containers per EC2 instance
  - creates ECS Cluster, EC2 instances, Load Balancer (in high availability mode), task definition and execution
  - `Dockerrun.aws.json` required in root to generate the ECS task definition
  - Image must be build and stored(e.g ECR)
- HTTPS: Load the SSL certificate onto LB using console or `.ebextensions/securelistener-alb.config` 
  - SSL Certificate provisioned using AWS Certificate Manager or CLI
  - Must have security group rule to allow incoming port 443
- Worker Environment: allows decoupling application into two tiers, offload tasks from web server(can use a `cron.yaml`)
- Custom Platform: allows defining OS, additional software, scripts that beanstalk runs
  - Custom Image: modified existing Beanstalk Platform
  - Custom Platform: new Beanstalk Platform

### Beanstalk Deployment Options
- `All at once` - fastest but there is downtime
- `Rolling` - update a few instances and move on once the instance is healthy
- `Rolling with additional batches` - besides rolling, spins up additional instances(small extra cost)
- `Immutable` - new instances in a new ASG, deploys version and then swaps all the instances
- `Blue / Green` - not a direct feature! Create a new stage env with the new version, Route 53 to redirect fraction of the traffic to new version. Easy to roll back. Use Beanstalk, “swap URLs” to complete deployment.
- `Traffic Splitting` - good for canary testing. Small fraction of traffic is sent to a temporary ASG. Health is monitored and automatically rolls back in case of failure. Zero downtime. New instances are migrated from the temporary to the original ASG and old app versions terminated.

# [Back to content](#content)

## CICD

### **CodeCommit**: 
  - AWS version control offer(GIT), scale seamlessly, private, integrated with CI tools e.g CodeBuild or Jenkins
  - Authentication: HTTPS / SSH / MFA
  - Authorization: can use IAM Policies to manage user / roles rights to repositories
  - repos are encrypted at rest using KMS
  - both GitHub and CodeCommit can integrate with CodeBuild
  - can trigger AWS SNS (Simple Notification Service) or AWS Lambda or AWS CloudWatch Event Rules


### **CodePipeline**:
  - Continuous delivery
  - Source can be GitHub / CodeCommit / Amazon S3
  - Build: CodeBuild / Jenkins / etc
  - Load testing(3rd party tools)
  - Deploy: AWS CodeDeploy / Beanstalk / CloudFormation / ECS
  - Stages can have both sequential actions and parallel actions
  - Whats a stage? Build / Test / Deploy / Load Test / etc
  - Can set manual approval at any stage(e.g deploy to prod)
  - Artifacts stored in S3(passed to next stage from there)
  - State changes happen in AWS CloudWatch Events, can create SNS notifications(e.g failed pipeline)
  - CloudTrail can be used to audit AWS API calls
  - Pipeline need the right permissions(IAM Service Role) attached to perform certain actions

### **CodeBuild**
  - Managed build service(similar to Jenkins)
  - Continuous scaling(no provision required)
  - Pay per usage(build time)
  - Leverages Docker
  - Integrates with KMS for encryption of build artifacts
  - Build instructions in `buildspec.yml`
  - Logs to Amazon S3 & AWS CloudWatch Logs
  - CloudWatch Events(e.g detect failed builds and trigger notifications)
  - CloudWatch Alarms(to notify if “thresholds” for failures needed)
  - SNS notifications
  - Can reproduce CodeBuild locally to troubleshoot
  - Builds can be defined in `CodePipeline` or `CodeBuild`
  - SSM Parameter store can be used to store secrets
  - Phases: `Install` // `Pre build` // `Build` // `Post build`
  - Artifacts: uploaded into S3 (KMS encrypted)
  - Cache: Files to cache into S3 (future build speedup)
  - By default CodeBuild launches outside of the deployed VPC, can set VPC config so it can access resources(e.g integration tests etc.)

### **CodeDeploy**
  - Managed deployment service(Elastic Beanstalk also uses under the hood)
  - How?
    - EC2 or on-prem must run CodeDeploy Agent
    - Agent is calling CodeDeploy to get next work
    - CodeDeploy sends `appspec.yml`
    - App pulled from GitHub or S3
    - Instance runs deployment instructions
    - CodeDeploy Agent reports: success or failure
  - Instances grouped by deployment env(dev / test / prod)
  - Can be chained into CodePipeline to use artifacts from there
  - Blue / Green only works with EC2(not on prem)
  - Support deployment of AWS Lambda
  - Does not provision resources
  - AppSpec:
    - How to source and copy from S3 / GitHub to filesystem
    - Hooks: instructions to deploy new version
      - ApplicationStop
      - DownloadBundle
      - BeforeInstall
      - AfterInstall
      - ApplicationStart
      - ValidateService

### **CodeStar**
  - Integrated solution(GitHub/CodeCommit, CodeBuild, CodeDeploy, CloudFormation, CodePipeline, CloudWatch)
  - Issue tracking integration with: JIRA / GitHub Issues
  - Dashboard to view all components
  - Pay only for the underlying services

# [Back to content](#content)

## CloudFormation
- Infrastructure as Code
- Cost effectiveness: 
  - Can estimate cost of the resources using CloudFormation template
  - Can destroy resources when not in use
- Templates has to be uploaded into S3 and referenced in CF
- To update, need to upload new version(cannot edit existing version in bucket)
- Stacks identified by name
- Deleting stack deletes all referenced artifact 
- How to edit? CloudFormation Designer on console or edit the YAML and deploy using AWS CLI

### CF template
- Components:
  - Resources (mandatory)
    - Can reference each other
    - Every resource has to declared cannot use dynamic generation
  - Parameters (dynamic inputs)
    - Reference a Parameter: use `Fn::Ref` or short in YML `!Ref`
  - Mappings (static variables)
    - Fixed variables within CF Template
    - Values are hardcoded within the template
    - User `Fn::FindInMap` to return value for given key, e.g `!FindInMap [ MapName, TopLevelKey, SecondLevelKey ]`
  - Outputs (references to created resources)
    - optional values, can be imported into other stacks
    - can’t delete a CF Stack if its outputs are referenced by another stack(using `!ImportValue`)
  - Conditionals (conditions to perform resource creation)
    - Logical conditions such as `Fn::Equals`, `Fn::Not` , `Fn::If` etc..
  - Metadata
- Helpers: References and Functions

### Updating stacks
- When stack creating fails everything rolls back by default(gets deleted)
- There is option to disable rollback
- When update fails, the stack rolls back to previous working state(see logs for errors)
- `ChangeSets` tells you the changes before applying, but it won't tell if the update will be successful 
- `Nested stacks` allows reusing repeated patterns or common components from other stacks(best practice)
- `Cross Stacks` reference export values in other stacks
- `StackSets` manage changes across multiple accounts/regions with a single operation
- `CloudFormation drift` protect against manual changes after an infrastructure has been deployed

# [Back to content](#content)

## CloudWatch
### **Metrics** 
- Analyse key metrics
- Metric is variable of a resource to monitor(e.g `CPUUtilization`). Belongs to `Namespaces` and have timestamps
- Dimension is an attribute of a metric(e.g `instance id`)
- EC2 instance tracks metrics for every 5 minutes by default(up to 1 minutes with detailed monitoring => extra cost)
- Free Tier allows 10 detailed monitoring metrics
- EC2 Memory usage not pushed by default(enable using custom metric)
- Custom Metrics:
  - To segment metrics, use dimensions(e.g `Instance.id`)
  - Metric resolution: `Standard` => 60 sec, `High Resolution` => 1 sec for extra cost
  - Use the `PutMetricData` to send custom metrics
  - throttle errors -> exponential back off 

### **Logs** 
- Collect log files
- Send logs using SDK
- Can batch export to archive into S3
- Can stream to ElasticSearch for analytics
- `Log groups` - represents an application
- `Log stream` - instances within application or containers
- Expiration policies to define how long can a log live
- IAM permissions needs to allow to send logs to CW
- Can encrypt using KMS
- **EC2 logs**:
  - Must run cloudWatch agent(can set on premises too!)
  - Must have the IAM permissions
  - Agents:
    - `CloudWatch Logs Agent`(old version to send logs to CW)
    - `CloudWatch Unified Agent` (can do other things e.g system-level metrics -> RAM/CPU), config using SSM Parameter Store
- When filter applied only the filtered data sent to CW

### **Events** 
- Notifications when events happen
- `Schedule` using CRON
- `Event Pattern` react to a service change(e.g CodePipeline)
- Can trigger Lambda, SQS/SNS/Kinesis Messages
- Creates a JSON with the info about the change 
- **EventBridge** -> new CloudWatch Events(built on top of CW Events)
  - Default event bus(AWS generated)
  - Partner event bus(to receive event from 3rd party e.g `DataDog`)
  - Custom Event buses(for own app)
  - Event buses can be accesses by other AWS accounts
  - `Schema Registry`: generates code based on the data structured in the event bus

### **Alarms** 
- React to metrics or events
- Triggers notifications for a metric(e.g ASG, EC2 Actions, SNS)
- States can be:
  - ||`OK` ||
  - || `INSUFFICIENT_DATA` || 
  - ||`ALARM`||
- Period:
  - Time window to evaluate the metric(in seconds)
  - High resolution: 10 sec or 30 sec

# [Back to content](#content)

## X-Ray
- Visual analysis of cloud applications
- Troubleshooting performance
- Visualize components in a microservice architecture
- Can be used with AWS Lambda, Elastic Beanstalk, ECS, ELB, API Gateway, EC2(or even on prem)
- Allows tracing requests(every or % of requests)
- IAM for authorization
- Encryption at rest -> KMS
- Enabled using SDK and X-ray daemon/enable X-Ray AWS Integration(AWS Lambda running it already)
- Each app must have the IAM rights to write to X-Ray
- Service map computed(using segments and traces)
- **Sampling Rules** - controls the amount of data sent to X-ray(default records first request each second, and five percent of any additional requests)
- APIs: 
  - Write: `PutTraceSegments`, `PutTelemetryRecords`, `GetSamplingRules`, `GetSamplingTargets`, `GetSamplingStatisticSummaries`
  - Read: `GetServiceGraph`, `BatchGetTraces`, `GetTraceSummaries`, `GetTraceGraph`
  - X-Ray daemon needs to have an IAM for the above API calls
- Config on Elastic Beanstalk using `.ebextensions/xray-daemon.config` 
  - instance profile needs to have the correct IAM permissions
  - application code with the X-Ray SDK
  - **X-Ray daemon is not provided for Multicontainer Docker**
- In ECS cluster can enabled using X-Ray Daemon Container or using "Sidecar" on the app container
- Fargate cluster "Sidecar" on the app container in the Fargate task
 
# [Back to content](#content)

## CloudTrail
- Governance, compliance and audit for AWS Account
- Default enabled 
- events and API calls made within an AWS Account(using Console/SDK/CLI)
- Can put logs into CW or S3
- CloudTrail can help investigating deleted resources
- CloudTrail Events:
  - **Management Events:**
    - performed on resources e.g `AttachRolePolicy`, `CreateSubnet`
    - logged by default
    - can separate Read/Write events
  - **Data Events:**
    - Not logged by default
    - Object level. e.g `GetObject`(Read event) `DeleteObject` (Write event)
    - Lambda function execution is a data event
  - **Insights Events:**
    - Enable to detect unusual activity
    - Baseline created by analyzing normal management events 
    - Sent to S3 and EventBridge
- Events stored for 90 days(for longer -> log into S3 and use Athena)

# [Back to content](#content)

## SQS
- Managed service, queue model
- Unlimited throughput / unlimited messages in queue
- Default retention 4 days(maxi of 14 days)
- Low latency (10ms)
- 256KB message size limit
- Best effort ordering messages(can be out of order)
- Can have duplicate
- Sending:
  - Use the `SendMessage` API (SDK) to produce a message
  - persisted until a consumer deletes / retention period
- Consuming:
  - Poll SQS for messages(up to 10 messages per request)
  - Process and Delete the message using `DeleteMessage` API
- Consumers receive messages in parallel(can scale consumers horizontally for more throughput)
- Use CW alarm to scale ASG
- Encryption:
  - at rest using KMS 
  - inflight using HTTPS API
  - client-side encryption ->  encryption/decryption by client
- Use IAM to manage access to SQS 
- SQS Access Policies:
  - for cross-account access
  - allowing other services to write to a queue
- Message Visibility Timeout:
  - When message polled by a consumer it becomes invisible
  - Default 30 sec
  - Avoid double processing a message by `ChangeMessageVisibility` API(increase timeout)
  - If timeout too high, retry will take too long in case of failure
  - If timeout too short, may get double processed
- Dead Letter Queue
  - Failed message goes back to queue, but we can limit how many times(`MaximumReceives`)
  - After `MaximumReceives` it can go to a DLQ
  - Why? Debugging(be aware of retention period)
- Delay Queue
  - Delay message(up to 15 minutes) before consumer can see it
  - Can set at send using `DelaySeconds` param(default 0 sec)
- Long Polling
  - If no message in Q wait for message to arrive
  - decreases the number of API calls
  - Enable using `WaitTimeSeconds`(API or queue level), preferable to short polling
  - Large messages(> 256KB) send message to S3 and metadata to queue(use SQS Extended Client)
  - Some API calls:
    - `PurgeQueue` -  delete all the messages in Q
    - `CreateQueue`, `CreateQueue`, `SendMessage`, `ReceiveMessage`, `DeleteMessage`, `MaxNumberOfMessages`, `ReceiveMessageWaitTimeSeconds`, `ChangeMessageVisibility` 
    - use Batch API calls where possible to decrease cost!
  - FIFO Queue
    - Messages can be processed in order
    - Throughput limit = 300 msg/s without batching or 3000 msg/s
    - Removes duplicates(Deduplication in 5 minutes)
      - SHA-256 hash of the message body to perform content based de-dup
      - or by provided `Message Deduplication ID`
    - Message Grouping
      - Use `MessageGroupID` 
      - If using the same value, only one consumer will receive messages
      - Each `MessageGroupID` can have different consumers
      - Messages ordered withing groups, but not guaranteed across groups
 
# [Back to content](#content)

## SNS
- **Simple Notification Service**, sends message to a single SNS topic
- Many "receivers" / subscriptions can listen to an SNS topic(all of them gets all the messages)
- Many subscriptions per topic(up to 10,000,000 and 100,000 topics limit)
- Subscribers can be `SQS`, `HTTP / HTTPS`, `Lambda`, `Email`, `Text`, `mobile notification`
- Many AWS services can send data to SNS(CloudWatch, ASG, S3, CF, etc.)
- Publish:
  - `Topic publish`(SDK) = Create topic -> Create a subscription -> Publish the topic
  - `Direct publish`(Mobile app SDK) = Create platform application -> Create platform endpoint -> Publish to endpoint(Google GCM, Apple APNS, Amazon ADM)
- Encryption just like SQS:
  - at rest using KMS 
  - inflight using HTTPS API
  - client-side encryption ->  encryption/decryption by client
- Use IAM to manage access to SNS API 
- SNS Access Policies(Similar to S3 or SQS):
  - for cross-account access
  - allowing other services to write to an SNS topic
- SNS + SQS = `Fan Out`: 
  - One push to SNS receives all SQS subscribers
  - Decoupled and no data loss
  - Can scale by adding more SQS subscribers over time
  - SQS access policy must allow SNS to write
  - can send S3 event to multiple queues with `fan out`
- FIFO Topic -> ordering messages in a topic:
  - Ordering and Deduplication and throughput limitation similar to SQS
  -  **! Only SQS FIFO queues can be subscribers !**
- Filter messages using JSON policy(subscription without filter policy receives all messages from topic)

# [Back to content](#content)
