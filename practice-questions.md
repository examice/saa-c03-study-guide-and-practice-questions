# SAA-C03 practice questions

These 15 questions follow the scenario style of the AWS Certified Solutions Architect - Associate exam and are spread across its four domains by weight: 4 on security, 4 on resilience, 4 on performance, and 3 on cost. Try each one before you open the answer. Every explanation says why the right option fits, why the others don't, and links to the AWS documentation behind it. The study guide is in [README.md](README.md).

> These are unofficial practice questions by Examice. Examice is not affiliated with or endorsed by Amazon Web Services. The questions are original and are not taken from the exam.

### Question 1

*Domain 1: Design Secure Architectures*

A company runs 40 member accounts in AWS Organizations. A new policy says that nobody in any member account, including account administrators, may create resources outside two approved AWS Regions. The control must prevent the action, not just report it. What should a solutions architect do?

- A. Attach an IAM policy that denies other Regions to every IAM user and role in each account.
- B. Create a service control policy that denies requests when `aws:RequestedRegion` is not one of the two approved Regions, with exceptions for global services, and attach it to the organizational units that hold the member accounts.
- C. Deploy AWS Config rules in every account that flag resources created in other Regions.
- D. Attach a permissions boundary to the root user of each member account.

<details><summary>Show answer</summary>

**B.** A service control policy sets the most that any IAM user or role in a member account can do, and it is managed from the organization, not from inside the member accounts. A deny on `aws:RequestedRegion` blocks calls to Regions outside the list. Global services such as IAM have a single endpoint in `us-east-1`, so a Region SCP has to exempt them or it blocks them too.

- A is wrong because an administrator in the account can edit or detach those policies, and every new user or role would need the policy too.
- C is wrong because AWS Config detects and reports noncompliant resources after they exist; it doesn't stop the request.
- D is wrong because permissions boundaries apply to IAM users and roles, not to the root user.

Docs: [Service control policies](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_policies_scps.html), [aws:RequestedRegion](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-requestedregion), [Permissions boundaries](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_boundaries.html)

</details>

### Question 2

*Domain 1: Design Secure Architectures*

A fleet of EC2 instances in a public subnet accepts TCP connections directly from the internet. Logs show a flood of malicious connections from the range `203.0.113.0/24`. The team must block that range for every instance in the subnet right away while all other traffic keeps flowing. What should they do?

- A. Add a rule to the instances' security group that denies `203.0.113.0/24`.
- B. Add an inbound deny rule for `203.0.113.0/24` to the subnet's network ACL, with a lower rule number than the rule that allows the traffic.
- C. Remove the route to the internet gateway from the subnet's route table.
- D. Enable Amazon GuardDuty in the account.

<details><summary>Show answer</summary>

**B.** Network ACLs support deny rules and apply to every instance in the subnet. They evaluate rules in ascending order and stop at the first match, so the deny rule must have a lower number than the allow rule.

- A is wrong because security groups only support allow rules.
- C is wrong because it cuts off all internet traffic, not just the malicious range.
- D is wrong because GuardDuty detects threats and produces findings; it doesn't block traffic by itself.

Docs: [Compare security groups and network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/infrastructure-security.html#VPC_Security_Comparison), [Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html)

</details>

### Question 3

*Domain 1: Design Secure Architectures*

An application on Amazon ECS reads the password for an Amazon RDS for MySQL database from a configuration file in its container image. Security wants the password out of the image and rotated every 30 days, with as little custom code as possible. What should the solutions architect recommend?

- A. Store the credentials in AWS Secrets Manager, turn on automatic rotation every 30 days, and let the ECS task role read the secret at runtime.
- B. Store the credentials in an S3 bucket encrypted with SSE-KMS and write a scheduled job that changes the password and updates the file.
- C. Pass the password as a plain environment variable in the task definition and change it by hand each month.
- D. Encrypt the password with AWS KMS and keep the ciphertext in the application code.

<details><summary>Show answer</summary>

**A.** Secrets Manager stores the secret outside the image, controls access with IAM, and rotates it on a schedule. Rotation updates both the secret and the database, through managed rotation or a rotation Lambda function, depending on the type of secret.

- B is wrong because it works only with a custom rotation job that you write and maintain.
- C is wrong because the password stays visible in the task definition and rotation is manual.
- D is wrong because the secret still lives in the code and nothing rotates it.

Docs: [Rotate Secrets Manager secrets](https://docs.aws.amazon.com/secretsmanager/latest/userguide/rotating-secrets.html), [Get secrets from Secrets Manager](https://docs.aws.amazon.com/secretsmanager/latest/userguide/retrieving-secrets.html)

</details>

### Question 4

*Domain 1: Design Secure Architectures*

A healthcare company stores reports in Amazon S3. Its compliance team needs to control who can use the encryption key through a key policy, be able to disable the key at once in an incident, and see an audit record every time the key is used. Which encryption option meets all three requirements?

- A. Server-side encryption with Amazon S3 managed keys (SSE-S3)
- B. Server-side encryption with an AWS KMS customer managed key (SSE-KMS)
- C. Server-side encryption with the AWS managed key `aws/s3` (SSE-KMS)
- D. Server-side encryption with customer-provided keys (SSE-C)

<details><summary>Show answer</summary>

**B.** With a customer managed key you write the key policy and can disable the key, and AWS CloudTrail logs each use of the key, such as `GenerateDataKey` and `Decrypt`.

- A is wrong because Amazon S3 manages SSE-S3 keys for you; there is no key policy for you to edit and no key for you to disable.
- C is wrong because you can't manage the lifecycle or permissions of an AWS managed key, so you can't change its key policy or disable it.
- D is wrong because you supply and protect the key yourself outside AWS KMS, so there is no KMS key policy or KMS audit trail.

Docs: [SSE-KMS](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingKMSEncryption.html), [AWS KMS keys](https://docs.aws.amazon.com/kms/latest/developerguide/concepts.html), [Enable and disable keys](https://docs.aws.amazon.com/kms/latest/developerguide/enabling-keys.html)

</details>

### Question 5

*Domain 2: Design Resilient Architectures*

An ordering application has a web tier on EC2 that calls the order processing servers directly and waits for a reply. During sales the processing servers can't keep up, requests time out, and orders are lost. The company wants no lost orders and wants each tier to scale on its own. What should the solutions architect do?

- A. Move the processing servers to larger instance types.
- B. Have the web tier send each order to an Amazon SQS queue, have processing workers in an Auto Scaling group poll the queue, and scale the workers on the queue backlog.
- C. Publish each order to an Amazon SNS topic with the processing servers subscribed through HTTP endpoints.
- D. Put an Application Load Balancer in front of the processing servers.

<details><summary>Show answer</summary>

**B.** The queue stores orders until a worker processes and deletes them, so spikes wait in the queue instead of failing. The workers pull at their own pace and scale on the backlog, independent of the web tier.

- A is wrong because a bigger server only raises the ceiling; the tiers stay tightly coupled and a larger spike still fails.
- C is wrong because SNS pushes each message to subscribers as it arrives, so an overloaded subscriber still gets work faster than it can handle it.
- D is wrong because the calls stay synchronous; a load balancer spreads load but doesn't hold work for later.

Docs: [What is Amazon SQS?](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html), [Scaling policy based on Amazon SQS](https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-using-sqs-queue.html)

</details>

### Question 6

*Domain 2: Design Resilient Architectures*

An internal reporting application runs in one Region. If the Region becomes unavailable, the business accepts up to 24 hours to recover and up to one day of lost data. The company wants the lowest cost disaster recovery approach that meets this. What should the solutions architect choose?

- A. Multi-site active/active across two Regions
- B. Warm standby in a second Region
- C. Daily backups with AWS Backup, copied to a second Region, plus infrastructure as code to rebuild the stack there when needed
- D. Pilot light with continuous database replication to a second Region

<details><summary>Show answer</summary>

**C.** Backup and restore has the lowest cost of the AWS disaster recovery strategies and targets an RPO and RTO measured in hours. Copying backups to another Region and rebuilding with infrastructure as code covers the loss of a Region.

- A is wrong because it runs full production in two Regions, the most expensive option, for a near zero RPO and RTO the business doesn't need.
- B is wrong because it keeps a scaled down copy running all the time, which costs more than the requirement justifies.
- D is wrong because continuous replication and live data in the second Region cost more than daily backups and target recovery in tens of minutes.

Docs: [Disaster recovery options in the cloud](https://docs.aws.amazon.com/whitepapers/latest/disaster-recovery-workloads-on-aws/disaster-recovery-options-in-the-cloud.html), [AWS Backup copies across Regions](https://docs.aws.amazon.com/aws-backup/latest/devguide/cross-region-backup.html)

</details>

### Question 7

*Domain 2: Design Resilient Architectures*

A production PostgreSQL database runs on a single Amazon RDS DB instance. The company needs automatic failover to another Availability Zone with no data loss for committed transactions and no change to the connection endpoint in the application. What should the solutions architect do?

- A. Create a read replica in another Availability Zone and promote it if the primary fails.
- B. Convert the DB instance to a Multi-AZ DB instance deployment.
- C. Take automated snapshots every hour and restore the latest one in another Availability Zone after a failure.
- D. Put Amazon RDS Proxy in front of the DB instance.

<details><summary>Show answer</summary>

**B.** A Multi-AZ DB instance deployment keeps a standby in another Availability Zone with synchronous replication. On failure, RDS switches to the standby automatically and moves the instance's DNS record to it, so the application keeps the same endpoint.

- A is wrong because read replicas use asynchronous replication, so recent transactions can be lost. Promotion is also a manual step, and the application would have to switch to the replica's own endpoint.
- C is wrong because restoring a snapshot loses up to an hour of data and takes manual work.
- D is wrong because RDS Proxy pools and manages connections; it doesn't create a standby database.

Docs: [Multi-AZ DB instance deployments](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZSingleStandby.html), [Failing over a Multi-AZ DB instance](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.Failover.html), [Read replicas](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ReadRepl.html)

</details>

### Question 8

*Domain 2: Design Resilient Architectures*

A web application runs behind an Application Load Balancer in `us-east-1`, with a standby copy behind another load balancer in `us-west-2`. All users should reach `us-east-1` while it is healthy, and DNS should send them to `us-west-2` automatically if it fails. Which Amazon Route 53 configuration meets this?

- A. Weighted routing with 50% of traffic to each Region
- B. Failover routing, with a health check on the primary record for `us-east-1` and a secondary record for `us-west-2`
- C. Latency routing with records for both Regions
- D. Geolocation routing that maps each continent to one Region

<details><summary>Show answer</summary>

**B.** Failover routing answers with the primary record while its health check passes and switches to the secondary record when it fails, which is exactly an active/passive setup.

- A is wrong because it sends half of the traffic to the standby all the time.
- C is wrong because it sends each user to the Region with the lowest latency, so both Regions serve traffic.
- D is wrong because it routes by user location, not by the health of the primary.

Docs: [Failover routing](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy-failover.html), [Configuring DNS failover](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/dns-failover-configuring.html)

</details>

### Question 9

*Domain 3: Design High-Performing Architectures*

A genomics lab runs analysis jobs on Linux EC2 instances in an Auto Scaling group that spans three Availability Zones. Every job reads the same input files and writes its results next to them, at the same time as the other jobs, through a standard POSIX file system. The data set grows by several terabytes a month, and nobody has time to resize volumes. Which storage should the solutions architect use?

- A. One Amazon EBS gp3 volume attached to all instances
- B. An Amazon EFS file system with Regional availability, mounted on every instance
- C. Instance store volumes on each instance
- D. An Amazon FSx for Windows File Server file system

<details><summary>Show answer</summary>

**B.** Amazon EFS is a managed NFS file system that many instances can mount at once, stores data across several Availability Zones in Regional mode, and grows and shrinks as you add and remove files.

- A is wrong because an EBS volume must be in the same Availability Zone as its instance, and gp3 doesn't support Multi-Attach.
- C is wrong because instance store is temporary storage local to one instance, so nothing is shared.
- D is wrong because FSx for Windows File Server is built for Windows workloads over SMB; a Linux application that expects NFS fits EFS.

Docs: [What is Amazon EFS?](https://docs.aws.amazon.com/efs/latest/ug/whatisefs.html), [Amazon EBS volumes](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-volumes.html), [EBS volume types](https://docs.aws.amazon.com/ebs/latest/userguide/ebs-volume-types.html)

</details>

### Question 10

*Domain 3: Design High-Performing Architectures*

A gaming leaderboard stores scores in Amazon DynamoDB. Reads outnumber writes 50 to 1, the same items are read again and again, and the product team wants read latency in microseconds. Eventually consistent reads are fine, and the developers want to change as little code as possible. What should the solutions architect add?

- A. A DynamoDB Accelerator (DAX) cluster, with the application using the DAX client
- B. More provisioned read capacity units on the table
- C. An Amazon ElastiCache cluster with custom code to read from and write to the cache
- D. A DynamoDB global table in a second Region

<details><summary>Show answer</summary>

**A.** DAX is a cache for DynamoDB that holds data in memory and brings eventually consistent reads from milliseconds down to microseconds. It is compatible with the DynamoDB API, so the application changes very little.

- B is wrong because more capacity raises throughput but doesn't bring latency below DynamoDB's normal millisecond range.
- C is wrong because it can be fast, but it needs custom caching logic in the application, which the requirement wants to avoid.
- D is wrong because global tables replicate data across Regions; they don't add a cache.

Docs: [In-memory acceleration with DAX](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/DAX.html)

</details>

### Question 11

*Domain 3: Design High-Performing Architectures*

A multiplayer game uses a custom protocol over UDP. Game servers run behind Network Load Balancers in two Regions. Players around the world report lag, and some corporate networks must allow a fixed, short list of IP addresses. The company also wants traffic to move to the other Region if one fails. What should the solutions architect use?

- A. An Amazon CloudFront distribution in front of both Network Load Balancers
- B. AWS Global Accelerator with an endpoint group in each Region that points to the Network Load Balancer
- C. Amazon Route 53 latency routing between the two Network Load Balancers
- D. Elastic IP addresses on every Network Load Balancer subnet, shared with all players

<details><summary>Show answer</summary>

**B.** Global Accelerator gives you two static anycast IPv4 addresses, supports UDP listeners, carries traffic over the AWS global network, and routes to the best healthy Regional endpoint, so a failed Region stops receiving players.

- A is wrong because CloudFront delivers and caches web content over HTTP and HTTPS; it doesn't carry a custom UDP game protocol.
- C is wrong because DNS routing gives no fixed IP list, and clients that cache DNS answers keep reaching a failed endpoint until the record expires.
- D is wrong because it gives players a long list of Regional addresses with no automatic failover and no path over the AWS global network.

Docs: [What is AWS Global Accelerator?](https://docs.aws.amazon.com/global-accelerator/latest/dg/what-is-global-accelerator.html), [Listeners](https://docs.aws.amazon.com/global-accelerator/latest/dg/about-listeners.html)

</details>

### Question 12

*Domain 3: Design High-Performing Architectures*

A website sends clickstream events as JSON. Analysts query the data in Amazon Athena and want it in Amazon S3 as Apache Parquet within a few minutes of each event. The team wants a managed service and no consumer code to run. What should the solutions architect use?

- A. Kinesis Data Streams with a fleet of EC2 consumers that convert records to Parquet
- B. Amazon Data Firehose with record format conversion to Parquet, using a schema in the AWS Glue Data Catalog, delivering to S3
- C. An Amazon SQS queue with a Lambda function that writes each event to S3 as a separate JSON file
- D. Write raw JSON to S3 and run a nightly AWS Glue job to convert it to Parquet

<details><summary>Show answer</summary>

**B.** Firehose is a managed delivery stream that can convert JSON records to Parquet or ORC before writing them to S3. It reads the schema from the Glue Data Catalog, and you run no consumers.

- A is wrong because it works but you build, run, and scale the consumer fleet yourself.
- C is wrong because the data stays as many small JSON files, not Parquet.
- D is wrong because a nightly job doesn't meet the few minutes requirement.

Docs: [Convert input data format in Amazon Data Firehose](https://docs.aws.amazon.com/firehose/latest/dev/record-format-conversion.html)

</details>

### Question 13

*Domain 4: Design Cost-Optimized Architectures*

A company stores compliance logs in Amazon S3. Teams read the logs often during the first 30 days, and after that almost never. The logs must be kept for 7 years, and when auditors ask for old logs, waiting up to 48 hours is acceptable. Which approach costs the least?

- A. Keep all logs in S3 Standard for 7 years.
- B. Add a lifecycle rule that moves objects to S3 Glacier Deep Archive after 30 days and expires them after 7 years.
- C. Add a lifecycle rule that moves objects to S3 One Zone-IA after 30 days.
- D. Add a lifecycle rule that moves objects to S3 Glacier Instant Retrieval after 30 days.

<details><summary>Show answer</summary>

**B.** S3 Glacier Deep Archive is the lowest cost storage option in AWS. Standard restores finish within 12 hours and bulk restores within 48 hours, which fits the audit requirement, and 7 years is far beyond its minimum storage duration of 180 days.

- A is wrong because S3 Standard is priced for frequent access, which these logs don't need after 30 days.
- C is wrong because it costs more than an archive class for data that is almost never read, and keeping compliance records in a single Availability Zone risks losing them.
- D is wrong because it pays for millisecond access that nobody needs here, at a higher storage price than Deep Archive.

Docs: [S3 storage classes](https://docs.aws.amazon.com/AmazonS3/latest/userguide/storage-class-intro.html), [S3 Glacier storage classes](https://docs.aws.amazon.com/AmazonS3/latest/userguide/glacier-storage-classes.html), [Archive retrieval options](https://docs.aws.amazon.com/AmazonS3/latest/userguide/restoring-objects-retrieval-options.html), [Lifecycle](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html)

</details>

### Question 14

*Domain 4: Design Cost-Optimized Architectures*

A studio renders video frames every night on 50 On-Demand EC2 instances. The jobs save checkpoints and can restart if an instance stops, and they only need to finish within 8 hours. The instances sit idle during the day. What change reduces compute cost the most?

- A. Run the jobs on Spot Instances, spread across several instance types
- B. Commit to Reserved Instances for the 50 instances for three years
- C. Move the jobs to Dedicated Hosts
- D. Create On-Demand Capacity Reservations for the 50 instances

<details><summary>Show answer</summary>

**A.** Spot Instances use spare EC2 capacity at a steep discount and suit batch jobs that can be interrupted. EC2 sends an interruption notice two minutes before it stops or terminates a Spot Instance, and checkpoints let the job resume elsewhere. Using several instance types makes spare capacity easier to find.

- B is wrong because Reserved Instances are billed for the whole term, and these instances sit idle for most of each day.
- C is wrong because Dedicated Hosts give you a whole physical server, mainly so you can bring licenses bound to sockets or cores; they don't make interruptible batch work cheaper.
- D is wrong because Capacity Reservations are billed at the On-Demand rate whether you use them or not, so they add no discount.

Docs: [Spot Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-instances.html), [Spot interruption notices](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-instance-termination-notices.html), [Capacity Reservation pricing](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/capacity-reservations-pricing-billing.html)

</details>

### Question 15

*Domain 4: Design Cost-Optimized Architectures*

EC2 instances in private subnets upload several terabytes a month to an S3 bucket in the same Region through a NAT gateway, and the NAT gateway's data processing charges are high. The instances must stay unreachable from the internet. Which change cuts the cost with the least operational effort?

- A. Create a gateway VPC endpoint for Amazon S3 and add it to the private subnets' route tables.
- B. Replace the NAT gateway with a NAT instance that the team manages.
- C. Create an interface VPC endpoint for Amazon S3 in each Availability Zone.
- D. Move the instances to public subnets and give them public IP addresses.

<details><summary>Show answer</summary>

**A.** A gateway endpoint gives private subnets a route to S3 without a NAT device, and there is no additional charge for using it. S3 traffic stops passing through the NAT gateway, so it stops adding to the processing charge for each GB.

- B is wrong because the team takes on patching, scaling, and availability of the NAT instance, and the traffic still flows through it.
- C is wrong because interface endpoints work but are billed per hour in each Availability Zone and per GB processed, which costs more than a gateway endpoint for this traffic.
- D is wrong because it exposes the instances to the internet.

Docs: [Gateway endpoints](https://docs.aws.amazon.com/vpc/latest/privatelink/gateway-endpoints.html), [NAT gateway pricing](https://docs.aws.amazon.com/vpc/latest/userguide/nat-gateway-pricing.html), [Interface endpoints pricing](https://docs.aws.amazon.com/vpc/latest/privatelink/privatelink-access-aws-services.html)

</details>

## More practice

The [Examice SAA-C03 practice bank](https://examice.com/exams/amazon/saa-c03?utm_source=github&utm_medium=social&utm_campaign=saa-c03-study-guide-and-practice-questions&utm_content=link_3) has 1,000+ questions with a written explanation and a discussion under each one. The first 5 are free, explanations included.
