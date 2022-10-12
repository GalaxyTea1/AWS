# AWS
### **What is cloud computing**
The practice of using a network of remote servers hosted on the Internet to store, manage, and process data, rather than a local server or a personal computer.

### **On-Premise**
- You own the servers
- You hire the IT people
- You pa or rent the real-estate
- You take all the risk

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

### **Zones**
- Usually 3, min is 2, max is 6

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

### **AMI**
- AMI = Amazon Machine Image
- AMI are a customization of an EC2 instance
  - Add own software, configuring, operating system, monitoring...
  - Faster boot / configuration time because all software is pre-packaged
- AMI are built for a specific region ( add can be copied across regions)
- Lauch EC2 instance from:
  - A public AMI: AWS provided
  - 