# CLOUD COMPUTING AND AWS

## What is Cloud Computing

- Cloud computing is the on-demand delivery of IT resources over the Internet with pay-as-you-go pricing. Instead of buying, owning, and maintaining physical data centers and servers, you can access technology services, such as computing power, storage, and databases, on an as-needed basis from a cloud provider like Amazon Web Services (AWS).

### What are 3 benefits of Cloud Computing

- Agility: The cloud gives you easy access to a broad range of technologies so that you can innovate faster and build nearly anything that you can imagine. You can quickly spin up resources as you need them–from infrastructure services, such as compute, storage, and databases, to Internet of Things, machine learning, data lakes and analytics, and much more. You can also deploy technology services in a matter of minutes, and get from idea to implementation several orders of magnitude faster than before. This gives you the freedom to experiment, test new ideas to differentiate customer experiences, and transform your business.

- Elasticity: With cloud computing, you don’t have to over-provision resources up front to handle peak levels of business activity in the future. Instead, you provision the amount of resources that you actually need. You can scale these resources up or down to instantly grow and shrink capacity as your business needs change.

- Cost savings: The cloud allows you to trade capital expenses (such as data centers and physical servers) for variable expenses, and only pay for IT as you consume it. Plus, the variable expenses are much lower than what you would pay to do it yourself because of the economies of scale. 


## What is AWS
- Amazon Web Services (AWS) is the world’s most comprehensive and broadly adopted cloud platform, offering over 200 fully featured services from data centers globally. Millions of customers—including the fastest-growing startups, largest enterprises, and leading government agencies—are using AWS to lower costs, become more agile, and innovate faster.

### 3 Advantages of AWS
- Largest community of customers and partners: AWS has the largest and most dynamic community, with millions of active customers and tens of thousands of partners globally. Customers across virtually every industry and of every size, including startups, enterprises, and public sector organizations, are running every imaginable use case on AWS. The AWS Partner Network (APN) includes thousands of systems integrators who specialize in AWS services and tens of thousands of independent software vendors (ISVs) who adapt their technology to work on AWS. 

- Most secure: AWS is architected to be the most flexible and secure cloud computing environment available today. Our core infrastructure is built to satisfy the security requirements for the military, global banks, and other high-sensitivity organizations. This is backed by a deep set of cloud security tools, with 230 security, compliance, and governance services and features. AWS supports 90 security standards and compliance certifications, and all 117 AWS services that store customer data offer the ability to encrypt that data.

- Fastest pace of innovation: With AWS, you can leverage the latest technologies to experiment and innovate more quickly. We are continually accelerating our pace of innovation to invent entirely new technologies you can use to transform your business. For example, in 2014, AWS pioneered the serverless computing space with the launch of AWS Lambda, which lets developers run their code without provisioning or managing servers. And AWS built Amazon SageMaker, a fully managed machine learning service that empowers everyday developers and scientists to use machine learning–without any previous experience.

### Who is using AWS and Cloud Computing 

- BBC
- Netflix : They use auto-scaling, pre determine and estimate how many people are watching. 
- Twitch 
- Linkedin 
- Facebook 

### Scaling

- Scaling up is when we get bigger servers
- Scaling out is adding new servers 
- Scaling down is when we scale down to be cost effective 

### What is Hybrid Cloud 

- Hybrid cloud is IT infrastructure that connects at least one public cloud and at least one private cloud
##### Diagram of Cloud
![cloud_diagram](https://github.com/ArunPanesar42/cloud_computing_AWS/blob/main/types_of_cloud.png?raw=true)

### AWS DIAGRAM

![aws_diagram](https://github.com/ArunPanesar42/cloud_computing_AWS/blob/main/diagram.JPG?raw=true)

## Architectures 
### Monolithic Architecture 
When all of the servers/instances are run from a single machine. Components of the program are interconnected and dependent on each other. If an update is made, the whole application has to re-run. Despite this, it's simpler to test than modular approaches (microservices), due to having fewer components as well as being simple to deploy. For example, running all vagrant virtual machines from a single Vagrantfile.

### Two Tier Architecture
When the presentation layer (interface) runs on a client and a data layer/structure (database) gets stored on a server. Basically, when each instance is run on a separate machine. It separates these two components into different locations. Having separate layers can improve performance and scalability.

## EC2
Elastic Compute Cloud provides scalable computing capacity in the AWS cloud. Effectively running virtual computing environments (instances) on the cloud. Some benefits:
- Scalable - can scale up or down based on changes in requirements
- No need for hardware up front - can develop and deploy applications faster
- Secure - has security configurations with security groups
#### extra info
- For EC2 we need to follow this naming convention, Key is the Name "Eng84_Apanesar_app"

## Virtual Private Cloud (VPC)
- VPC is a service that lets you launch AWS resources in a logically isolated (secure) virtual network that you define. Services, such as EC2 instances, can be launched into the VPC.
- It allows EC2 instances to communicate with eachother, we can also create multiple subnets.
### Benefits OF VPC
- *Secure and monitored network connections* - inbound and outbound filtering can be performed at the instance and subnet level.
- *Customisable virtual network* - IP address ranges, subnets, and route tables can be configured to any available gateways.
## Subnets
A subnet is a network inside a network. They make networks more efficient as network traffic can travel a shorter distance without passing through unnecessary routers to reach its destination. E.g. a subnet for teachers and another one for students.
- Public subnets have their traffic routed to an internet gateway.
- Private subnets are not routed to an internet gateway, but its traffic is routed to a virtual private gateway for a Site-to-Site VPN connection (known as VPN-only subnet). Limits access from the internet.
## Internet Gateway 
An internet gateway is a horizontally scaled and highly available VPC component that allows communication between your VPC (and its components) and the internet.
## Route Table
A route table contains a set of rules, called routes, that are used to determine where network traffic from your subnet or gateway is directed.
## Security Group
Security group acts as a firewall that controls the traffic for your instance (server) on the machine. Other points:
- As mentioned, it operates at an instance level
- It's stateful - returning traffic is automatically allowed, regardless of the rules
- All rules are evaluated before deciding to allow traffic
- Supports allow rules only
## Network Access Control List (NACL)

A network access control list is an additional layer of security for your VPC that acts as a firewall for controlling traffic in and out of one or more subnets. Other points:
- It operates at a subnet level - the rules apply to all the instances in the subnet (in addition to the SG rules applied to those instances).
- It's stateless - returning traffic must follow the rules
- Rules are processed in order, starting with the lowest numbered rule, when deciding to allow traffic
- Supports both allow and deny rules
## Ephemeral/Dynamic Ports

- Shortly lived ports, 'lives' for the duration of their use
- Automatically allocated based on the demand
- Range from 1024-65535

## Extra Commands
### Dos2Unix
- We use this to covert the `provisions.sh` to executable code in  
- the command to download ``dos2unix wget "http://ftp.de.debian.org/debian/pool/main/d/dos2unix/dos2unix_6.0.4-1_amd64.deb"``
- Then Followed by ``sudo dpkg -i dos2unix_6.0.4-1_amd64.deb``
### Copy Permissions
- We use this to copy files from local to ``scp -i ~/.ssh/DevOpsStudent.pem -r app/ ubuntu@'AddIP'':~/app/``







