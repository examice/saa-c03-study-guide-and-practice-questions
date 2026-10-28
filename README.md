# SAA-C03 study guide and practice questions

SAA-C03 is the exam behind the AWS Certified Solutions Architect - Associate certification. It is meant for people who design systems on AWS, and it checks whether you can choose the services and patterns that keep a workload secure, available, fast, and affordable, measured against the AWS Well-Architected Framework. This repository holds a study guide mapped to the official exam guide, a study plan for six weeks, a cheat sheet with links to the AWS documentation behind each fact, and 15 original practice questions with explanations.

> These are unofficial study notes by [Examice](https://examice.com/exams/amazon/saa-c03?utm_source=github&utm_medium=social&utm_campaign=saa-c03-study-guide-and-practice-questions&utm_content=link_1). Examice is not affiliated with or endorsed by Amazon Web Services. The practice questions are original and are not taken from the exam.

## Contents

- [Exam at a glance](#exam-at-a-glance)
- [What the exam covers](#what-the-exam-covers)
- [Study plan](#study-plan)
- [Cheat sheet](#cheat-sheet)
- [Free official resources](#free-official-resources)
- [Practice questions](#practice-questions)
- [Exam day tips](#exam-day-tips)
- [FAQ](#faq)
- [Contributing](#contributing)
- [License](#license)

## Exam at a glance

| Fact | Detail |
| --- | --- |
| Exam code | SAA-C03 |
| Certification | AWS Certified Solutions Architect - Associate |
| Level | Associate |
| Questions | 65: 50 scored and 15 unscored. The unscored questions are not marked. |
| Question types | Multiple choice (one correct answer out of four) and multiple response (two or more correct answers out of five or more) |
| Duration | 130 minutes |
| Passing score | 720 on a scaled score of 100–1,000 |
| Scoring | Pass or fail. The model is compensatory, so you need to pass the exam as a whole, not each domain. Unanswered questions count as wrong, and there is no penalty for guessing. |
| Price | 150 USD; local currency prices and taxes are on the exam pricing page |
| Delivery | Pearson VUE test center or online proctored |
| Languages | English, French (France), Italian, Japanese, Korean, Portuguese (Brazil), Spanish (Latin America), Spanish (Spain), Simplified Chinese, and Traditional Chinese |
| Language retirement | The Italian version of the exam retires after December 31, 2026 |
| Recommended experience | At least 1 year of experience designing cloud solutions that use AWS services |
| Validity | 3 years |
| Recertification | Pass the latest version of this exam or pass AWS Certified Solutions Architect - Professional (each adds 3 years), or maintain the certification on AWS Skill Builder with a paid subscription (adds 1 year) |
| Results | In your AWS Certification Account within 5 business days |
| Retakes | Wait 14 calendar days after a failed attempt; no limit on attempts; full fee each time |

Source: [AWS Certified Solutions Architect - Associate exam page](https://aws.amazon.com/certification/certified-solutions-architect-associate/), [SAA-C03 exam guide](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-associate-03/solutions-architect-associate-03.html), [recertification](https://aws.amazon.com/certification/recertification/), [exam pricing](https://aws.amazon.com/certification/policies/before-testing/), and [after testing](https://aws.amazon.com/certification/policies/after-testing/), checked October 28, 2026.

## What the exam covers

| Domain | Weight of scored content |
| --- | --- |
| 1. Design Secure Architectures | 30% |
| 2. Design Resilient Architectures | 26% |
| 3. Design High-Performing Architectures | 24% |
| 4. Design Cost-Optimized Architectures | 20% |

The exam guide's [revision history](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-associate-03/saa-03-revisions.html) lists version 1.1 with a publication date of October 29, 2026; that revision removes Amazon Elastic Transcoder from the services in scope. The full lists are in [in-scope services](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-associate-03/saa-03-in-scope-services.html) and [service mentions](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-associate-03/saa-service-mentions.html). The summaries below are written in our own words; the exam guide has the official task statements.

### Domain 1: Design secure architectures (30%)

Three task areas: access to AWS resources, secure workloads and applications, and data security controls.

- Protect the root user with MFA, then build access from IAM users, groups, roles, and policies with least privilege. Use temporary credentials from AWS STS and roles for access between accounts. See [IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html) and [IAM security best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html).
- Govern many accounts with AWS Organizations, [service control policies](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html), and [AWS Control Tower](https://docs.aws.amazon.com/controltower/latest/userguide/what-is-control-tower.html). Know when to federate an existing directory and when [IAM Identity Center](https://docs.aws.amazon.com/singlesignon/latest/userguide/what-is.html) fits.
- Know which controls AWS owns and which you own under the [shared responsibility model](https://aws.amazon.com/compliance/shared-responsibility-model/).
- Design a VPC with public and private subnets, route tables, NAT gateways, [security groups, and network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/infrastructure-security.html#VPC_Security_Comparison).
- Match the threat to the service: [AWS WAF](https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html) for web exploits such as SQL injection, [AWS Shield](https://docs.aws.amazon.com/waf/latest/developerguide/shield-chapter.html) for DDoS, [Amazon GuardDuty](https://docs.aws.amazon.com/guardduty/latest/ug/what-is-guardduty.html) for threat detection, [Amazon Macie](https://docs.aws.amazon.com/macie/latest/user/what-is-macie.html) for sensitive data in S3, [Amazon Cognito](https://docs.aws.amazon.com/cognito/latest/developerguide/what-is-amazon-cognito.html) for app sign-in, and [AWS Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/rotating-secrets.html) for credentials.
- Connect other networks with [AWS Site-to-Site VPN](https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN.html) or [AWS Direct Connect](https://docs.aws.amazon.com/directconnect/latest/UserGuide/Welcome.html).
- Encrypt data at rest with AWS KMS and in transit with TLS certificates from AWS Certificate Manager, write key policies, and [rotate keys](https://docs.aws.amazon.com/kms/latest/developerguide/rotate-keys.html).
- Meet retention and compliance rules with [AWS Backup](https://docs.aws.amazon.com/aws-backup/latest/devguide/whatisbackup.html), replication, lifecycle rules, and [S3 Object Lock](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html).

### Domain 2: Design resilient architectures (26%)

Two task areas: scalable, loosely coupled designs, and designs that stay available when parts fail.

- Decouple tiers with [Amazon SQS](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html), [Amazon SNS](https://docs.aws.amazon.com/sns/latest/dg/welcome.html), and [Amazon EventBridge](https://docs.aws.amazon.com/eventbridge/latest/userguide/eb-what-is.html), and coordinate multistep work with [AWS Step Functions](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html).
- Decide between containers ([Amazon ECS](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/Welcome.html), [Amazon EKS](https://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html), [AWS Fargate](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html)) and functions ([AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)), and front services with [Amazon API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html).
- Scale out with [Elastic Load Balancing](https://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/what-is-load-balancing.html) and [Amazon EC2 Auto Scaling](https://docs.aws.amazon.com/autoscaling/ec2/userguide/what-is-amazon-ec2-auto-scaling.html); know when vertical scaling, caching, or [read replicas](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ReadRepl.html) fit better.
- Spread workloads across Availability Zones and Regions, remove single points of failure, and fail over with [Route 53 routing policies](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html) and health checks.
- Pick a [disaster recovery strategy](https://docs.aws.amazon.com/whitepapers/latest/disaster-recovery-workloads-on-aws/disaster-recovery-options-in-the-cloud.html) from the RPO and RTO the business accepts.
- Use [RDS Proxy](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/rds-proxy.html), [AWS X-Ray](https://docs.aws.amazon.com/xray/latest/devguide/aws-xray.html), and service quotas in your design, including quotas for a standby environment.
- Make applications you can't change more reliable with managed services around them. The [Reliability Pillar](https://docs.aws.amazon.com/wellarchitected/latest/reliability-pillar/welcome.html) covers the design principles.

### Domain 3: Design high-performing architectures (24%)

Five task areas: storage, compute, databases, networks, and data ingestion and transformation.

- Storage: match object, file, and block storage to the workload with [S3 storage classes](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html), [EBS volume types](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-volume-types.html), [Amazon EFS](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html), [FSx for Windows File Server](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/what-is.html), [FSx for Lustre](https://docs.aws.amazon.com/fsx/latest/LustreGuide/what-is.html), and hybrid storage with [Amazon S3 File Gateway](https://docs.aws.amazon.com/filegateway/latest/files3/what-is-file-s3.html).
- Compute: pick [instance families and sizes](https://docs.aws.amazon.com/ec2/latest/instancetypes/instance-types.html), [Lambda memory](https://docs.aws.amazon.com/lambda/latest/dg/configuration-memory.html), AWS Batch, or [Amazon EMR](https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-what-is-emr.html), and scale on the right metric with [target tracking](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-scaling-target-tracking.html).
- Databases: choose the engine and type ([Amazon Aurora](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/CHAP_AuroraOverview.html), Amazon RDS, [Amazon DynamoDB capacity modes](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/capacity-mode.html)), plan capacity, and add replicas, proxies, and caches ([Amazon ElastiCache](https://docs.aws.amazon.com/AmazonElastiCache/latest/dg/WhatIs.html), [DAX](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DAX.html)).
- Networks: lay out subnet tiers and IP ranges, choose a load balancer, and choose between [Amazon CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html) and [AWS Global Accelerator](https://docs.aws.amazon.com/global-accelerator/latest/dg/what-is-global-accelerator.html) at the edge and VPN, Direct Connect, or [AWS PrivateLink](https://docs.aws.amazon.com/vpc/latest/privatelink/what-is-privatelink.html) for private connections.
- Data: stream with [Kinesis Data Streams](https://docs.aws.amazon.com/streams/latest/dev/introduction.html) or [Amazon Data Firehose](https://docs.aws.amazon.com/firehose/latest/dev/record-format-conversion.html), move data with [AWS DataSync](https://docs.aws.amazon.com/datasync/latest/userguide/what-is-datasync.html), transform with [AWS Glue](https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html) (for example CSV to Parquet), query with [Amazon Athena](https://docs.aws.amazon.com/athena/latest/ug/what-is.html), secure a data lake with [AWS Lake Formation](https://docs.aws.amazon.com/lake-formation/latest/dg/what-is-lake-formation.html), and build dashboards with [Amazon Quick](https://docs.aws.amazon.com/quick/latest/userguide/what-is.html), which grew out of Amazon QuickSight.

### Domain 4: Design cost-optimized architectures (20%)

Four task areas: storage, compute, databases, and networks. Each one also expects you to know the cost tools: [Cost Explorer](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html), [AWS Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html), [Cost and Usage Reports](https://docs.aws.amazon.com/cur/latest/userguide/what-is-cur.html), cost allocation tags, and consolidated billing.

- Storage: storage classes and [lifecycle rules](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html), [S3 Intelligent-Tiering](https://docs.aws.amazon.com/AmazonS3/latest/userguide/intelligent-tiering.html), EBS type and size, [Requester Pays](https://docs.aws.amazon.com/AmazonS3/latest/userguide/RequesterPaysBuckets.html), backup and archive choices, and the cheapest way to move data in with DataSync, [AWS Transfer Family](https://docs.aws.amazon.com/transfer/latest/userguide/what-is-aws-transfer-family.html), or Storage Gateway.
- Compute: [purchasing options](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-purchasing-options.html) (On-Demand, [Savings Plans](https://docs.aws.amazon.com/savingsplans/latest/userguide/what-is-savings-plans.html), Reserved Instances, [Spot](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-instances.html)), rightsizing with [AWS Compute Optimizer](https://docs.aws.amazon.com/compute-optimizer/latest/ug/what-is-compute-optimizer.html), [hibernation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Hibernate.html), and Lambda versus Fargate versus EC2 by usage pattern.
- Databases: DynamoDB versus RDS versus serverless options, snapshot frequency and retention, and migrations with [AWS DMS](https://docs.aws.amazon.com/dms/latest/userguide/Welcome.html) and the [AWS Schema Conversion Tool](https://docs.aws.amazon.com/SchemaConversionTool/latest/userguide/CHAP_Welcome.html).
- Networks: one [NAT gateway](https://docs.aws.amazon.com/vpc/latest/userguide/nat-gateway-basics.html) per Availability Zone or a shared one, [gateway endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/gateway-endpoints.html), Direct Connect versus VPN, [Transit Gateway](https://docs.aws.amazon.com/vpc/latest/tgw/what-is-transit-gateway.html) versus [VPC peering](https://docs.aws.amazon.com/vpc/latest/peering/what-is-vpc-peering.html), data transfer between Availability Zones and Regions, and when a CDN pays for itself.

## Study plan

Six weeks at 8–10 hours a week, about 55 hours in total. If you already work on AWS every day, compress weeks 2–5 into two weeks and spend the time saved on practice.

| Week | Focus | Tasks | Hours |
| --- | --- | --- | --- |
| 1 | Baseline and foundations | Read the exam guide. Take the free Official Practice Question Set cold and note your weak domains. Create a Free Tier account and build a VPC with public and private subnets, a NAT gateway, and one EC2 instance in each subnet. Delete the NAT gateway when you finish; it bills by the hour. | 8 |
| 2 | Domain 1: security | IAM policy evaluation, roles across accounts, SCPs, KMS keys and key policies, S3 encryption options, Secrets Manager, WAF, and Shield. Lab: encrypt a bucket with SSE-KMS and read the CloudTrail events for the key; block an IP range with a network ACL. | 9 |
| 3 | Domain 2: resilience | ELB types, Auto Scaling, SQS, SNS, EventBridge, Lambda, Step Functions, RDS Multi-AZ and read replicas, Aurora, Route 53 routing policies, and the disaster recovery whitepaper. Lab: an Auto Scaling group behind an ALB that scales on an SQS backlog. | 10 |
| 4 | Domain 3: performance | EBS, EFS, FSx, and S3 performance; ElastiCache, DAX, and CloudFront; Global Accelerator; Kinesis, Firehose, Glue, and Athena. Lab: send JSON to Firehose, convert it to Parquet, and query it in Athena. | 10 |
| 5 | Domain 4: cost | Purchasing options, S3 storage classes and lifecycle rules, NAT gateway versus VPC endpoints, data transfer paths, Cost Explorer, and Budgets. Lab: write a lifecycle rule and set a monthly budget alert. | 8 |
| 6 | Practice and review | Work through the 15 questions in this repository, retake the Official Practice Question Set, watch the free Skill Builder domain reviews for your weakest domains, and do two timed sets at 2 minutes per question. Book the exam when you score well on questions you haven't seen. | 10 |

## Cheat sheet

### Domain 1: security

Security groups versus network ACLs ([VPC docs](https://docs.aws.amazon.com/vpc/latest/userguide/infrastructure-security.html#VPC_Security_Comparison)):

| | Security group | Network ACL |
| --- | --- | --- |
| Applies at | Instance level | Subnet level |
| Rules | Allow only | Allow and deny |
| Evaluation | All rules, then decide | In ascending rule number until a match |
| Return traffic | Allowed automatically (stateful) | Must be allowed explicitly (stateless) |

S3 server-side encryption ([S3 docs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/serv-side-encryption.html), [SSE-KMS](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingKMSEncryption.html)):

| Option | Key managed by | When to pick it |
| --- | --- | --- |
| SSE-S3 | Amazon S3 | The default. Every new object upload has been encrypted this way since January 5, 2023. |
| SSE-KMS | AWS KMS (AWS managed key or your customer managed key) | You need key policies, the option to disable the key, or a CloudTrail record of each use of the key. S3 Bucket Keys cut the KMS request cost. |
| DSSE-KMS | AWS KMS | Two layers of encryption |
| SSE-C | You, sent with each request | You must hold the keys yourself |

- An explicit deny in any policy wins ([policy evaluation logic](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html)).
- SCPs never grant permissions; they cap what IAM users and roles in member accounts can do. They don't affect the management account ([SCPs](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html)).
- AWS KMS rotates AWS managed keys every year, and you can't turn that off. For customer managed keys, rotation is optional: automatic (yearly by default, or a custom period) or on demand ([key rotation](https://docs.aws.amazon.com/kms/latest/developerguide/rotate-keys.html)).
- An ACM certificate used by CloudFront must be requested or imported in US East (N. Virginia), `us-east-1` ([CloudFront certificates](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/cnames-and-https-requirements.html)).
- Use origin access control (OAC) so that only CloudFront can read an S3 origin ([restrict S3 origins](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/private-content-restricting-access-to-s3.html)).

### Domain 2: resilience

Disaster recovery strategies, from Figure 6 of the [AWS disaster recovery whitepaper](https://docs.aws.amazon.com/whitepapers/latest/disaster-recovery-workloads-on-aws/disaster-recovery-options-in-the-cloud.html):

| Strategy | RPO and RTO | What runs in the recovery Region | Cost |
| --- | --- | --- | --- |
| Backup and restore | Hours | Nothing; you restore backups and provision resources after the event | Lowest |
| Pilot light | Tens of minutes | Data is live; services are idle until you provision and scale them | Low |
| Warm standby | Minutes | A smaller copy that is always running and scales up after the event | Higher |
| Multi-site active/active | Real time | Full production in every Region | Highest |

RDS and Aurora high availability ([Multi-AZ](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZSingleStandby.html), [read replicas](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ReadRepl.html), [Aurora replication](https://docs.aws.amazon.com/AmazonRDS/latest/AuroraUserGuide/Aurora.Replication.html)):

| | RDS Multi-AZ DB instance | RDS read replica | Aurora Replica |
| --- | --- | --- | --- |
| Replication | Synchronous, to a standby in another AZ | Asynchronous | Shared cluster storage |
| Serves reads | No, the standby can't take read traffic | Yes | Yes |
| Failover | Automatic; the DNS record moves to the standby | Manual promotion | Automatic; a reader becomes the writer |
| Main purpose | Availability | Read scaling, reporting, DR in another Region | Availability and read scaling, up to 15 replicas |

Amazon SQS ([queue types](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-queue-types.html), [visibility timeout](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-visibility-timeout.html), [long polling](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-short-and-long-polling.html)):

| Setting | Value |
| --- | --- |
| Standard queue | Delivers each message at least once; tries to keep order but doesn't guarantee it |
| FIFO queue | Keeps order and processes each message exactly once; 300 API calls per second per action, or 3,000 messages per second with batches of 10, and more in high throughput mode |
| Visibility timeout | 30 seconds by default, 12 hours at most |
| Message retention | 4 days by default, from 60 seconds to 14 days |
| Long polling wait | 20 seconds at most |

Load balancers ([ALB](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html), [NLB](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/introduction.html), [GWLB](https://docs.aws.amazon.com/elasticloadbalancing/latest/gateway/introduction.html)):

| Type | OSI layer | Pick it for |
| --- | --- | --- |
| Application Load Balancer | 7 | HTTP and HTTPS, with listener rules that route on the request, such as the URL path |
| Network Load Balancer | 4 | TCP, UDP, and TLS at very high request rates; a static IP per AZ, with an optional Elastic IP per subnet |
| Gateway Load Balancer | 3 | Inserting firewalls and other virtual appliances; uses GENEVE on port 6081 |

Route 53 routing policies: simple, failover, geolocation, geoproximity, latency, IP-based, multivalue answer, and weighted ([routing policies](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html)). Failover routing needs a health check on the primary record ([DNS failover](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-failover-configuring.html)).

### Domain 3: performance

Storage services ([EBS volumes](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-volumes.html), [EFS](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html), [FSx for Windows](https://docs.aws.amazon.com/fsx/latest/WindowsGuide/what-is.html), [FSx for Lustre](https://docs.aws.amazon.com/fsx/latest/LustreGuide/what-is.html)):

| Service | Type | Remember |
| --- | --- | --- |
| Amazon S3 | Object | Objects in buckets, read and written through the S3 API |
| Amazon EBS | Block | A volume and its instance must be in the same AZ |
| Amazon EFS | File (NFSv4) | Regional file systems store data across several AZs; One Zone keeps it in one |
| FSx for Windows File Server | File (SMB) | Windows workloads and Active Directory |
| FSx for Lustre | File | High performance computing and machine learning |

EBS volume types ([volume types](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-volume-types.html), [gp3](https://docs.aws.amazon.com/ebs/latest/userguide/general-purpose.html)):

| Type | Remember |
| --- | --- |
| gp3 | Baseline of 3,000 IOPS and 125 MiB/s at any size; provision more IOPS and throughput separately |
| io2 Block Express | Highest IOPS, average latency under 500 microseconds, supports Multi-Attach |
| st1 and sc1 (HDD) | Built for large streaming workloads where throughput matters most; sc1 is the cheaper, colder option; neither can be a boot volume |

Edge services ([CloudFront](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html), [Global Accelerator](https://docs.aws.amazon.com/global-accelerator/latest/dg/what-is-global-accelerator.html), [listeners](https://docs.aws.amazon.com/global-accelerator/latest/dg/about-listeners.html)):

| | CloudFront | Global Accelerator |
| --- | --- | --- |
| What it does | Caches and delivers web content at edge locations | Sends TCP and UDP traffic over the AWS global network to the best Regional endpoint, based on health and client location |
| Caching | Yes | No |
| Addresses | DNS name for the distribution | Two static anycast IPv4 addresses |

- DAX takes DynamoDB eventually consistent reads from milliseconds to microseconds with no change to the DynamoDB API ([DAX](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DAX.html)).
- Kinesis Data Streams keeps records for 24 hours by default and up to 365 days ([retention](https://docs.aws.amazon.com/streams/latest/dev/kinesis-extended-retention.html)).
- Amazon Data Firehose can convert JSON to Parquet or ORC before writing to S3, using a schema from the AWS Glue Data Catalog ([format conversion](https://docs.aws.amazon.com/firehose/latest/dev/record-format-conversion.html)).

### Domain 4: cost

S3 storage classes ([storage classes](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html), [archive retrieval](https://docs.aws.amazon.com/AmazonS3/latest/userguide/restoring-objects-retrieval-options.html)):

| Class | AZs | Minimum duration | Minimum billable size | Access |
| --- | --- | --- | --- | --- |
| S3 Standard | 3 or more | None | None | Milliseconds |
| S3 Intelligent-Tiering | 3 or more | None | None (objects under 128 KB aren't moved between tiers) | Milliseconds for the default tiers |
| S3 Standard-IA | 3 or more | 30 days | 128 KB | Milliseconds, retrieval fee |
| S3 One Zone-IA | 1 | 30 days | 128 KB | Milliseconds, retrieval fee; lost if the AZ is lost |
| S3 Glacier Instant Retrieval | 3 or more | 90 days | 128 KB | Milliseconds, retrieval fee |
| S3 Glacier Flexible Retrieval | 3 or more | 90 days | None (40 KB of metadata per object) | Restore: expedited 1–5 minutes, standard 3–5 hours, bulk 5–12 hours |
| S3 Glacier Deep Archive | 3 or more | 180 days | None (40 KB of metadata per object) | Restore: standard within 12 hours, bulk within 48 hours |

EC2 purchasing options ([purchasing options](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/instance-purchasing-options.html), [Savings Plans](https://docs.aws.amazon.com/savingsplans/latest/userguide/what-is-savings-plans.html), [Spot interruptions](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-instance-termination-notices.html)):

| Option | Commitment | Pick it for |
| --- | --- | --- |
| On-Demand | None | Short, spiky, or unpredictable workloads |
| Compute Savings Plans | An hourly amount of compute use for 1 or 3 years | Steady use that may change instance family, size, Region, or move to Fargate or Lambda |
| Reserved Instances | 1 or 3 years | Steady use of a known instance configuration |
| Spot Instances | None | Workloads that tolerate interruption; you get a notice two minutes before EC2 stops or terminates the instance |
| On-Demand Capacity Reservations | None | Guaranteed capacity in an AZ, billed at the On-Demand rate whether you use it or not |

- Gateway endpoints for S3 and DynamoDB cost nothing extra and take traffic off the NAT gateway ([gateway endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/gateway-endpoints.html)). Interface endpoints are billed per hour per AZ and per GB ([interface endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/privatelink-access-aws-services.html)).
- A NAT gateway is billed for each hour it exists and each GB it processes ([NAT pricing](https://docs.aws.amazon.com/vpc/latest/userguide/nat-gateway-pricing.html)). It lives in one AZ, so if instances in other AZs share it and that AZ goes down, they lose internet access ([NAT basics](https://docs.aws.amazon.com/vpc/latest/userguide/nat-gateway-basics.html)).
- Cost Explorer shows and forecasts spend, Budgets alerts you when spend or usage passes a threshold, and Cost and Usage Reports hold the most detailed line items ([Cost Explorer](https://docs.aws.amazon.com/cost-management/latest/userguide/ce-what-is.html), [Budgets](https://docs.aws.amazon.com/cost-management/latest/userguide/budgets-managing-costs.html), [CUR](https://docs.aws.amazon.com/cur/latest/userguide/what-is-cur.html)).

## Free official resources

- [Exam page](https://aws.amazon.com/certification/certified-solutions-architect-associate/): format, price, languages, and recertification.
- [SAA-C03 exam guide](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-associate-03/solutions-architect-associate-03.html): domains, task statements, and the list of services in scope.
- [Exam prep on AWS Skill Builder](https://skillbuilder.aws/category/exam-prep/solutions-architect-associate-SAA-C03): the exam prep plan in four steps. Items that need a paid subscription are marked Subscription on the page.
- [Official Practice Question Set](https://skillbuilder.aws/learn/6NV91XYP1P/official-practice-question-set-aws-certified-solutions-architect--associate-saac03--english/N1HSPV1K17): free, 20 questions written by AWS with feedback on every answer. You can retake it; the questions repeat in a different order.
- [Domain 1 Review](https://skillbuilder.aws/learn/FZFYKYYV19/domain-1-review-aws-certified-solutions-architect--associate-saac03--english/1SSMX53WP6) and the other domain review courses linked from the exam prep page: free videos by domain. The Official Practice Exam, which has the same length as the exam, needs a subscription.
- [AWS Free Tier](https://aws.amazon.com/free/): new accounts get $100 in credits, can earn up to $100 more, and stay on the Free plan for up to 6 months.
- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html) and the [disaster recovery whitepaper](https://docs.aws.amazon.com/whitepapers/latest/disaster-recovery-workloads-on-aws/disaster-recovery-options-in-the-cloud.html): the design principles the exam is built on.
- [AWS training events](https://aws.amazon.com/training/events/): free live webinars on the main exam topics, and [AWS Training Live on Twitch](https://www.twitch.tv/awstraininglive) for free live and recorded sessions. Both are linked from the exam page.

## Practice questions

[practice-questions.md](practice-questions.md) has 15 original questions in the exam's scenario style, spread across the four domains by weight, each with an explanation and a link to the AWS documentation. For more, the [Examice SAA-C03 practice bank](https://examice.com/exams/amazon/saa-c03?utm_source=github&utm_medium=social&utm_campaign=saa-c03-study-guide-and-practice-questions&utm_content=link_2) has 1,000+ questions, each with a written explanation and a discussion, and the first 5 are free with explanations.

## Exam day tips

- You have 130 minutes for 65 questions, so 2 minutes per question. Move on from a hard question and come back to it.
- Answer every question. Unanswered questions are scored as wrong and there is no penalty for guessing.
- 15 questions are unscored and not marked, so treat every question as if it counts.
- Find the constraint before you read the options: lowest cost, least operational effort, highest availability, or a stated RPO or RTO. Two options often work; the constraint decides which one is best.
- The exam uses short service names (for example Amazon SNS). The Help button in the exam lists the full names ([service mentions](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-associate-03/saa-service-mentions.html)).
- If English isn't your first language and you take the exam in English, request the ESL +30 accommodation once in your AWS Certification Account before you register ([accommodations](https://aws.amazon.com/certification/policies/before-testing/)).
- Most AWS exams don't show a pass or fail result on screen; results arrive within 5 business days ([FAQ](https://aws.amazon.com/certification/faqs/)).

## FAQ

### How hard is SAA-C03?

AWS recommends at least 1 year of experience designing cloud solutions on AWS. You need a scaled score of 720 out of 1,000, and the compensatory model lets a strong domain make up for a weaker one. AWS writes the wrong options to look plausible to someone with incomplete knowledge, so expect to choose between answers that all sound reasonable ([exam guide](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-associate-03/solutions-architect-associate-03.html)).

### How long should I study for SAA-C03?

AWS doesn't set a number. The Exam Prep Plan on Skill Builder lists 18 hours 25 minutes of content ([exam prep](https://skillbuilder.aws/category/exam-prep/solutions-architect-associate-SAA-C03)). The plan in this guide takes about 55 hours over six weeks; adjust it to your score on the free Official Practice Question Set.

### Does the AWS Certified Solutions Architect - Associate expire?

Yes. It is valid for 3 years. You can renew it for 3 more years by passing the latest version of the exam or by earning AWS Certified Solutions Architect - Professional, or extend it by 1 year through the maintain option on AWS Skill Builder, which needs a paid subscription ([recertification](https://aws.amazon.com/certification/recertification/)).

### What is the retake policy?

After a failed attempt you wait 14 calendar days. There is no limit on attempts, and each attempt costs the full fee. After you pass, you can't take the same exam again for two years unless AWS releases a new version with a new exam guide and exam series code ([after testing](https://aws.amazon.com/certification/policies/after-testing/)).

### Is SAA-C03 being replaced?

As of October 28, 2026, the exam page announces no new version and no retirement of SAA-C03. It announces only that the Italian version retires after December 31, 2026. AWS publishes exam guide revisions at least one month before the changes reach the exam ([exam page](https://aws.amazon.com/certification/certified-solutions-architect-associate/), [FAQ](https://aws.amazon.com/certification/faqs/)).

## Contributing

Found a mistake or a fact that has changed? Open an issue or a pull request, and link the AWS page that supports the correction.

## License

Text and questions © Examice, licensed under [CC BY 4.0](LICENSE).

AWS, Amazon Web Services, and the names of AWS services are trademarks of Amazon.com, Inc. or its affiliates. This guide is not affiliated with or endorsed by Amazon Web Services.
