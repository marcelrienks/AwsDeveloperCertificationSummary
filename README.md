# Identity and Access Management (IAM):
Essentially allows you to manage users and their level of access to the AWS console.
* Centralized control of your AWS account
* Shared access to your AWS account
* Granular Permissions
* Identity Federation (including Active Directory, facebook, LinkedIn etc.)
* Multifactor Authentication
* Provides temporary access for users/devices and services, as necessary
* Allows you to set up your own password rotation policy
* Integrates with many different AWS Services
* Supports PCI DSS Compliance
## Key Terms:
* **Users**  
End User (Think People)  
Users can have two types of access, ***'Programmatic'*** which allows for mamaging AWS recources from the **CLI**  
and/or ***'Management Console'*** which allows a user to log in the AWS management console through a browser to administer resources.
* **Groups**  
A collection of users under one set of permissions.
* **Roles**  
IAM roles are a secure way to grant permissions to entities that you trust. IAM roles issue keys that are valid for short durations, making them a more secure way to grant access.
Examples of entities include the following:
    * IAM user in another account
    * Application code running on an EC2 instance that needs to perform actions on AWS resources
    * An AWS service that needs to act on resources in your account to provide its features
    * Users from a corporate directory who use identity federation with SAML
* **Policies**  
A document that defines either one or more permissions. Can be attached to (any or all) User, Group or Role, granting the permissions defined in that policy.
## Best Practice:
* **Create Groups**  
Create individual user accounts, and group them into logical groups, and assign permissions on a group by group basis using *Policies*
* **Least Privilege**  
Always give users the minimum viable access required
## Note:
* IAM is universal, and does not apply to regions only
* Root account is the one that was used to register, and has complete admin rights
* New IAM users have **NO** permissions by default, and have assigned **Access Key ID** & **Secret Access Keys**
* Secret Access keys can only be viewed once, when they are generated. But can be downloaded in a csv file
* Access keys can be used to access API's and command line, but cannot use access keys and secret keys to log into the console
* A role can be attached to a running EC2 instance
* Roles allow you to gain access to AWS services without using Access Key ID's and Secret Access Keys
* Roles are controlled by *Policies*, and editing a policy has immediate affect on the roles associated with it
## Web Identity Federation:
Let's you give your users access to AWS resources after they successfully authenticate with a web-based identity provider (Amazon, Facebook, Google). Once authenticated, the user receives an 'authentication code' from the web ID provider, which they can trade for temporary AWS security credentials
## Assume Role With Web Identity:
Is an API provided by the Security Token Service (STS). This function returns security credentials for users authenticateing by mobile or web applications, or using a Web ID Provider (Amazon, Facebook, Google).  
***Note: This function is intended for regular web applications, for mobile applications Cognito is recommended***
## IAM Policies
### Managed Policies
Is a policy which is created and managed by AWS, for common use cases,  
e.g. AmazonEC2ReadOnlyAccess, AWSCodeCommitPowerUser etc.  
A single Managed Policy can be attached to multiple users, groups, and roles, and allows you to assign appropriate permissions, without having to create your own.  
***Note: You cannot change the permissions defined in an AWS Managed Policy***
### Customer Managed Policies
Is a standalone policy that you create and administer inside your own AWS account. You can attach this policy to multiple users, groups, and roles, but only within your account. You can copy an existing Managed Policy in order to create a Customer Managed policy, and customize it.  
Recommended for uses cases where AWS Managed Policies do not meet the exact needs of your environment.
### Inline Policies
Is a policy which is embedded within the user, group, or role. There is a strict 1:1 relationship between the entity and the policy. When you delete the user, group, or role, the inline policy will also be deleted.  
Inline policies are useful when you want to ensure that the permissions in the policy are not inadvertently assigned to any other users, groups, or toles.
In most cases AWS recommends using managed policies over inline policies.
## Interesting:
* From the IAM dashboard there is an 'IAM users sign-in link' that relates to your customer account, this can be customized with an alias. When using this link, the login is pre-populated with the account alias, allowing for easy sign in with IAM roles other than root
# Amazon Cognito:
An Identity broker which handles interaction between your application, and the Web ID provider. It brokers between the app and the web-based identity provider (Amazon, Facebook, Google), to provide temporary credentials which map to an IAM role allowing access to the required resources
* Sign-up, Sign-in, and guest user access
* Sync user data for a seamless experience across devices
* Cognito is the AWS recommended approach for Web ID federation particularly for mobile apps
Acts an Identity broker
## User Pools:
Cognito uses User Pools to manage user sign-up and sign-in directly or via Web ID provider (Amazon, Facebook, Google).
## Identity Pools:
Enables you to create unique identities for your users and authenticate them with identity providers
## Push Synchronization
Cognito tracks the association between user identity, and the various different devices they sigh-in from. In order to provide a seamless user experience for your application, Cognito uses Push Synchronization (using Amazon SNS) to push updates and synchronize user data across multiple devices
# Systems Manager Parameter Store:
Provides a secure, hierarchical storage for configuration data management and secrets management. You can store data such as passwords, database strings, and license codes as parameters values.
# Elastic Compute (EC2):
Can be thought of as a ***'Virtual Server'***, it is a web service that provides resizable compute capacity in the cloud, that allows you to quickly scale up and down as requirements change and also allows you pay only for the capacity you actually use.

*Individual instances are provisioned in Availability zones, and as RDS and many other services are built on top of EC2 instance, this applies to these services as well*.
## Pricing:
* **On Demand** allows you to pay fixed rate by the hour or second, and is good for short term, or spiky or unpredictable workloads
* **Reserved Instance** provides a capacity reservation offering significant discount on a 1 or 3 year term, and is good for steady state or predictable usage workloads.
    * Standard RI can save up to 75% vs on-demand
    * Convertible RI allows a reserved instance to be converted to change the attributes of the server (CPU intensive vs Memory intensive) *if the exchange results in a reserved instance of equal or greater value.*
    * Scheduled RI will launch within a window, allowing you to match your capacity to predictable recurring workloads
* **Spot** enables you to bid for an instance capacity for applications that have flexible start and end times (e.g. companies who need to run heavy computing workload, but which can be started early hours of a sunday morning).  
The spot price changes frequently, and if you bid R100 and the instance is provisioned, but the price then climbs to R120, the instance will be terminated.  
*Note: you will not be changed for the partial hour that AWS terminated the instance within, however if you terminate the instance you will be charged for the full hour*.
* **Dedicated Hosts** Physical EC2 server dedicated for your use. Can reduce costs by allowing you to use your existing server-bound licenses. Also useful for regulatory requirements that may not support multi-tenant virtualization.
## Types:
A stupid mnemonic for Edward Nortan from the movie Fight club, fighting Scrooge McDuck who looks like a doctor, and likes to hand out pictures (**FIGHT** **DR** **MC** **PX**)
* **F** - Field Programmable Gate Array  
Genomic research, financial analytics, video processing
* **I** - High speed storage  
NoSQL DBs, Data Warehousing
* **G** - Graphics Intensive  
Video encoding, 3D application steaming
* **H** - High Disk throughput  
Distributed file system
* **T** - Lowest cost General Purpose  
WebServers / Small DBs
* **D** - Dense Storage  
FileServers / Data Warehousing
* **R** - Memory Optimized (mnemonic for this is to think of **R**AM)  
Memory intensive apps
* **M** - General Purpose  
Application Servers
* **C** - Compute Optimized  
CPU intensive Apps / DBs
* **P** - Graphics General Purpose  
Machine learning, Bitcoin mining
* **X** - Extreme Memory Optimized
## Elastic Block Store (EBS):
Can be thought of as a ***'Virtual Disk'***, which allows you to create storage volumes and attach them to EC2 instances. Once attached you can create file systems, run a database, or use them in any other way you would use a block device. EBS volumes are placed in a specific 'Availability Zone', and are automatically replicated to protect against failure.

You can Encrypt ABS volumes using Operating System level encryption.  
You can encrypt root device volume by first taking a snapshot of the volume, creating a copy of that snap show with encryption, and then making an AMI of this snapshot and deploy an encrypted root device volume.  
You can encrypt additional attached volumes using the console, CLI or API
### Types:
#### SSD:
* **GP2** - General Purpose  
balances both price and performance, *best for anywhere between 3 - 10 000 IOPS*
* **IO1** - Provisioned IOPS  
intensive applications such as relational or NoSQL DBs, *above 10 000 IOPS*
#### Magnetic:
* **ST1** - Throughput Optimized  
best for Big Data and Data Warehousing, and *cannot* be a boot volume
* **SC1** - Cold  
lowest cost storage for infrequent access, and *cannot* be a boot volume
* **Magnetic** (*legacy*)  
lowest cost per gigabyte that *can* be booted from
## Security Group:
Think of a security group as a ***'Virtual Firewall'*** that contains a set of firewall rules to control traffic for your EC2 instance
## Elastic Load balancer (ELB):
### Types:
* **Application Load balancer**  
Intelligent and can create advanced request routing all the way up to the application layer, work at layer 7, best suited for HTTP and HTTPS traffic
    * **Target Group**
    An Application Load Balancer is associated with a Target Group which contains references to multiple EC2 instances
* **Network load balancer**  
High performance, work at layer 4, best suited for TCP traffic
* **Classic Load balancer** (*legacy*)  
Similar to the Application load balancer handling layer 7 Http/Https, but without the intelligent routing abilities (***using X_Forwarded and sticky sessions to indicate the origin of the request after the load balancer has forwarded on the request***).  
Or it can also handle layer 4 TCP/TLS/UDP like the Network load balancer.  
**Note:** With the Classic load balancer, if the application stops responding, the load balancer will throw an HTTP 504 - Gateway Timeout error.
## Exam Tips:
* **Know pricing models**
* Know EC2 instance types
* Know EBS volumes
* Know ELB types, common error, and X-Forwarded-For header
# Route 53:
You can use Amazon Route 53 to register new domains, transfer existing domains, route traffic for your domains to your AWS and external resources, and monitor the health of your resources.

Once a domain has been registered or transferred into Route 53, a hosted zone is automatically created for it, with **NS** and **SOA** record sets

An alias **A** record set can then be created in this hosted zone, that points to AWS resources, like EC2s, or ELBs
## Note:
* Route 53 allows you to map your domain name to
    * EC2 instances
    * Load Balancers
    * S3 buckets
# Command Line Interface (CLI):
A unified tool to manage your AWS services. With just one tool to download and configure, you can control multiple AWS services from the command line and automate them through scripts.

The CLI requires credentials in order to communicate with AWS, which can be configured suing the below command, which will guide you through configuring the credentials for the CLI on this EC2 instance, which will request a users *'AWS Access Key ID'* and *'AWS Secret Access Key'*. The configuration for this will be stored in the ``~/.aws`` directory in unix/linux and ``%UserProfile%\.aws`` in windows.
```
aws configure
```
***TODO:*** Check out some basic CLI commands, especially around S3 for the exam
# AWS Database:
## Types:
### **RDS** - *OLTP (Online Transaction Processing)*
* Aurora *(AWS developed db)*
* MySql Server
* MariaDB
* PostgreSQL
* Oracle
* SQL Server
### **DynamoDB** - No Sql document store
### **RedShift** - *OLAP (Online Analytics Processing)*
### **Elasticache** - In memory caching
* Memcached
* Redis
## Note:
* Aurora DB does not qualify for Free tier yet
* RDS databases use DNS names, and you are not given a public IP for an RDS database
# Amazon Relational Database Service (RDS):
## Backups:
* Backups are taken within a defined window and storage I/O may be suspended during this time causing elevated latency.
* Restoring either of the types below results in a new RDS instance with a new DNS endpoint.
### Types:
* **Automated Backups**  
Enabled by default, and stored in S3 in a free storage space equal to the size of the DB. Allows you to recover to any point in the time within a 'retention period'. The retention period can be within 1- 35 days. This option will take a full daily backup snapshot as well as store transaction logs. When recovery is done, AWS takes the latest snapshot, and then applies transaction logs relevant to that day. Backups do not persist after the RDS instance is deleted.
* **Database Snapshots**  
Snapshots are done manually (user initiated), and they persist after the RDS instance is deleted.
## Multi-AZ (Multi Availability Zones):
Primarily for **disaster recovery only** where AWS ***synchronously*** replicate any writes to an RDS database to an replica RDS database in another availability zone. In the event of planned DB maintenance, or DB failure, AWS will automatically fail over to the replica by updating the DNS address to point to the new DB.  
(This is why DNS names are used when referencing RDS databases)
### Note:
* Aurora by default is split across Multi-AZ, where as **all** other DBs can have Multi-AZ enabled when configuring
## Read Replicas:
Allows you to have a read-only copy of your production database, where AWS ***asynchronously*** replicate from the primary instance to the rad replicas, primarily for **Performance improvements** as well as **Scaling**.
* Available on all DBs except **Oracle**, and **SqlServer**
* You can have 5 read replicas
* Must have automatic backups enabled in order to deploy a read replica
* You can have read replicas of read replicas, but this has a latency trade off
* Each read replica will have it's own DNS end point
* You can have read replicas that have Multi-AZ
* You can create read replicas of Multi-AZ source databases
* Read replicas can be promoted to be their own database (*This will break replication*)
* You can have a read replica in a different region
## Note:
* If you have an EC2 instance using one security group, and an RDS instance using another, the security group for the DB needs to be updated to accept connections on a specific port (related to the DB being used). And the source for that connection should be set tot he security group associated with the EC2 instance.
* Because RDS databases use EBS volumes, all AWS databases support Encryption at rest, and is done using **KMS** (AWS Key Management Service). Once the RDS instance is encrypted, the data stored at rest in the underlying storage as are the backups and snapshots.
At the present time encryption of an existing RDS instance is not supported. To use RDS Encryption in this case you must create a snapshot, ***Copy the snapshot*** and in the copy wizard you can select the Encrypt option. Restoring this snapshot will result in an encrypted RDS instance.
# Elasticache - In memory caching:
Elasticache is a web service that makes it easy to deploy, operate, and scale an in memory cache in the cloud. It can be used to significantly improve latency and throughput for many read-heavy applications workloads or compute-intensive workloads.  
Caching improves application performance by storing critical pieces of data in memory for low-latency access.
## Types:
* Memcached  
Elasticache-Memcached does **not** have redundancy features
Memcached is designed as a pure caching solution with no persistence, Elasticache manages nodes in a pool that can grow and shrink, similar to EC2 Auto Scalling Group.  
    * Object caching is your primary goal (e.g offload a database)
    * Simplicity
    * Multi-threaded performance which utilizes multiple cores
    * Scale horizontally
* Redis  
Elasticache-Redis **does** supports Master/Slave replication and Multi-AZ for redundancy.
Elasticache manages Redis more like a relational database, clusters are managed as statefull entities that include failover, similar to how AWS manages RDS failover.  
    * Advanced data types, such as lists, hashes, and sets
    * Can do sorting and ranking of datasets
    * Persistence of key store
    * Multi-AZ failover
## Exam Tips:
Typically you will be given a scenario where a db is under stress. You may be asked which service you should use to alleviate this.  
* Elasticache is a good choice if your Database is particularly read-heavy and not prone to frequent changes
* Using a Redshift data warehouse is a good choice if your database is currently being used to run OLAP transactions
# Simple Storage Service (S3):
Secure, durable, highly-scalable object based storage, the data is spread across multiple devices and facilities.
* Object based storage
* Buckets emulate folders
* Files can be **0 - 5TB**  
However the largest object that can be uploaded in a single **PUT** is **5GB**
* Unlimited storage
* Lifecycle management
* Encryption
* S3 has a universal namespace, meaning it must be unique globally
* Uploading from API or CLI a file will result in an HTTP 200 OK response
## Objects:
Note: As objects are generally just files, and each file will have a domain name allowing download of that resource, it's possible to use S3 to host 'static' website. As such the S3 console supplies a static website properties, when configuring a bucket.
* Key (name of object)
* Value (bytes of the file)
* Version ID
* Metadata (data describing the object, e.g. 'Content-Type: jpg')
## Data Consistency Model:
* Read after Write for PUTS of new Objects
* Eventual consistency for overwrite PUTS and DELETES (can take some time)
## Classes:
*These classes apply to the object being uploaded, **not** to the entire bucket.*
* **S3 Standard**  
Offers high durability, availability, and performance object storage for frequently accessed data. It delivers low latency and high throughput, and you can also use S3 Lifecycle policies to automatically transition objects between storage classes without any application changes.
    * 11x9% durable
    * 99.99% availability
    * N/A Retrieval fee
    * Milliseconds first byte latency
* **S3 Intelligent-Tiering**  
The S3 Intelligent-Tier storage class is designed to optimize costs by automatically moving data to the most cost-effective access tier, without performance impact or operational overhead. It works by storing objects in two access tiers: one tier that is optimized for frequent access and another lower-cost tier that is optimized for infrequent access. For a monthly monitoring and automation fee per object, AWS monitors access patterns of the objects and moves the ones that have not been accessed for 30 consecutive days to the infrequent access tier. If an object in the infrequent access tier is accessed it is automatically moved back to the frequent access tier.
    * 11x9% durable
    * 99.9% availability
    * N/A Retrieval fee
    * Milliseconds first byte latency
* **S3 Standard-IA** - Infrequently accessed  
S3 Standard-IA is for data that is accessed less frequently, but requires rapid access when needed. S3 Standard-IA offers the high durability, high throughput, and low latency of S3 Standard, with a low per GB storage price and per GB retrieval fee.
    * 11x9% durable
    * 99.9% availability
    * Per GB retrieval Charge
    * Milliseconds first byte latency
    Data that is accessed less frequently but requires rapid access when needed. Lower S3 fee, but you are charged a retrieval fee when reading
* **S3 One Zone-IA** - Infrequently accessed  
S3 One Zone-IA is for data that is accessed less frequently, but requires rapid access when needed. Unlike other S3 Storage Classes which store data in a minimum of three Availability Zones (AZs), S3 One Zone-IA stores data in a single AZ and costs 20% less than S3 Standard-IA
    * 11x9% durable
    * 99.5% availability
    * Per GB retrieval Charge
    * Milliseconds first byte latency
    Same as standard IA, but stored in a single availability Zone only. (Cost is 20% less then regular S3-IA)
* **Glacier**  
S3 Glacier is a secure, durable, and low-cost storage class for data archiving. S3 Glacier provides three retrieval options that range from a few minutes to hours
    * 11x9% durable
    * 99.99% availability
    * Per GB retrieval Charge
    * Minutes/Hours first byte latency
* **Glacier Deep Archive**  
S3 Glacier Deep Archive is Amazon S3’s lowest-cost storage class and supports long-term retention and digital preservation for data that may be accessed once or twice in a year
    * 11x9% durable
    * 99.99% availability
    * Per GB retrieval Charge
    * Hours first byte latency
* **Reduced Redundancy** (*Not recommended legacy*)  
Frequent accessed, non-critical data
    * N/A Retrieval fee
## Security:
By Default all newly created buckets are Private, and you can set up access control to the buckets by using the options below. Buckets can be configured to log all requests made, these logs can be written to another bucket.  
* Bucket Policies (Applied at bucket level)
* Access Control Lists (Applied at object level)
### Note:
* If you try to set an access control permission, that violates a Bucket Policy, you will receive an error.
* If you have a bucket, with no public access, you upload a file and do not set any access control permission, it will only be accessible by the owner. If you then later set the bucket to be publicly accessible, the original file is still not publicly accessible, as it's access control still stipulates the owner.
* Bucket policies can be configured through json, the AWS console provides a 'Policy Generator' wizard that will guide you through configuring a policy, and then output the json for that policy.
* You can grant users access to private data wihin an S3 bucket by creating a pre-signed URL that can be used to temporarily access content
## Encryption:
*This is enforced and configured at the Bucket level*
* **Encryption in transit** - SSL/TLS
* **At rest** - Server Side Encryption  
With this option, headers can be set when making a PUT request to the API, these params will dictate which option should be used for the file being uploaded. You can enforce the use of these options by setting a Bucket Policy.  
*S3 uses Advanced Encryption Standard (AES) 256*
    * **SSE-S3** - S3 Managed Keys
    AWS manage and Encrypt the keys, and regularly rotate the keys
    * **SSE-KMS** - Key Management Service, Managed Keys
    AWS manage as well as more powerful features including logging of key usage
    * **SSE-C** - Server Side Encryption with Customer Provided Keys
    AWS manage the Encryption, but you manage your own keys
* **Client Side Encryption**
Where you encrypt the data before uploading
## Transfer Acceleration:
AWS S3 Transfer Acceleration allows you to accelerate the transfer of files into S3 using CloudFront, by uploading content to an Edge Location, before it's sent to the origin, over an optimized network path
## S3 Static website:
You can host a static website on Amazon S3. On a static website, individual webpages include static content. They might also contain client-side scripts.

Website endpoints follow these two conventions, which one of these is dictated by the region used.
* bucket-name.s3-website-region.amazonaws.com
* bucket-name.s3-website.region.amazonaws.com

Alternatively you can use a Route 53 domain name, and register a bucket with the ***exact*** same name.
## Cross Origin Resource Sharing (CORS):
With regards to S3 when using hosting a website, CORS refers to allowing one S3 Bucket website, to access resources in another S3 bucket website.

This can be done by using the domain name for the website that is requesting access to a shared resource in another website. Then under the permissions of the shared resource website, configure CORS by setting the allowed origin to that of the requesting website domain.
## Performance Optimisation:
S3 is designed to support very high request rates. If the bucket receives more than 3500 PUT / LIST / DELETE, or more than 300 GET requests per second, there are some best practice guidelines to help optimisation.
### **Get** Intensive workloads  
Use CloudFront CDN service to improve performance by caching content at edge locations
### **Mixed** workloads (GET, PUT, DELETE etc.)  
The key names used for your content objects can impact performance. S3 uses the key name to determine which partition an object will be stored in. However in 2018 Amazon announced a massive increase in S3 performance, which means logical and sequential naming patterns can now be used without any performance implications
# CloudFront:
*A Content Delivery Network (CDN) is a system of distributed servers that deliver webpages and other web content to users based on geographic locations, the origin of the content, and a content delivery server.*

CloudFront is a fast, highly secure and programmable CDN, that can be used to deliver your entire website, including dynamic, static, streaming, and interactive content using a global network of Edge locations,

CloudFront works by using 'Edge Locations' which are a collection of servers which are in a  geographically dispersed data centers around the world. These data centers are used by CloudFront to keep a cache of copies of your content. Which allows users to access this content from the nearest edge location, rather than the origin location, which is much farther away.
Once the first request is made, the edge location forwards on the request to the origin, downloads the content, and stores a cached version on that edge location. Allowing any future requests to be downloaded directly from the edge location.

## Note:
* Edge content is cached by downloading it from the origin, but you can also write content to an edge location. (i.e. Edge locations are not just READ, they can be WRITE as well)
* The edge location will cache the content for a period of time, referred to as a 'Time To Live'. After that time is up, the content is automatically cleared from the cache.
* The cache can be manually cleared by 'Invalidating' a specific pieve of content, if you have a new version of some content, however you will be charged for doing that.
* CloudFront allows you to accelerate the transfer of files into S3 using 'Amazon S3 Transfer Acceleration', by uploading content to the edge location, before it's sent to the origin, over an optimized network path
* You are able to white/black list any geographic locations of your CloudFront Distribution using Geo-Restrictions to prevent certain edge locations from having access to content
## Key Terms:
* **Edge Location**  
Location in the geographically distributed network where the content is cached, separate to an AWS Region/AZ
* **Origin**  
This is the origin of the content that the CDN will distribute to the edge locations. Origins can be one of the following. Note that you can have multiple origins
    * S3 Buckets
    * EC2 Instance
    * Elastic Load Balancer
    * Route 53
* **Distribution**  
This is the name given to instance of CloudFront using the CDN which consists of a collection of Edge Locations. There are two types of distributions.  
    * **Web Distribution** (http/https)  
    Typically used for websites
    * **RTMP** (Real time messaging protocol)  
    Used for media streaming
# Lambda:
AWS Lambda is a compute service where you upload your code and create a Lambda function. AWS Lambda takes case of provisioning and managing the servers that are used to run the code. AWS Lambda handles the operating systems, patching, scaling, etc.
## Read:
For services that generate a queue or data stream, you create an event source mapping in Lambda and grant Lambda permission to access the other service in the execution role. Lambda reads data from the other service, creates an event, and invokes your function. Services That Lambda reads events from are
* Kinesis
* DynamoDB
* Simple Queue Service
## Triggers:
Other services invoke your function directly. You grant the service permission in the function's resource-based policy, and configure the service to generate events and invoke your function. Depending on the service, the invocation can be synchronous or asynchronous.
### Synchronous:
For synchronous invocation, the other service waits for the response from your function and might retry on errors.
* Application Load Balancer
* API Gateway
* Cognito
* Lex
* Alexa
* CloudFront (Lambda@Edge)
* Kinesis Data Firehose
* Step Functions
### Asynchronous:
For asynchronous invocation, Lambda queues the event before passing it to your function. The service gets a success response as soon as the event is queued and isn't aware of what happens afterwards. If an error occurs, Lambda handles retries, and can send failed events to a dead-letter queue that you configure.
* Simple Storage Service
* Simple Notification Service
* Simple Email Service
* CloudFormation
* CloudWatch Logs
* CloudWatch Events
* CodeCommit
* Config
* IoT
* IoT Events
* CodePipeline
## Languages:
* Node.js
* Java
* Python
* C#
* Powershell
* GO
* Ruby
## Pricing:
*AWS Lambda free usage tier includes 1 million free requests, and 400 000GB/s compute time per month.*

It's priced on the number of requests and the duration of the code to execute (rounded to nearest 100ms). The price depends on the amount of memory allocated to your function. Pricing is region specific, but seems to be about $0.20 per 1 million requests.

Data transferred 'in' to and 'out' of your AWS Lambda functions from outside the region the function executes in will be chanted at the EC2 data transfer rates. Data transfer between various AWS Services within the same region are free. The use of VPC and VPC peering will incur additional charges.
* **Priced at (dependant on memory allocated)**
    * Number of Requests
    * Duration of execution
* **Options**
    * Provisioned Concurrency
* **Other Charges**
    * Data Transfer between regions
    * VPC and VPC Peering

There is no free tier for Lambda@Edge currently. Lambda@Edge counts a request each time it starts executing in response to a CloudFront event globally.

Request pricing is $0.60 per 1 million requests ($0.0000006 per request).
## Functions:
When authoring a function you can choose one of three different ways to do so in the AWS Console.
* From scratch
* Blueprints  
Preconfigured template as a starting point
* Serverless Application Repository  
Find and deploy serverless apps published by developers, companies, and partners on AWS
## Versioning:
When you use versioning, you can publish one or more versions of your function, allowing you to work with different variations of your function in your workflow, such as development, beta, production etc. Each version has a unique ARN, and after being published a version is Immutable.  
The latest version of code can be referenced by using `$Latest`, and this version is the only one who's code is editable.

When referring to a function using the ARN, there are two references you can use.
* Qualified ARN:
The function ARN with the version suffix  
`arn:aws:lambda:aws-region:acct-id:function:helloworld:$Latest`
* Unqualified ARN:
The function ARN without the version suffix  
`arn:aws:lambda:aws-region:acct-id:function:helloworld`
## Alias:
You can create an Alias (e.g. Prod) that points to a specific version of your deployed Lambda function, and then reference this alias to invoke the function.  
This allows you to later deploy a newer version of the function, and promote this to Prod by simply 'remapping' the alias to this new version.  

Using an Alias you can 'shift' traffic between two versions of a function based on a percentage. In other words if you have a Prod alias, and then create a new beta alias with new code, you can shift a percentage of the traffic from the Prod function, to the beta function code using a new alias.  

The ARN looks as follows when referencing a function by it's alias.  
`arn:aws:lambda:aws-region:acct-id:function:helloworld:alias`
## Notes:
* In the AWS Resource model, you choose the amount of memory you want for your function, and are allocated proportional CPU power and other resources. An increase in memory size triggers an equivalent increase in CPU available to your function.
* You can enable Provisioned Concurrency for your Lambda functions which keeps functions initialized and hyper-ready to respond in double-digit milliseconds. You pay for the amount of concurrency that you configure and for the period of time that you configure it.
* **Lambda scales out, not up, automatically**
* **Functions are independent, 1 event = 1 function**
* **Lambda is a serverless, compute service**
* **Lambda functions can trigger other Lambda functions, 1 event = n functions**
* AWS X-ray is a service that allows you to debug Lambda functions
# API Gateway:
API gateway is a fully managed service that makes it easy to maintain, monitor, and secure API's at any scale. API Gateway acts as a 'front door' for application access data, business logic, or functionality from your back end service (e.g. EC2, Lambda, DynamoDB etc.).
## Resources:
Resources can be be used to determine the 'path' of the request URL, as well as the 'method' that is used to make the request.  
Resources are also used to create path input variables, but wrapping the resource path in {} braces.  
The Method resource is where the request and integration configuration is done
## Methods:
Each Method has 4 sets of configuration
* Method Request/Response  
This configuration deals with the incoming http request, and responding back to the client with an HTTP response. This is where you specify the input and output parameters, and validation on those parameters.
* Integration Request/Response  
This configuration deals with integrating the HTTP request and response with the AWS service selected (e.g. Lambda). This is where you can map the incoming parameters, to models, which will be passed directly to your code in a Lambda, or you can configure this to run under 'Proxy integration' where the incoming request is simply proxie'd directly on to the Lambda code.
### Method Integration types:
* Lambda Function
* Http
* Mock
* AWS Service
* VPC Link
## Models:
Models are simply json schema's that represent a model that can be used to map incomping request parameters into models that are then passed on to the selected AWS Service (e.g. Lambda)
## Stages:
Stages are actual 'deployments' of the configured Resources.  
Each time a set of resources is created or updated the changes can be saved, but these changes will not take affect until they are deployed to a stage (deploying means taking current configuration, and packaging it as a point in time of the configuration).  
Stages keep a history of 'deployments' and allow you to very easily roll back or forward to any of the deploys (configuration changes) that were made.  
The stage forms part of the request URL, and appear just after the domain section.  
`http://test.com/{stagename}/resources`
## API Caching:
You can enable API caching in API Gateway to cache your endpoints response, this will reduce the number of calls made to your endpoint and also improve the latency of requests. API gateway caches responses for a specific time-to-live (TTL) period in seconds.
## API Throttling:
Api Gateway limits the steady state request rate to 10 000 requests per second (rps). The maximum concurrent request rate is 5000 requests across all API's within an account.  
Exceeding these limits will result in a `429 Too Many Request` error.
* If 10 000 requests are received evenly, spread over 1 second (10/ms), all requests are processed.  
* If 10 000m requests are received in the first millisecond, 5 000 of those requests are served with the rest of the requests being throttled across the one second period
* If 5 000 request are received in the first millisecond, and a further 5 000 requests are received evenly over the remaining one second period, all requests are served
## Authorization:
TODO: complete a paragraph explaining Authorization headers
## Usage Plans with API Keys:
You can configure usage plans and API keys to allow customers to access selected APIs at agreed-upon request rates and quotas that meet their business requirements and budget constraints. If desired, you can set default method-level throttling limits for an API or set throttling limits for individual API methods
## Note:
* Api Gateway can expose HTTPS endpoints to define a restfull API.
* You can configure API Gateway as a SOAP web service passthrough
* Can send each API endpoint to different targets (e.g. EC2, Lambda, DynamoDB etc.)
* Track and control usage by API key
* Throttle requests to prevent attacks
* Can use Cloudwatch to monitor
* Uses API Gateway domain by default, but you can use custom domain
* Supports AWS Certificate Manager (free SSL/TLS certs)
* API Gateway can have CORS enabled to allow multiple domain requests
* You can import an API from an external definition file to create an API Gateway (e.g. swagger).  
You can even create a new API or update an existing API buy making a rest Post or Put call and including the swagger definition.
# Step Functions:
Step Functions allow you to visualize and test your serverless applications, they provide a graphic console to arrange components of your application (e.g Lambda functions) as a series of steps, making it simple to build and run multistep applications.  
Step Functions automatically trigger and track each step and retry when an error occurs, logging the state of each step to help diagnose errors.  
In other words, Step Functions allows you to orchestrate and run a workflow of Lambda functions, with built in retry and error logging.
# Amazon Polly:
Amazon Polly converts text to lifelike speech in the cloud. You can download the generated audio from the console, or stream it directly to your applications and services through the API.
# X-Ray:
AWS X-Ray is a service that collects data about requests that your application serves, and provides tools you can use to view, filter, and gain insights into that dta to identify issues and opportunities for optimization.
The X-Ray SDK provides
* Interceptors to add to your code to trace incoming HTTP requests
* Client handlers to instrument AWS SDK clients that your application uses to call other AWS Services
* An Http Client to use to instrument calls to other internal and external HTTP web services

X-Ray integrates with the following services
* Elastic Load Balancing
* AWS Lambda
* Api Gateway
* Elastic Compute Cloud
* Elastic Beanstalk
# DynamoDB:
Fast and flexible serverless, NoSQL database, offering consistent, single digit latency at scale. It's a fully managed database supporting document and key-value data models.
## Storage:
* Stored on SSD storage
* Spread across 3 geographically distinct data centers
* 2 Consistency models
    * Eventual Consistent Reads (default)
    Consistency across all copies is reached within 1 second. Repeating a read after short time should return the updated data.  
    (Best read performance)
    * Strongly Consistent Reads
    Always reflects all writes that received a successful response prior to the read
## Structure:
* Tables
* Items (like a row of data)
* Attributes (like a column of data)
* Supports Key-Value storage
* Supports document storage
    * JSON
    * HTML
    * XML
## Data:
Data is stored based on a Primary Key, of which there are two types.
* **Partition Key (unique attribute):**  
The value of the partial key is input to an internal hash, which determines the partition (physical location) on which the data is stored. Partition keys are therefore unique attributes.
* **Composite Key (partition key + sort key):**  
This is useful of the partition key is not unique (e.g. user id, if the user can exist multiple times)
## Access control:
AWS IAM
* A role can be given specific permissions to access and create tables
* You can create a role which enables temporary access keys
* You can use a special `IAM Condition` to restrict user access to only their records.
This can be done by adding a condition to the IAM Policy to allow access only to items where the Partition Key value matches the User ID
## Indexes:
An index is a data structure which contains selected attributes (think columns) of data, allowing you to search on the index, rather then the entire dataset.
### Local Secondary Index
* Can only be created when creating the table, and cannot be modified later
* Same partition key as original Table, but a different sort key
* Gives a different view of the data, organised according to an alternative sort key
* Queries based on this sort key are much faster using the index
### Global Secondary Index
* Can be created when creating the table, but also modified later
* Different partition key as well as different sort key
* This gives a completely different view of the data
* Speeds up queries relating this this alternative partition and sort key
## Data retrieval:
### Queries:
A query operation finds items in a table based on a distinct value against the *primary key* attribute (e.g. select an item where `user id = 212`)
* Use an optional Sort Key name and value to refine search (e.g. )
* By default returns all the attributes for an item, using the `ProjectExpression` parameter returns only the specific attribute
* By default numeric order is Ascending, setting the `ScanIndexForward`parameter to false changes it to descending
* By default queries are eventually consistent, you can set the query to be Strongly Consistent
### Scan:
A scan operation examines every item in the table, and can then be filtered by one or more attributes
* By default returns all the attributes for an item, using the `ProjectExpression` parameter returns only the specific attribute
* A scan is less efficient than a query, because it dumps the entire table before applying filters
### Improve Performance:
* Reduce the impact of a query or scan by setting the page size smaller
* Larger number of smaller operations will allow other requests to succeed without throttling
* Avoid using scan operations, design tables in a way to use `Query`, `Get` or `BatchGetItem` api's
* Scan operations process data sequentially in 1MB increments, one partition at a time. Scans can be configured to use Parallel scans, by logically dividing a table or index into segments, but this is not recommended if the table or index is already incurring heavy read/write activity
## Pricing modals:
### Provisioned Throughput:
Provisioned throughput is measured in Capacity Units. When you create a table, you specify your requirements in terms of the *Read Capacity Units* and *Write Capacity Units*. This is meant for predictable consistent application traffic, where you cn forecast capacity requirements.
* 1 Write Capacity Unit = 1x 1KB write per second
* 1 Read Capacity Unit = 1x 4KB Strongly Consistent read per second
* 1 Read Capacity Unit = 2x 4KB Eventually Consistent read per second (default)
#### Provisioned Throughput Exceeded Exception:
The ProvisionedThroughputExceededException occurs when your request rate is too high for the read/write capacity provisioned. If you're using the SDK, it will automatically retry the request until successful in this case. If not you can try reduce the request frequency, or use `Exponential Backoff`
* Exponential Backoff is functionality that exists in all AWS SDKs, and it allows progressively longer waits between consecutive retries
### On-Demand Capacity:
On-Demand Capacity charges for Reading, writing and storing. You don't need to specify your requirements, and DynamoDB instantly scales up and down based on activity. You pay for only what you use (per request). This is god for unpredictable workloads.
## DynamoDB Accelerator (DAX):
DAX is a fully managed, clustered in-memory cache for DynamoDB, which improves read performance.  
Delivers up to 10x read performance, and microsecond performance for millions of requests per second. Ideal for read heavy and bursty workloads.  
DAX is a write-through caching service, meaning data is written to the cache, and then stored in the back end.  
If the item being queried is in the cache (cache hit), it is simply returned. If the item being queried is not available in the cache (cache miss), DAX performs an **Eventually Consistent** GetItem operation against the DB.  
DAX is not suitable for **Strongly Consistent** reads, or write intensive applications
## DynamoDB Transactions:
ACID transactions (Atomic, Consistent, Isolated, Durable), allows you to read or write items across multiple tables as an all or nothing operation. Transactions also allow you to check for a pre-requisite condition before writing to a table.
## DynamoDB Time to Live (TTL):
The time to live attribute defines an expiry time for your data. TTL is expressed as an epoch, and once the current time is greater than the TTL, the item is marked for deletion. You can also filter out expired items from your queries.
## DynamoDB Streams:
DynamoDB Streams are a time ordered sequence of item level modifications logs (insert, update, delete).
* Logs are encrypted at rest and stored for 24 hours.
* The logs can be accessed using a dedicated endpoint
* By default the *Primary Key* is recorded
* Events are recorded in near real-time
* Event source for Lambda, so applications can take actions based on contents. And Lambda can poll the stream
## Summary:
* DynamoDB is low latency NoSQL database
* Consists of Tables, Items, and Attributes
* Supports Document and Key-Value data models
* Document formats are JSON, HTML, XML
* 2 types of primary keys, **Partition Key**, and **Partition Key + Sort Key**
* 2 consistency models, **Eventually Consistent** and **Strongly Consistent**
* Access controlled using IAM Policies
* Fine grained access control using IAM Conditions
* 2 types of indexes, **Local Secondary Index**, and **Global Secondary Index**
# Key Management Service (KMS):
This is a managed service that makes it easy for you to create and control the encryption keys used to encrypt data. You can either create 'AWS managed keys' or 'Customer managed keys'
## Integrated:
KMS is integrated with the following AWS services
* EBS
* S3
* Redshift
* Elastic Transcoder
* Workmail
* RDS
## Customer Master Key:
A customer master key can be created in AIM, and best practice dictates that you should have two users, one that can administer the keys, and a second user that can encrypt/decrypt using the keys.
All of this is linked and set up when making a KMS Customer Master Key, which consists of
* Alias
* Creation Date
* Description
* Key State
* Key Material (either customer provided or AWS provided)
## AWS KMS API calls:
* encrypt (encrypt plain text file)
* decrypt (decrypt plain text file)
* re-encrypt (encrypts an already encrypted file, effectively doing a decrypt and encrypt, but does not generate a plain text file)
* enable-key-rotation (rotates the keys yearly)
## Data Key (Envelope key):
A Customer Master Key is used to encrypt/decrypt a **data key** (envelope key). This data key, once decrypted, can then be used to encrypt/decrypt data in a database, for example.  
i.e. We're encrypting keys that are used for encryption. 
## Note:
* Keys can **never** be exported
* KMS is regions specific, a key generated in one region, cannot be used in another region
# Simple Queue Service (SQS):
Web service that give you access to a message queue, storing messages while waiting for them to be processed. Amazon SQS is a distributed queue system that enables web service applications to quickly and reliably queue messages that one component in an applications generates, to be consumed by another.  
i.e. It is a temporary repository for messages awaiting processing. It can decouple components if an application so they can run independently.
* SQS is a pull-based system
* Messages are limited to 256KB
* Can be stored in queue from 1 minute - 14 days (default retention is 4 days)
* SQS guarantees that your messages will be processed at least once
## Standard queue (default):
Nearly unlimited number of transactions per second, and a guarantee that a message will be delivered at least once. (occasionally more than one copy of a message might be delivered out of order). Standard queues provide best effort ordering.
## Fifo queue (First in, First out):
This complemented the Standard queue, and the order in which messages are send and received is strictly preserved, and a message is delivered once and remains available until a consumer processes and deletes it. Duplicates are not introduced into the queue. Fifo also supports message groups, that allow multiple ordered message groups within a single queue. Queues are limited to 300 transactions per second.
## Visibility Timeout:
Is the amount of time that the message is invisible to the SQS queue, after a reader picks up the message. Provided the job is processed before the visibility timeout expires, the message will be deleted, else it will become visible again.
* Default visibility timeout is 30 seconds
* Maximum is 12 hours 
## Long Polling:
This is a way of retrieving messages from SQS, while normal short polling returns immediately with a message if one is available, or nothing if no messages are available, Long polling does not return a response if no messages are available, but rather waits for a message to become available for a set amount of time. This can save money be requiring less api calls.
## Simple Notification Service (SNS):
It provides scalable, flexible, and cost effective capability to publish (push) messages from an application and immediately deliver them to a subscribers or other applications. It can push notifications to various devices (e.g. Apple, Google, Fire OS, Windows, Android).  
It can also trigger lambda functions:  
When a message is published to an SNS topic that has a Lambda function subscribed to it, the function is invoked with the payload of the published message.
## SNS Topic:
SNS allows you to group multiple recipients using topics. A topic is an 'access point' for allowing recipients to dynamically subscribe for identical copies of the same notification.
## Note:
* SNS follows the (pub-sub) paradigm, and is a push based system
* All messages published to SNS are stored redundantly across multiple availability zones
* Instantaneous push based delivery (no polling)
* Flexible message delivery over multiple transport protocols (e.g. SMS, Email, SQS Queues, and HTTP endpoints)
* inexpensive, pay as you go model with no upfront costs
# Amazon Simple Email Service (SES):
Is a scalable and highly available email service designed to help marketing teams and application developers send marketing, notification, and transactional emails to customers using a pay as you go model.  
It can also be used to receive emails, having them delivered to an S3 bucket. Incoming emails can be used to trigger Lambda functions, and SNS notifications.
# Elastic Beanstalk (EBS):
Is a service for deploying and scaling web applications developed many languages.
You upload your code and Elastic Beanstalk handles deployment, capacity provisioning, load balancing, and auto-scaling as well as the application health.  
You still retain full control of the underlying AWS resources powering your application, and you only pay for the resources used to store and run the application.

**Think of Elasitc Beanstalk as the abstraction layer between running your own EC2 instance, and AWS Lambda**
## Notes:
* Deploys and scales your web application, including the web application server platform where required
* Supports widely used programming technologies
    * Java
    * PHP, Python
    * Ruby
    * Go
    * Docker
    * .Net
    * Node.js
* Provisions the underlying resources for you
* Can fully manage the EC2 instance for you, or allow full admin control to you
* Updates, monitors, metrics, and health checks included
## Updating EBS:
* **All at once:**  
Updates and deploys the new version to all instances simultaneously. Meaning that all instances are out of service while deployment takes place. If the update fails you need to roll back the changes by re-deploying original version.
* **Rolling:**  
Deploys the new version in batches, each batch of instances is taken out of service while deployment takes place. Meaning that your environment capacity wil reduce by the number of instances in the batch while deployment takes place. If the update fails you need to perform additional rolling updates to roll back changes.
* **Rolling with additional Batch:**  
Similar to Rolling, however additional batch of instances is launched. Meaning that full capacity is maintained while deployment takes place. If the update fails you need to perform an additional rolling update to roll back the changes.
* **Immutable:**  
Deploys new version to a fresh group of instances in their own autoscaling group. Once new instances pass their health checks, they are moved to the existing auto scaling group, and the old instances are terminated. Meaning that full capacity is maintained while deployment takes place. The impact of a failed deploy is far less, and the rollback process requires inly terminating the new auto scaling group
## Configuration:
EBS environments can be configured using Elastic Beanstalk configuration files, which can be written in either YAML or JSON formats. The configuration files can have a name of your choosing, but must end in an `.config` extension, and must be placed in a top level `.ebextensions` directory.
## RDS and EBS:
EBS supports two ways of integrating an RDS database with the EBS environment
* **Launch RDS instance from within EBS console**  
However this is not ideal for production environments, as the lifecycle of your database is tied to the lifecycle of your application environment. If you terminate the environment, the database will be terminated as well. This is better used for testing environments.
* **Launch RDS instance from the RDS section of the AWS Console**  
This is the preferred option, allowing your RDS instance to be decoupled from your EBS environment. This gives you more flexibility, and allows connection of multiple environments to your database, as well as providing a larger choice of database types.  
There are two additional configurations that are required to allow an EC2 instance in an EBS environment to access an external RDS database
    * Security Group must be added to the Auto Scaling group, allowing access to the DB
    * Connection string configuration in your application
# Serverless:
The following AWS services are serverless (i.e. don't run on a provisioned EC2 instance)
* Lambda
* SQS
* SNS
* API Gateway
* Cognito
* Cloudwatch
* DynamoDB
* Kinesis
* Fargate
* Step Functions
* Aurora
# AWS CodeCommit:
Is a fully managed Source Control service providing secure, highly scalable private Git Repositories.
CodeCommit provides all the functionality of Git and you can use Git on your local machine to communicate with your CodeCommit repository. All your data is encrypted in transit.
* Based on Git
* Centralized repository for all your code, binaries, images and libraries
* Tracks and manages code changes
* Maintains version history
* Manages updates from multiple sources and enables collaboration
# AWS CodeDeploy:
Is an automated deployment service which allows you to deploy your application code automatically to EC2 instances, on-premises systems and Lambda functions. It allows you to quickly release new features, avoid downtime during application deployments, and avoid risks associated with manual deploys.

It automatically scales with your infrastructure and integrates with various CI/CD tools, (e.g. jenkins, Github, Atlasian, AWS CodePipeline).

There are two deployment approaches available:
* **In-Place (Rolling update):**  
The application is stopped on each instance and the latest revision installed. This means that the instance is out of service during this time, and you capacity will be reduced, and in order to roll back changes, the previous version of the application must be re-deployed.
    * if your code is behind a load balancer, you can configure tha load balancer to stop sending traffic to this instance
    * It is only available for EC2 and on-premises systems, and is not supported with Lambda
* **Blue/Green:**  
New instances are provisioned and the latest revision is installed on these. The new instances are registered with an Elastic Load Balancer, traffic is then routed to the new instances, and the original instances are eventually terminated. The new instances can be created ahead of time, and the code released to production simply by routing traffic to the new instances. Also allowing roll back to be done by switching traffic back to the original instances.  
Blue represents the active version, while green is the new release version.
## Terminology:
* **Deployment Group:**  
A set of EC2 instances or Lambda functions to which the new revision of the software is to be deployed
* **Deployment:**  
The process and components used to apply the new revision
* **Deployment Configuration:**  
A set of deployment rules as well as success / failure conditions
* **AppSpec File:**  
Defines the deployment actions you want CodeDeploy to execute, as well as all the parameters needed (e.g location, pre/post deployment validation). Note this file only supports YAML.  
    * For EC2 or on-premises systems, the appspec.yml file must be placed in the root directory.
    * Lambda supports yaml and json
    * The AppSpec file supports **hooks**, which allow you to specify code, scripts, or functions that you want to run at set points in the deployment life cycle.
* **Revision:**  
Everything needed to deploy the new version (e.g. AppSpec file, application files, executables, config files)
* **Application:**  
Unique identifier for the application you want to deploy
# AWS CodePipeline:
Is a fully manage Continuous Integration and Continuous Delivery Service. CodePipeline can orchestrate the Build, Test and Deployment of your application based on code commits.
It integrates with CodeCommit, CodeBuild, CodeDeploy, Lambda, EBS, ECS, and CloudFormation
# CodeBuild & Docker:
## Note:
* Docker commands to build, tag, and push image to Elastic Container Repository (ECR).
    * `docker build -t myimagerepoo`
    * `docker tag myimagerepoo:latest ...`
    * `docker push ...`
* Use the buildspec.yml to define the build commands and settings used by CodeBuild to run the build
* You can override the settings in the buildspec.yml by adding your own commands in the console when launching the build
* CodeBuild supplies logs of the deploy incase of failure. You can also view the full logs in CloudWatch
# CloudFormation:
Is a service that allows you to manage, configure and provision your AWS infrastructure as code.
Resources are defined using a CloudFormation template, that is interpreted and the appropriate APU calls are made to create the resources defined. The template file supports YAML and JSON. Can be used to manage updates and dependencies, and you can rollback and delete the entire stack.
## Template sections:
* **Parameters**  
input custom values
* **Conditions**  
provision resources based on environment
* **Resources** - required  
the AWS resources to be created
* **Mappings**  
create custom mappings like region : AMI
* **Transforms**  
reference code located in S3
## Nested Stacks:
CloudFormation allows for **Nested Stacks**, which are simply stacks, which create other stacks. This allows for re-use of CloudFormation code for common use cases (e.g. standard configuration for load balancer, web server, application server etc.). Instead of copying out the code each time, create a standard template for each common use case, and reference from within your CloudFormation template.
# Serverless Application Model (SAM):
Is an extension to CloudFormation, with simplified syntax for defining serverless resources like API's, Lambda functions, and DynamoDB Tables. Use the SAM cli to package your deployment code, upload it to S3 and deploy your serverless application.  

Nested stacks are declared within the **Resources** section. Two important properties of this section are:
* `Type: AWS::CloudFormation::Stack`
* `TemplateURL: https://s3amaonaws.com/.../template.yml` - required
## CLI Commands:
* `sam package --template-file --output-template-file --s3-bucket`  
Outputs a sam compatible template, and uploads a deployment package to an S3 bucket
* `sam deploy --template-file --stack-name --capabilities`  
Takes as input the the template file outputted from the package command, the name of the cloudFormation stack to be created, and the capabilities. One of the capabilities flags is CAPABILITY_IAM, which allows CloudFormation to create an IAM role on your behalf that will be used to allow the function to execute.
# Cloudwatch:
Is a monitoring service to monitor your aws resources, as well as the applications that you run in AWS.  
***Note: Cloudwatch can be used on premise, by downloading and installing SSM agent and ClourWatch agent, so it's not restricted to just AWS resources***  
Cloudwatch can monitor things such as:
* Compute
    * Autoscaling Groups
    * Elastic Load Balancers
    * Route53 Health Checks
* Storage & Content Delivery
    * EBS Volumes
    * Storage Gateways
    * CloudFront
* Databases & Analytics
    * DynamoDB
    * Elasticache Nodes
    * RDS Instances
    * Elastic MapReduce Job flows
    * Redshift
* Other
    * SNS Topics
    * SQS Queues
    * Opsworks
    * CloudWatch Logs
    * Estimated Charges on your AWS Bill
## EC2:
By default CloudWatch can monitor these host level metrics:
* CPU
* Network
* Disk
* Status Check  
***Note: Ram utilization is a custom metric, by default EC2 monitoring is in 5 minute intervals, unless you enable detailed monitoring, which will use 1 minute intervals***
## Retention:
You can store log data in CloudWatch logs for as long as you want, by default CloudWatch logs will store your log data indefinitely, but you can change retention for each 'Log Group' at any time.  
***Note: you can retrieve data from any terminated EC2 or ELB instance after it's termination***
## Metric Granularity:
* 1 minute for detailed monitoring
* 5 minutes for standard monitoring
***Note: For custom metrics, the minimum granularity that you can have is 1 minute***
## CloudWatch Alarms:
You can create an alarm to monitor any metric in your account, this can include  
e.g. EC2 CPU Utilization, ELB latency, or even charges on your AWS bill.  
You can set the appropriate thresholds in which to trigger the alarms, and also set what actions should be taken if an alarm state is reached.
# CloudWatch vs CloudTrail vs Config:
* CloudWatch monitors performance
* CloudTrail monitors API calls in the AWS platform
* AWS Config records the state of your AWS Environment, and can notify you of changes