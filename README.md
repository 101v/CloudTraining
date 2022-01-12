Cloud Training
================
Date 13 Dec 2021
----------------
	(1) Latency
	(2) Disaster Recovery
	(3) Region
	(4) Availibility Zone

	
	Amazon EC2 (EC2 = Elastic Compute Cloud)
	- Instance type -> Defines Memory(M) and CPU(C) -> t2 - test instance, m - moderate balance ram and cpu, c - more cpu, r - more ram/memory, 
	- EBS (Elastic Block Storage gives hard disk)
		- HDD types: Megnatic, General Purpose SSD (GP2, GP3), Provisioned IOPS SSD (IO2, IO3), Throughput optimixed HDD, Cold HDD
		IOPS = IO per Second
		
Date 15 Dec 2021
----------------
	OS - AMI (Amazon machine image)
		- type 1 Community  2 AWS marketplace 3 MyAMI
	Instance Purchasing Options
		- 1 On demand 2 Reserved 3 Spot
		
Date 17 Dec 2021
--------------------
	- With image wee can assign tags
	- Need to specify ports and protocols when creating images. ex. for windows RDP, for linux SSH 22, HTTP 80 etc
	
Date 20 Dec 2021
---------------------
	- Volume and snapshot
		- volume - disk space
			- to increase - create volume
			- attach volume
			- create D drive
			- Write some data
			- create snapshot
	- We can create volume and then we can attach to the instance/machine. We can create new partitions this way
	- Better to keep only one partition on one volume
	- We can modify volume but I think we can not reduce its size 
	- Snapshot is the snapshot of volume or snapshot of image (volume snapshot is used to create new volume, while image snapshot is used to create custom made images (AMI) which can be used to create machine instances)
	- Volume and machine both should be in same data center
	- We can create new volume from snapshot. We can copy snapshots in different regions too. 
	
22 Dec 2021
--------------
	- Load Balancer - Classic
	- Home work: Application Load Balancer. Prepare document
		
24 Dec 2021
------------
	- Auto scaling
		- Scale up, down on the basis of condition like CPU usage
		- Min, Max, Desire
		- For new instances Launch Configuration is required for specifying AMI, Security Group, Instance type etc
		- Create Alarams for Matrix like CPU usage in Cloud watch
		- Then in Scaling group define how many instances to be added or removed on particular alaram
		
27 Dec 2021
-------------
	- S3 service
	------------
		- object based storage
		- unlimited storage
		- charge per GB
		- 3 copies of data
		- Bucket
		- max limit per object 5TB
		- Types: standard, Infrequent Access, Reduced Redundancy, single zone infrequent, Intelligent tearing, Glacier, Glacier Deep Archieve
		- Life cycle policy
		- Versioning the data
		
29 Dec 2021
-------------
	- S3 service
	-------------
		- cross region replication of data (data replication in different buckets of different regions)
		
	- Cloud Trail
	--------------
		- we can store audit trail of cloud actions in S3 buckets
		
	- S3 can store Server access logging too
	
31 Dec 2021
---------------
	- Route 53 aka DNS
	-------------------
	- 53 is default port
	- DNS: Record Type A => Name to IP record 
	Record Type: CName => Name to Name record
	Record Type: Mx Record (Mail Exchange Record)
	Record Type: NS Record (Name Server Record) - tells where the dns mappings are stored. (multiple NS for master and backup servers)
	
	Hosted Zone is used to use already bought domain somewhere else for ex. godaddy
	Routing Policies: Simple Routing Policy, Latency, Geolocation, Weighted, Failover
	
3 Jan 2021
------------------
	- CName record => Name to Name
	
	IAM
	------
	- Identity and Access Manager - authentication and authorization
	- In AWS there is root account and user account. - Root account is Administrator
	- Two types of access typse in IAM user creation - (1) console (gui, browser) (2) Programatic (CLI, API)
	Console - need to set password
	CLI - Access Key, Secret Access Key
	- Then need to attach policy with user to give permission
	
5 Jan 2021
-------------------
	- User Group - we can give group level permissions and then add users in group, so users will get those permissions
	- MFA (Multi factor Authentication) for Root user
	- For non root users too, MFA can be used
	
	AWS CLI
	---------
	- command line for AWS operations where we can do activities using commands instead of GUI
	- Need to install aws cli package
	- aws configure is the command to store credential secret access key on the local machine
	- aws help can be used to get help for commands
	- give piped output of command to less or grep to show data page by page or to search something in command output
	
	some examples
	-------------
	aws configure
	aws ec2 help | less
	aws ec2 help | grep instance
	
	(1) Create instance
	aws ec2 run-instances --image-id ami-0851b76e8b1bce90b --count 1 --instance-type t2.micro --key-name vishalimorphr --security-group-ids sg-00225d371c854c9c1

	(2) List AMI created by me only
	aws ec2 describe-images --owners self
	aws ec2 describe-images --owners 696612176554

	(3) Create S3 bucket
	aws s3 mb s3://fromclisisvis --region ap-south-1

	(4) Upload and download file in s3 bucket
	-upload
	aws s3 cp testupload.txt s3://fromclisisvis/testupload.txt
	
	-download
	 aws s3 cp s3://fromclisisvis/testupload.txt testdownload.txt
	 
	(5) upload and download folder in s3 bucket
	-upload
	 aws s3 cp dira s3://fromclisisvis/dira/ --recursive

	-download
	aws s3 cp s3://fromclisisvis/dira/ dirdownload --recursive

7 Jan 2021
------------
	aws exams
	----------
		basic exam - cloud practicioner
		Next level associate - Solution Architect, Developer, Sysops
		Next level - solution architect, devops
		there are special exams too wx. database related, alexa etc.
		
	Preparation of exam
	-------------------
		- first list the topics from hardest to easiest
		- start refering according to that list - refere video training, do practical
		- service wise question paper and dummy exams like international exam
		- Read FAQ from AWS site. search like AWS FAQ for EC2 etc.
		
	without storing aws access keys on server do programatic access of aws cloud (topic Roles)
	--------------------------------------------------------------------------------------------
		- Difference between group and role
		- using role we can give one aws service the access of another service
			ex. we can set role with permission where ec2 service can get full access of s3 and we assign that role to ec2 instnce, then from that instance we
			can access s3 cli commands without giving access key and secret access key (without doing aws configure)
		- metadata command can be run on ec2 instance command prompt to get meta data of that machine
			ex. on linux terminal use - curl http://169.254.169.254/latest/meta-data/iam/security-credentials/s3-access-role
		- Sequence for checking permission : Default deny -> deny -> allow -> deny
		- policy name: arn: aws: service: region: accountid: resource
					ex.arn: aws: ec2: eu-west-2: <accountid>: instance/*
					   arn: aws: ec2: : : bucketname   (here region not required as s3 is global and account id not required as s3 bucket names are unique)
					   
		- full form of arn -> amazon resource name			
		- IAM and S3 services are global not region specific
		
10 Jan 2021
------------
	- IAM - AWS has given predefined policies. But we can create custom policies too. 
	- By creating custom policies we can achieve scenarios like following
		- User A should be allowed to create/manage instances in Mumbai region only. In any other region he should not be allowed.
		- User B can only start/stop instances but he should not be allowed to create or terminate instances
		- User C can only access specific S3 bucket not any other bucket except it
		- User D can create object in S3 bucket but he can not delete it
