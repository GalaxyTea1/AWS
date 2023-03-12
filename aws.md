
# AWS

## What is cloud computing

The practice of using a network of remote servers hosted on the Internet to store, manage, and process data, rather than a local server or a personal computer.

  

## Cloud Providers

- Someone else owns the servers

- Someone else hires the IT people

- Someone else pays or rent the real-estate

- You are responsible for your configuring cloud services and code, someone else takes care of the rest

  

## AWS Global Infrastructure

- AWS Regions

- AWS Availability Zones

- AWS Data Centers

- AWS Edge Locations / Points of Presence

> https://aws.amazon.com/about-aws/global-infrastructure/

  

## AWS Regions

- AWS has Regions all around the word

- Names can be us-east-l, eu-west-3,...

- A region is a cluster of data centers

- Most AWS services are region-scoped

  

## Choose AWS Region

- Compliance

- Proximity

- Available services

- Pricing

Choose a region nearest customer

  

## Zones

- Usually 3, min is 2, max is 6

- Singapore (ap-southeast-1)

  

## IAM

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

## Connect SSH

- PuTTY Gen Load key (save private key)

- Input ipv4 address (port 22) ec2-user@hostname

- Create session & save session

- Connection -> SSH -> Auth -> Browser -> Add link key -> wayback session & save

  

## AMI

- AMI = Amazon Machine Image

- AMI are a customization of an EC2 instance

- Add own software, configuring, operating system, monitoring...

- Faster boot / configuration time because all software is pre-packaged

- AMI are built for a specific region ( add can be copied across regions)

- Lauch EC2 instance from:

- A public AMI: AWS provided

- Own AMI: make & matain them yourself

- AWS Marketplace AMI: someone made

  

## User data

```#!/bin/bash

yum update -y

yum install -y httpd

systemctl start httpd

systemctl enable httpd

echo"$(hostname -f)"> /var/www/html/index.html

```
![](https://res.cloudinary.com/boo-it/image/upload/v1669642430/aws/lifecycle.png)

**Note**:
Chọn loại EC2 cho phù hợp

- Đối với môi trường lại Test/dev, bạn có thể sử dụng loại T
Với các môi trường Product, có thể chọn loại M hoặc R (loại R cần khi dùng nhiều RAM) hơn.
- Với các loại C, G2, dùng cho các trường hợp đòi hỏi xử lý cao như các website cần phân tích nhiều, hoặc gaming.  

## EBS Volume

An EBS (Elastic Block Store) Volume is a network drive you can attach

to your instances while they run (là 1 dịch vụ lưu trữ cấp khối, sử dụng riêng biệt giữa các phiên bản EC2, không thể gắn cùng lúc 2 phiên bản trên cùng 1 ổ đĩa EBS)
Note: EBS lưu trữ các tệp trong nhiều ổ đĩa được gọi là khối, hoạt động như các ổ đĩa cứng riêng biệt và không thể truy cập kho lưu trữ này qua internet. 

- It allows your instances to persist data, even after their termination

- They can only be mounted to one instance at a time (at the CCP level)

- They are bound to a specific availability zone

- Analogy: Think of them as a “network USB stick”

EBS – Delete on Termination attribute (default: terminated when instance terminated)

![](https://res.cloudinary.com/boo-it/image/upload/v1667975914/aws/ebs_volume.png)

  

## EBS Volume Types

#### 6 types:

- gp2/gp3(SSD): Genaral purpose, balances price & performance

- iol/io2(SSD): Highest-performance, low-latency & high-throughput

- stl(HDD): Low cost, frequently accessed & throughput-intensive workloads

- scl(HDD): Lowest cost, less frequently accessed

  

## EFS

Managed NFS (network file system). Là một dịch vụ lưu trữ ở cấp độ tệp, EFS là bộ lưu trữ có tính sẵn sàng cao có thể được nhiều máy chủ sử dụng cùng một lúc

- EFS has a higher price point than EBS

- Save cost (EFS-IA)

EBS sẽ phải trả cho dung lượng được cung cấp trước, EFS xài bao nhiêu thì trả bấy nhiêu

  

![](https://res.cloudinary.com/boo-it/image/upload/v1667976670/aws/efs_filesystem.png)

## Comparison EBS & EFS
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

## Load balancing

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

## Classic Load Balancers (v1)

- Supports TCP, HTTP & HTTPS

![](https://res.cloudinary.com/boo-it/image/upload/v1668581444/aws/classic_balancer_1.png)

  

![](https://res.cloudinary.com/boo-it/image/upload/v1668581472/aws/classic_balancer_2.png)

  

![](https://res.cloudinary.com/boo-it/image/upload/v1668581502/aws/classic_balancer_3.png)

  

![](https://res.cloudinary.com/boo-it/image/upload/v1668581508/aws/classic_balacer_4.png)

  

## Application Load Balancer

- Support for HTTP/2 and WebSocket

- Support redirects


## Network Load Balancer

- Support for TCP/TLS/UDP

![](https://res.cloudinary.com/boo-it/image/upload/v1669190112/aws/nlb_1.png)

## Gateway Load Balancer

- Deploy, scale, and manage a fleet of 3rd party network virtual appliances in AWS (Triển khai, thay đổi quy mô và quản lý nhóm thiết bị mạng ảo của bên thứ 3 trong AWS)

![](https://res.cloudinary.com/boo-it/image/upload/v1669191904/aws/glb_1.png)

## Elastic Load Balancer
**- Sticky Sessions:**
![](https://res.cloudinary.com/boo-it/image/upload/v1669469170/aws/sticky_ss.png)
>Như vậy sticky session giúp bạn giữ lại phiên làm việc, các request sẽ được gửi tới cùng 1 máy EC2
Note: Sticky note là vô dụng với scale

**- Cross Zone Load Balancing:**
![](https://res.cloudinary.com/boo-it/image/upload/v1669475504/aws/cross_zone.png)
**- Connection Drainning:**
- Connection Drainning - for CLB
- Deregistration Delay - for ALB & NLB

Khi một ứng dụng scale in (xóa server đi), nó cần thời gian để xử lý nốt các request hiện đang có trên EC2 đó tránh việc website của bạn bị gián đoạn, và thời gian đó là Connection Draining

## Summary
So sánh đơn giản về sự khác nhau 3 LB này:
ALB: (Application Loadblancing): Trong mô hình OSI, nó nằm ở layer 7 (tầng Application) hỗ trợ tốt về HTTP/HTTPS, Config SSL, không hỗ trợ Elastic IP

NLB: (Network Loadblancing) : nằm ở layer 4 (tầng Transport) hỗ trợ tốt về TCP/UDP, Config SSL cũng được, hỗ trợ Elastic IP (EIP)

CLB: (Classic Loadblancing): Đây là LB cổ điển, AWS thay thế nó bằng ALB, nó không hỗ trợ tốt về một số điểm như là: LB multiple port trên cùng Interface, không hỗ trợ config target bằng IP, không hỗ trợ Web socket.

Tuy NLB ở layer 4 nên nó nhanh hơn so với ALB, phù hợp với những hệ thống chịu tải cao.. nhưng ALB hoạt động layer 7 có nhiều tính năng hơn so với NLB ngoài ra ALB có thể hoạt động với gRPC á... 

**gRPC là gì?**
Trước hết gRPC theo google giới thiệu:
gRPC is a modern open source high performance RPC framework that can run in any environment. It can efficiently connect services in and across data centers with pluggable support for load balancing, tracing, health checking and authentication. It is also applicable in last mile of distributed computing to connect devices, mobile applications and browsers to backend services.

gRPC là một RPC framework giúp bạn kết nối giữa các service trong hệ thống, nó hỗ trợ load balancing, tracing, health checking và authentication hỗ trợ từ mobile, trình duyệt cho tới back-end service.

gRPC sử dụng Protocol Buffer để transfer data thay vì JSON/XML truyền thống nên tốc độ được gia tăng đáng kể, ngoài ra nó cũng dùng RPC thay cho REST API. Trong việc thiết kế API sự khác biệt giữa REST API với RPC là REST được thiết kế tập trung vào Resource còn RPC thì tập trung vào action.

**Auto Scaling Groups**
AWS Auto Scaling là tính năng tự động nhân rộng để đảm bảo rằng các phiên bản Amazon EC2 đủ để chạy các ứng dụng của bạn. Bạn có thể tạo một nhóm AWS Auto Scaling trong các phiên bản EC2. Bạn có thể chỉ định số lượng phiên bản EC2 tối thiểu trong nhóm đó và tự động mở rộng sẽ duy trì và đảm bảo số lượng phiên bản EC2 tối thiểu.

Bạn cũng có thể chỉ định số lượng phiên bản EC2 tối đa trong mỗi nhóm tự động mở rộng để AWS Auto Scaling đảm bảo các phiên bản không bao giờ vượt quá giới hạn tối đa đó. Bạn cũng có thể chỉ định các chính sách dung lượng và tự động mở rộng mong muốn cho phần AWS Auto Scailing trong Amazon EC2. Bằng cách sử dụng chính sách mở rộng, AWS Auto Scaling có thể khởi chạy hoặc chấm dứt các phiên bản EC2 tùy theo nhu cầu.

![](https://res.cloudinary.com/boo-it/image/upload/v1670076378/aws/auto_scaling_gr.png)

**ASG Attributes**
- **A Lauch Template**

![](https://res.cloudinary.com/boo-it/image/upload/v1670078074/aws/ASG_Lauch_Template.png)

- **Min Size/ Max Size/ Initial**
- **Scaling Policies**

**CloudWatch Alarms & Scaling**
Thông báo về CPU trung bình hoặc tuỳ chỉnh số liệu
-> Tăng hoặc giảm số lượng phiên bản EC2 để phù hợp

**Create ASG**
- **Firstly, create template**
![](https://res.cloudinary.com/boo-it/image/upload/v1670078656/aws/templateASG.png)

![](https://res.cloudinary.com/boo-it/image/upload/v1670078790/aws/templateASG2.png)

![](https://res.cloudinary.com/boo-it/image/upload/v1670078914/aws/templateASG3.png)

![](https://res.cloudinary.com/boo-it/image/upload/v1670078914/aws/templateASG4.png)

- **Secondly**
![](https://res.cloudinary.com/boo-it/image/upload/v1670079201/aws/createASG.png)

![](https://res.cloudinary.com/boo-it/image/upload/v1670079201/aws/createASG2.png)

![](https://res.cloudinary.com/boo-it/image/upload/v1670079201/aws/createASG3.png)

**Scaling Policies**
- **Target Tracking Scaling**
Most simple and easy to set-up
Ex: ASG CPU around 40%
- **Simple/ Step Scaling**
CloudWatch alarm is triggered (CPU > 70% -> add 2 units)
CloudWatch alarm is triggered (CPU < 40% -> remove 2 units)
- **Scheduled Actions**
Ex: 5pm add 2 units...
- **Predict Scaling** 
Continuously forecast load and  schedule scaling ahead (liên tục dự báo tải và lên lịch mở rộng quy mô trước)

**RDS**
- RDS stands for Relational Database Service (cho phép setup các thao tác, scale csdlqh trên AWS)
- Support: Postgres, MySQL, MariaDB, Oracle, Microsoft SQL Server, Aurora

Amazon RDS sẽ đảm nhận các tác vụ khó hay các tác vụ quản lý:

- Phân bổ CPU, IOPS hay storage một cách tuỳ biến
- RDS sử dụng AWS backup service cho việc backup data, tự động phát hiện lỗi và recovery
- Không support việc access RDS instance thông qua shell
- Có thể backup tự động hay thủ công Snapshot
- Khả năng tự đồng bộ cao giữa primary và secondary
- Kiểm soát được việc access vào RDS thông qua IAM, bảo vệ database bằng cách đẩy lên virtual private cloud

**RDS - Storage Auto Scaling**

Automatically modify storage if:
- Free storage is less than 10% of allocated storage
- Low-storage last at least 5 minutes
- 6 hours have passed since last modification

![](https://res.cloudinary.com/boo-it/image/upload/v1670757902/aws/RDS_AutoScaling.png)

**RDS - Replica & Multi AZ**

Read replicas là bản sao của main db được nhân rộng ra nhằm phục vụ cho việc đọc dữ liệu từ nó

![](https://res.cloudinary.com/boo-it/image/upload/v1670757963/aws/RSD_ReadReplica.png)

**Ví dụ về trường hợp sử dụng RR**

![](https://res.cloudinary.com/boo-it/image/upload/v1670759144/aws/ex_rep.png)

**RDS - Disaster Recovery**
- SYNC replication
- Sử dụng chung 1 DNS endpoint
- Không sử dụng cho scaling
- Read Replicas có thể setup như là Multi AZ cho Disaster Recovery

![](https://res.cloudinary.com/boo-it/image/upload/v1670758102/aws/RDS_DisasterRecovery.png)

## RDS - From Single AZ To Multi Az
Các hoạt động:
- Tạo 1 snapshot
- DB mới được restored từ snapshot sang AZ mới
- Synchronyze giữa 2 DB
![](https://res.cloudinary.com/boo-it/image/upload/v1670758142/aws/RDS_fromSingleAZtoMultipleAZ.png)


**Amazon Aurora**
Là một cơ sở dữ liệu quan hệ được cung cấp bởi Amazon, được quản lý đầy đủ, tương thích với MySQL và PostgreSQL và được xây dựng cho cloud, kết hợp hiệu suất và tính khả dụng của các cơ sở dữ liệu thương mại cao cấp với tính đơn giản và hiệu quả về chi phí của các cơ sở dữ liệu mã nguồn mở.

Aurora nhanh gấp 5 lần cơ sở dữ liệu MySQL tiêu chuẩn và nhanh gấp 3 lần các cơ sở dữ liệu PostgreSQL chuẩn mà không cần yêu cầu thay đổi gì đến các ứng dụng có sẵn. Nó cung cấp tính bảo mật, tính khả dụng và độ tin cậy của các cơ sở dữ liệu cấp thương mại với chi phí 1/10. 

Aurora được quản lý đầy đủ bởi Amazon Relational Database Service (RDS), giúp tự động hóa các nhiệm vụ quản trị tốn nhiều thời gian như cung cấp phần cứng, thiết lập cơ sở dữ liệu, vá lỗi và sao lưu.

Ngoài ra Aurora có thể có 15 replicas trong khi MySQL chỉ có 5

![](https://res.cloudinary.com/boo-it/image/upload/v1671001391/aws/aurora.png)

**Aurora DB Cluster**

![](https://res.cloudinary.com/boo-it/image/upload/v1671001461/aws/aurora_2.png)

Note: Writer Endpoint n Reader Endpoint

**Aurora Security**
- **At-rest encryption** (mã hóa trong quá trình nghỉ ngơi)
- **In-flight encryption** (mã hóa trong quá trình vận chuyển)
- **IAM Authentication**
- **Security Groups**
- **No SSH available exept on RDS Custom**
- **Audit Logs can be enabled**

##ElastiCache
Hai công nghệ cache phổ biến và được sử dụng nhiều nhất là Memcached and Redis. ElastiCache hỗ trợ cả hai loại trên.

ElastiCache là một dịch vụ của AWS mà cho phép ta tạo một clusters Memcached hoặc Redis một cách dễ dàng thay vì ta phải tự cài đặt và cấu hình nhiều thứ

![](https://res.cloudinary.com/boo-it/image/upload/v1670758205/aws/RDS_DBCache.png)

**User Session Store**
![](https://res.cloudinary.com/boo-it/image/upload/v1670758244/aws/userSesionStore.png)

**ElastiCache - Redis & Memcached**

![](https://res.cloudinary.com/boo-it/image/upload/v1671004470/aws/compareCache.png)

**Caching Implementation Considerations**
- **Lazy Load / Cache-Aside / Lazy Population**
- **Write Through - Add or Update cache when db is updated**
- **Cache Evictions and Time-to-live**


## Route 53
**DNS**
Domain Nam System which translates the human friendly hostnames into the machine IP addresses (google.com -> 172.217.18.36)

**DNS Terminologies**
- Domain Registrar: Amazon Route 53, GoDaddy...
- DNS Record: A, AAAA, CNAME, NS...
- Zone File: Contains DNS records
- Name Server: resolves DNS queries
- Top Level Domain (TLD): .com, .us, .vn,...
- Second Level Domain (SLD): amazon.com, google.com,...

![](https://res.cloudinary.com/boo-it/image/upload/v1673181073/aws/DNS.png)
<br/>
**How DNS works**
![](https://res.cloudinary.com/boo-it/image/upload/v1673181144/aws/DNS_work.png)

### Amazon Route 53
Amazon Route 53 is a highly available, scalable and Authoritative (the customer can update the DNS records) Domain Name System (DNS) web service.

![](https://res.cloudinary.com/boo-it/image/upload/v1673181269/aws/Route53.png)

### Route 53 Records
Each record contains:
- Domain/ subdomain Name - eg: example.com
- Record Type - eg: A, AAAA...
- Value - eg: 12.12.23.34
- Routing policy: how route 53 responds to queries
- TTL: amount of time the record cached at DNS Resolvers

### Route 53 – Record Types
- A: maps a hostname to IPv4
- AAAA: maps a hostname to IPv6
- CNAME: maps a hostname to another hostname

    - The target is a domain name which must have an A or AAAA record
    - Can't create CNAME record for the top node of a DNS namespace (Zone Apex). Eg: You can not create for example.com but you can create for xxx.example.com

- NS - Name Servers for the Hosted Zone
    - Control how traffic is routed for a domain

### Route 53 – Hosted Zones
A container for records that define how to route traffic to a domain (example.com) and its subdomains (zizi.example.com)
- Public Hosted Zones – contains records that specify how to route 
traffic on the Internet (public domain names) application1.mypublicdomain.com
- Private Hosted Zones – contain records that specify how you route 
traffic within one or more VPCs (private domain names) application1.company.internal

![](https://res.cloudinary.com/boo-it/image/upload/v1673772623/aws/HostedZone.png)

### Route53 - TTL (Time To Live)
- **High TTL - e.g., 24hr**: 
    - Less traffic
    - Possibly outdated records
- **Low TTL** - e.g.,60sec**:
    - More traffic (fee $)
    - Records are outdated for less time
    - Easy to change records

**Note**: 
   - Except for Alias records, TTL is mandatory for each DNS record

### CName & Alias
- **CName**:
    - Points a hostname to any orther hostname (app.domain.com => blabla.anything.com)
    - Only for Non Root Domain
- **Alias**:
    - Points a hostname to an AWS Resource (app.domain.com => blabal.amazonaws.com)
    - Works for Root Domain & Non Root Domain
    - Free of charge
    - Native health check
    - Alias records can't set the TTL
    - Can't set an Alias record for an EC2 DNS name

### Routing Policy
- Simple
    - Typical route traffic to a single resource
    - Can specify multiple values in the same record (a random one is chosen by the client)
    - Can not be associated with Health Checks
- Weight
    - Control % of the requests that go to each specific resource.
    - Asign each record a relative weight:
        - traffic (%) = weight for  a specific record / Sum of all the weights for all records
    - DNS records must have the same name and type
    - Can be associated with Health Checks
    - Use case: load balancing between regions, testing new application version...
- Latency based
    - Redirect to the resource that has least latency close to us
    - Can be associated with Health Checks
- Failover
![](https://res.cloudinary.com/boo-it/image/upload/v1675606285/aws/failover.png)
- Geolocation
     - This routing is based on user location
    - Specify location by continent, country
    - Should create default record (in case there is no match on location)
    - Can be associated with Health Checks
- Geoproximity
    - Route traffic to your resources based on the geographic location of users and resources
- Multiple Value
    - Use when routing traffic to multiple resources
    - Up to 8 healthy records are returned for each Multi-Value query
    - Can be associated with Health Checks (returns only values for healthy resources)
- Health Checks
    - HTTP Health Checks are only for public resources
    - About 15 global health checkers will check the endpoint health
    - Route 53 health checker are outside the VPC -> they can not access private endpoints (You can create CloudWatch Metric and associate a CloudWatch Alarm, then create a Health Check that checks the alarm itself)

### VPC Fundamentals (Virtual Private Cloud)
#### Introduction
##### 1. VPC, Subnets, Internet Gateways & NAT Gateways
**VPC**: private network to deploy your resources
**Subnets**: allow you to partition your network inside your VPC (AZ Resources)
- A public subnet is a subnet that is accessible from the Internet  
- A private subnet is a subnet that isn't accessible from the Internet
- To define access to the Internet and between subnets, we you Route Tables
![](https://res.cloudinary.com/boo-it/image/upload/v1676173816/aws/VPC_subnets.png)

**VPC Diagram**

![](https://res.cloudinary.com/boo-it/image/upload/v1676191744/aws/VPC_diagram.png)

**Internet Gateways & NAT**

![](https://res.cloudinary.com/boo-it/image/upload/v1676191721/aws/gateway_nat.png)

##### 2. Security Groups, Network ACL (NACL), VPC Flow logs
**NACL & Security Groups**
NACL (Network ACL)
- A firewall which controls traffic from and to 
subnet
- Can have ALLOW and DENY rules
- Are attached at the Subnet level
- Rules only include IP addresses

Security Groups
- A firewall that controls traffic to and from an 
ENI / an EC2 Instance
- Can have only ALLOW rules
- Rules include IP addresses and other security 
groups

<p align="center">
  <img src="https://res.cloudinary.com/boo-it/image/upload/v1676192209/aws/NACL.png" />
</p>
<!-- ![](https://res.cloudinary.com/boo-it/image/upload/v1676192209/aws/NACL.png) -->

![](https://res.cloudinary.com/boo-it/image/upload/v1676193212/aws/compare_nacl_sg.png)

**VPC Flow Logs**
- Capture information about IP traffic going into your interfaces:
    - VPC Flow Logs
    - Subnet Flow Logs
    - Elastic Network Interface Flow Logs
- Helps to monitor & troubleshoot connectivity issues. Example: 
    - Subnets to internet
    - Subnets to subnets
    - Internet to subnets
- Captures network information from AWS managed interfaces too: Elastic 
Load Balancers, ElastiCache, RDS, Aurora, etc… 
- VPC Flow logs data can go to S3 / CloudWatch Logs

##### 3. Site to Site VPN & Direct Connect 
**VPC Peering, Endpoints, VPN, DX**
**VPC Peering**

![](https://res.cloudinary.com/boo-it/image/upload/v1676193481/aws/VPC_peering.png)

**VPC Endpoints**
<p align="center">
  <img src="https://res.cloudinary.com/boo-it/image/upload/v1676193488/aws/VPC_endpoint.png" />
</p>

**Site to Site VPN & Direct Connect**

![](https://res.cloudinary.com/boo-it/image/upload/v1676193492/aws/SiteToSiteVPN.png)
### S3 Introduction
- Amazon S3 is one of the main building blocks of AWS
- It’s advertised as ”infinitely scaling” storage 
- Many websites use Amazon S3 as a backbone
- Many AWS services use Amazon S3 as an integration as well
- We’ll have a step-by-step approach to S3

#### Use cases
- Backup and storage
- Disaster Recovery
- Archive
- Hybrid Cloud storage
- Application hosting
- Media hosting
- Data lakes & big data analytics
- Software delivery
- Static websit

#### S3 - Security
- User-Based
    - IAM Policies – which API calls should be allowed for a specific user from IAM (lệnh gọi API nào sẽ được phép cho một người dùng cụ thể từ IAM)
- Resource-Based
    - Bucket Policies – bucket wide rules from the S3 console - allows cross account (bucket wide rules từ bảng điều khiển S3 - cho phép nhiều tài khoản)
    - Object Access Control List (ACL) – finer grain (can be disabled)
    - Bucket Access Control List (ACL) – less common (can be disabled)
- Encryption: encrypt objects in Amazon S3 using encryption keys

 Note: an IAM principal can access an S3 object if
• The user IAM permissions ALLOW it OR the resource policy ALLOWS it
• AND there’s no explicit DENY

#### Static Website
![](https://res.cloudinary.com/boo-it/image/upload/v1676793933/aws/demo_static_web.png)

![](https://res.cloudinary.com/boo-it/image/upload/v1676793734/aws/demo_static_web2.png)

#### S3 -Versioning
<p align="center">
  <img src="https://res.cloudinary.com/boo-it/image/upload/v1676795072/aws/S3_version.png" />
</p>

#### Replication
- Must enable Versioning in source and destination buckets
- Cross-Region Replication (CRR)
- Same-Region Replication (SRR)
- Buckets can be in different AWS accounts
- Copying is asynchronous
- Must give proper IAM permissions to S3
- Use cases:
    - CRR – compliance, lower latency access, replication across accounts
    - SRR – log aggregation, live replication between production and test 
accounts

**Note**
- After you enable Replication, only new objects are replicated
- Optionally, you can replicate existing objects using S3 Batch Replication
    - Replicates existing objects and objects that failed replication
- For DELETE operations
    - Can replicate delete markers from source to target (optional setting)
    - Deletions with a version ID are not replicated (to avoid malicious deletes)
- There is no “chaining” of replication
    - If bucket 1 has replication into bucket 2, which has replication into bucket 3
    - Then objects created in bucket 1 are not replicated to bucket 3

#### Storage Classes
- Amazon S3 Standard for frequent data access: Suitable for a use case where the latency should below. Example: Frequently accessed data will be the data of students’ attendance, which should be retrieved quickly. (Amazon S3 Tiêu chuẩn để truy cập dữ liệu thường xuyên: Phù hợp với trường hợp sử dụng mà độ trễ phải thấp hơn. Ví dụ: Dữ liệu thường xuyên truy cập sẽ là dữ liệu điểm danh của học sinh, dữ liệu này cần được truy xuất nhanh chóng)

- Amazon S3 Standard for infrequent data access: Can be used where the data is long-lived and less frequently accessed. Example: Students’ academic records will not be needed daily, but if they have any requirement, their details should be retrieved quickly. (Amazon S3 Standard dành cho truy cập dữ liệu không thường xuyên: Có thể được sử dụng khi dữ liệu tồn tại lâu và ít truy cập thường xuyên. Ví dụ: Hồ sơ học tập của sinh viên sẽ không cần thiết hàng ngày, nhưng nếu họ có bất kỳ yêu cầu nào, thông tin chi tiết của họ sẽ được truy xuất nhanh chóng)

- Amazon Glacier: Can be used where the data has to be archived, and high performance is not required. Example: Ex-student’s old record (like admission fee) will not be needed daily, and even if it is necessary, low latency is not required. (Có thể được sử dụng khi dữ liệu phải được lưu trữ và không yêu cầu hiệu suất cao. Ví dụ: Hồ sơ cũ của cựu sinh viên (như phí nhập học) sẽ không cần thiết hàng ngày và ngay cả khi cần thiết, độ trễ thấp cũng không cần thiết)

- One Zone-IA Storage Class: It can be used where the data is infrequently accessed and stored in a single region. Example: Student’s report card is not used daily and stored in a single availability region (i.e., school). (Nó có thể được sử dụng khi dữ liệu không được truy cập thường xuyên và được lưu trữ trong một vùng duy nhất. Ví dụ: Phiếu điểm của học sinh không được sử dụng hàng ngày và được lưu trữ ở một khu vực sẵn có duy nhất (ví dụ: trường học))

- Amazon S3 Standard Reduced Redundancy storage: Suitable for a use case where the data is non-critical and reproduced quickly. Example: Books in the library are non-critical data and can be replaced if lost. (Thích hợp cho trường hợp sử dụng khi dữ liệu không quan trọng và được sao chép nhanh chóng. Ví dụ: Sách trong thư viện là dữ liệu không quan trọng và có thể được thay thế nếu bị mất)

![](https://res.cloudinary.com/boo-it/image/upload/v1676813224/aws/storage_classes.jpg)

#### AWS CLI
- Dry Run
![](https://res.cloudinary.com/boo-it/image/upload/v1678023029/aws/dry-run.png)

- STS Decode Errors
![](https://res.cloudinary.com/boo-it/image/upload/v1678023692/aws/sts_decode.png)

#### AWS Instance Metadata
<code>[ec2-user@ip-172-31-3-136~]$ curl http://169.254.169/254/lastest/
dynamic
meta-data</code>

**Read more**:
- Profiles
- CLI with MFA
- AWS SDK
- Exponential Backoff & Service Limit Increase
- Credentials Provider Chain

#### CloudFont
#### ECS, ECR & Fargate Docker
#### BeanStalk
#### SQS, SNS & Kinesis
#### Lamda
#### KMS
