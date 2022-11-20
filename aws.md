
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

- Connection -> SSH -> Auth -> Browser -> Add link key -> wayback session & save

  

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

to your instances while they run (là 1 dịch vụ lưu trữ cấp khối, sử dụng riêng biệt giữa các phiên bản EC2, không thể gắn cùng lúc 2 phiên bản trên cùng 1 ổ đĩa EBS)
Note: EBS lưu trữ các tệp trong nhiều ổ đĩa được gọi là khối, hoạt động như các ổ đĩa cứng riêng biệt và không thể truy cập kho lưu trữ này qua internet. 

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

Managed NFS (network file system). Là một dịch vụ lưu trữ ở cấp độ tệp, EFS là bộ lưu trữ có tính sẵn sàng cao có thể được nhiều máy chủ sử dụng cùng một lúc

- EFS has a higher price point than EBS

- Save cost (EFS-IA)

EBS sẽ phải trả cho dung lượng được cung cấp trước, EFS xài bao nhiêu thì trả bấy nhiêu

  

![](https://res.cloudinary.com/boo-it/image/upload/v1667976670/aws/efs_filesystem.png)

### **Comparison EBS & EFS**
**Storage Type**

EBS (elastic block storage) & EFS (elastic file system), as the name suggests EBS is block-level storage and EFS is file-level storage.

**Availability**

As we know that EBS is directly attached to the instance so there is no sign of the term availability in it whereas Amazon EFS is highly durable and highly available storage. (EBS được gắn trực tiếp vào phiên bản nên không có dấu hiệu nào về thời hạn khả dụng trong đó trong khi Amazon EFS là bộ lưu trữ có độ bền cao và khả dụng cao.)

**Durability**

EBS is similar to hard disks, but the only difference is that EBS is connected to virtual EC2 instances and it offers 20 times more reliability than normal hard disks. (EBS tương tự như đĩa cứng nhưng điểm khác biệt duy nhất là EBS được kết nối với phiên bản EC2 ảo và nó mang lại độ tin cậy cao hơn 20 lần so với đĩa cứng thông thường.)

EFS is highly durable storage.

**Performance**

EBS offers a Baseline performance of 3 IOPS per GB for General Purpose volume and also we can use Provisioned IOPS for increased performance whereas EFS supports up to 7000 file system operations per second. (EBS cung cấp hiệu suất Cơ sở là 3 IOPS mỗi GB cho ổ đĩa mục đích chung và chúng ta cũng có thể sử dụng IOPS được cung cấp để tăng hiệu suất trong khi EFS hỗ trợ tới 7000 thao tác hệ thống tệp mỗi giây)

**Data Stored**

The data stored in EBS remains in the same availability zone and multiple replicas are created within the same availability zone whereas in EFS the data stored remains in the same region and multiple replicas are created within the same region.

**Comprehensive managed service**

EFS is a completely managed service, which means that your firm will never have to patch, deploy, or maintain your file system, but the same is not the case with EBS. (EFS là một dịch vụ được quản lý hoàn toàn, có nghĩa là công ty của bạn sẽ không bao giờ phải vá lỗi, triển khai hoặc bảo trì hệ thống tệp của mình, nhưng điều tương tự không xảy ra với EBS.)

**Data Access**

One most important disadvantage of EBS is that it cannot be accessed directly via the internet, it can only be accessed by a single EC2 instance with whom it is connected, whereas EFS storage allows access of 1 to 1000s of EC2 instances concurrently via the internet, but these instances must be present in the same region only. (Một nhược điểm quan trọng nhất của EBS là không thể truy cập trực tiếp qua internet, nó chỉ có thể được truy cập bởi một phiên bản EC2 duy nhất mà nó được kết nối, trong khi bộ lưu trữ EFS cho phép truy cập đồng thời từ 1 đến 1000 phiên bản EC2 qua internet nhưng những trường hợp này chỉ phải có mặt trong cùng một khu vực.)

**Encryption**

Both EBS and EFS supports encryption and uses an AWS KMS–Managed Customer Master Key (CMK) and AES 256-bit Encryption standards for encryption.

**File Size Limitation**

As EBS is directly connected to the EC2 instance so we have don’t have any limitation on file size whereas in EFS the maximum size of a single file can be up to 47.9TiB. (Vì EBS được kết nối trực tiếp với phiên bản EC2 nên không có bất kỳ giới hạn nào về kích thước tệp trong khi ở EFS, kích thước tối đa của một tệp có thể lên tới 47,9TiB.)

**Cost savings**

EFS is the only storage in which you’ll pay for is exactly what you use, as there’s no advance provisioning, up-front fees, or commitments whereas in EBS you need to attach a fixed amount of volume, and you are charged for the same.

### Use case
**Amazon EBS use cases:**

-   **Software Testing and development:**  Amazon EBS is connected only to a particular instance, so it is best suited for testing and development purposes. (Amazon EBS chỉ được kết nối với một phiên bản cụ thể, vì vậy, phiên bản này phù hợp nhất cho mục đích thử nghiệm và phát triển)
-   **Business continuity**: Amazon EBS provides a good level of business consistency as users can run applications in different AWS regions and all they require is EBS Snapshots and Amazon machine images. (Amazon EBS cung cấp mức độ nhất quán kinh doanh tốt vì người dùng có thể chạy các ứng dụng ở các khu vực AWS khác nhau và tất cả những gì họ yêu cầu là EBS Snapshots và Amazon machine images)
-   **Enterprise-wide applications:**  EBS provides block-level storage, so it allows users to run a wide variety of applications including Microsoft Exchange, Oracle, etc. (EBS cung cấp lưu trữ cấp khối, do đó, nó cho phép người dùng chạy nhiều loại ứng dụng bao gồm Microsoft Exchange, Oracle, v.v.)
-   **Transactional and NoSQL databases:**  As EBS provides a low level of latency so it offers an optimum level of performance for transactional and NO SQL databases. It also helps in database management. (Vì EBS cung cấp mức độ trễ thấp nên nó mang lại mức hiệu suất tối ưu cho cơ sở dữ liệu transactional and NO SQL databases. Nó cũng giúp quản lý cơ sở dữ liệu.)

**Amazon EFS use cases:**

-   **Lift-and-shift application support:**  EFS is elastic, highly available, and highly scalable storage, and these all features and enables users to move enterprise applications easily and quickly. (EFS là bộ lưu trữ đàn hồi, khả dụng cao và có khả năng mở rộng cao, đồng thời tất cả các tính năng này cũng như cho phép người dùng di chuyển các ứng dụng doanh nghiệp một cách dễ dàng và nhanh chóng.)
-   **Analytics for big data:**  EFS has got the ability to run big data applications. (EFS có khả năng chạy các ứng dụng dữ liệu lớn.)
-   **Web server support**: EFS is a highly robust throughput file system and is capable of enabling web serving applications, such as websites, or blogs. (EFS là một hệ thống tệp thông lượng mạnh mẽ và có khả năng kích hoạt các ứng dụng phục vụ web, chẳng hạn như trang web hoặc blog.)
-   **Application development and testing:** Among different storages provided by Amazon EFS is the only one that provides a shared file system needed to share code and files. (Trong số các kho lưu trữ khác nhau do Amazon EFS cung cấp, đây là kho lưu trữ duy nhất cung cấp hệ thống tệp dùng chung cần thiết để chia sẻ mã và tệp.)

### **Load balancing**

![](https://res.cloudinary.com/boo-it/image/upload/v1667983459/aws/load_balancer.png)

  

Nó là 1 or tập hợp nhiều máy chủ sẽ chuyển tiếp lưu lượng tiếp nhận được đến nhiều server downstream(EC2 instances...)

![](https://res.cloudinary.com/boo-it/image/upload/v1668919860/aws/load_balancer_transfer.png)

Có 4 loại giao thức chính mà quản trị **Load Balancer** có thể tạo quy định chuyển tiếp:

-   HTTP: dựa trên cơ chế HTTP chuẩn, HTTP Balancing đưa ra yêu cầu tác vụ. Load Balancer đặt X-Forwarded-For, X-Forwarded-Proto và tiêu đề X-Forwarded-Port cung cấp các thông tin backends về những yêu cầu ban đầu.
-   HTTPS: các chức năng tương tự HTTP Balancing. HTTPS Balancing được bổ sung mã hóa và nó được xử lý bằng 2 cách: passthrough SSL duy trì mã hóa tất cả con đường đến backend hoặc: chấm dứt SSL, đặt gánh nặng giải mã vào load balancer và gửi lưu lượng được mã hóa đến backend.
-   TCP: trong một số trường hợp khi ứng dụng không sử dụng giao thức HTTP hoặc HTTPS, TCP sẽ là một giải pháp để cân bằng lưu lượng. Cụ thể, khi có một lượng truy cập vào một cụm cơ sở dữ liệu, TCP sẽ giúp lan truyền lưu lượng trên tất cả các máy chủ
-   UDP: trong thời gian gần đây, Load Balancer đã bổ sung thêm hỗ trợ cho cân bằng tải giao thức internet lõi như DNS và syslogd sử dụng UDP.

Tùy thuộc công nghệ Load Balancing mà các thuật toán khác nhau sẽ được sử dụng để định tình trạng của máy chủ có hoạt động hay không. Có các loại thuật toán thường thấy là:

-   **Round Robin**
-   **Weighted Round Robin**
-   **Dynamic Round Robin**
-   **Fastest**
-   **Least Connections**

**Classic Load Balancers (v1)**

- Supports TCP, HTTP & HTTPS

![](https://res.cloudinary.com/boo-it/image/upload/v1668581444/aws/classic_balancer_1.png)

  

![](https://res.cloudinary.com/boo-it/image/upload/v1668581472/aws/classic_balancer_2.png)

  

![](https://res.cloudinary.com/boo-it/image/upload/v1668581502/aws/classic_balancer_3.png)

  

![](https://res.cloudinary.com/boo-it/image/upload/v1668581508/aws/classic_balacer_4.png)

  

Application Load Balancer

- Support for HTTP/2 and WebSocket

- Support redirects


Network Load Balancer

Gateway Load Balancer