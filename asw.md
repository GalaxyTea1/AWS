# AWS
### **What is cloud computing**
The practice of using a network of remote servers hosted on the Internet to store, manage, and process data, rather than a local server or a personal computer.

### **Cloud Providers**
- Someone else owns the servers
- Someone else hires the IT people
- Someone else pays or rent the real-estate
- You are responsible for your configuring cloud services and code, someone else takes care of the rest

### **AWS Global Infrastructure**
- AWS Regions
- AWS Availability Zones
- AWS Data Centers
- AWS Edge Locations / Points of Presence
> https://aws.amazon.com/about-aws/global-infrastructure/

### **AWS Regions**
- AWS has Regions all around the word
- Names can be us-east-l, eu-west-3,...
- A region is a cluster of data centers
- Most AWS services are region-scoped

### **Choose AWS Region**
- Compliance
- Proximity
- Available services
- Pricing
Choose a region nearest customer

### **Zones**
- Usually 3, min is 2, max is 6
- Singapore (ap-southeast-1)

### **IAM**
- Link user: https://trong-aws.signin.aws.amazon.com/console
- IAM = Identity and Access Management, Global service
- Users & Groups
- Permissions
- Policies
    - Consists of:
        - Version: policy language version, always include, ex:"2022-09-29"
        - Id: an identifier for the policy (optional)
        - Statement: one or more individual statements (required)
    - Statements consists of:
        - Sid: an identifier for the statements (optional)
        - Effect: whatever the statements allows or denies access (Allow, Deny)
        - Principal: account/user/role to which this policy applied to
        - Action: list of actions this policy allows or denies
        - Resource: list of resource to which the actions applied to
        - Condition: conditions for when this policy is in effect (optinal)
### **Connect SSH**
- PuTTY Gen Load key (save private key)
- Input ipv4 address (port 22) ec2-user@hostname
- Create session & save session
- Connection -> SSH -> Auth -> Browser -> Add link key ->  wayback session & save

### **AMI**
- AMI = Amazon Machine Image
- AMI are a customization of an EC2 instance
  - Add own software, configuring, operating system, monitoring...
  - Faster boot / configuration time because all software is pre-packaged
- AMI are built for a specific region ( add can be copied across regions)
- Lauch EC2 instance from:
  - A public AMI: AWS provided
  - Own AMI: make & matain them yourself
  - AWS Marketplace AMI: someone made

### **User data**
```#!/bin/bash
yum update -y
yum install -y httpd
systemctl start httpd
systemctl enable httpd
echo"$(hostname -f)"> /var/www/html/index.html
```

### **EBS Volume**
An EBS (Elastic Block Store) Volume is a network drive you can attach 
to your instances while they run
- It allows your instances to persist data, even after their termination
- They can only be mounted to one instance at a time (at the CCP level)
- They are bound to a specific availability zone
- Analogy: Think of them as a “network USB stick” 
  
EBS – Delete on Termination attribute (default: terminated when instance terminated)
![](https://res.cloudinary.com/boo-it/image/upload/v1667975914/aws/ebs_volume.png)

### **EBS Volume Types**
####6 types:
- gp2/gp3(SSD): Genaral purpose, balances price & performance
- iol/io2(SSD): Highest-performance, low-latency & high-throughput
- stl(HDD): Low cost, frequently accessed & throughput-intensive workloads
- scl(HDD): Lowest cost, less frequently accessed 

### **EFS**
Managed NFS (network file system)
- EFS has a higher price point than EBS
- Save cost (EFS-IA)
EBS sẽ phải trả cho dung lượng được cung cấp trước, EFS xài bao nhiêu thì trả bấy nhiêu

### **Load balancing**
Nó là 1 or tập hợp nhiều máy chủ sẽ chuyển tiếp lưu lượng tiếp nhận được đến nhiều server downstream(EC2 instances...)
Classic Load Balancers (v1)
- Supports TCP, HTTP & HTTPS

Application Load Balancer
- Support for HTTP/2 and WebSocket
- Support redirects
Network Load Balancer
Gateway Load Balancer