
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
- The user IAM permissions ALLOW it OR the resource policy ALLOWS it
- AND there’s no explicit DENY

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
- AWS CloudFront is a globally-distributed network offered by Amazon Web Services, which securely transfers content such as software, SDKs, videos, etc., to the clients, with high transfer speed.
- Content Delivery Network(CDN)
- Improves read performance, content 
is cached at the edge (216 Point of Presence globally)
- DDoS protection, integration with Shield, AWS Web Application Firewall
- Can expose external HTTPS and can talk to internal HTTPS backends

**There are three core concepts that you need to understand to start using CloudFront: distributions, origins, and cache control.**

##### Distributions
To use Amazon CloudFront, you start by creating a distribution, which is identified by a DNS domain name. To serve files from Amazon CloudFront, you simply use the distribution domain name in place of your website’s domain name; the rest of the file paths stay unchanged.

##### Origins

When you create a distribution, you must specify the DNS domain name of the origin — the Amazon S3 bucket or HTTP server — from which you want Amazon CloudFront to get the definitive version of your objects (web files).

##### Cache-Control
Once requested and served from an edge location, objects stay in the cache until they expire or are evicted to make room for more frequently requested content.
##### CloudFont Origin
- S3 bucket (can be private, need to setup Origin Access Identity + S3 bucket policy)
- Custom Origin (HTTP)
    - Application Load Balancer
    - EC2 instance 
    - S3 website (must first enable the bucket as a static S3 website) 
    - Any HTTP backend 

##### CloudFont at a high level
![](https://res.cloudinary.com/boo-it/image/upload/v1679679375/aws/CF_at_high_level.png)
##### CloudFont - S3 as an Origin
![](https://res.cloudinary.com/boo-it/image/upload/v1679679383/aws/s3_origin.png)

##### CloudFront vs S3 Cross Region Replication
- CloudFront:
    - Global Edge network
    - Files are cached for a TTL (maybe a day)
    - Great for static content that must be available everywhere
    
- S3 Cross Region Replication:
    - Must be setup for each region you want replication to happen
    - Files are updated in near real-time
    - Read only
    - Great for dynamic content that needs to be available at low-latency in few region
##### CloudFront Caching
Reducing the number of requests to our origin server directly is one of the goals of using CloudFront. Due to CloudFront caching, more objects are served from CloudFront edge locations, which are nearer to users. This reduces latency and minimizes the load on our origin server.
(Giảm số lượng yêu cầu trực tiếp tới máy chủ nguồn của chúng tôi là một trong những mục tiêu khi sử dụng CloudFront. Nhờ bộ đệm của CloudFront, nhiều đối tượng được phục vụ từ các vị trí cạnh CloudFront gần hơn với người dùng. Điều này giảm độ trễ và giảm thiểu tải trên máy chủ nguồn)

We can cache on multiple things:
- Headers
- Session Cookies
- Query string parameters

![](https://res.cloudinary.com/boo-it/image/upload/v1679830811/aws/cloundfont_cache.png)

In the diagram, we can see that when a client makes a request to the CloudFront Edge Locations the cache will be checked on the basics of the values of headers and the cookies and the query string parameters. And then the cache has an expiry based on the time to live in the cache, and if the value is not in the cache, then the query, or the entire HTTPS request, is forwarded directly to the origin, and then the cache is filled by query response

#### ECS, ECR & Fargate Docker
##### ECS (Elastic Container Service)
- ECS is a managed container orchestration platform that enables fast deployment and scaling of containerized workloads.

**Docker containers management on AWS**

- Amazon Elastic Container Service (Amazon ECS)
- Amazon Elastic Kubernetes Service (Amazon EKS)
- AWS Fargate (serverless)
- Amazon ECR (store container images)

**Amazon ECS - EC2 Launch Type**
- Launch Docker containers on AWS = Launch ECS Tasks on ECS Clusters
- EC2 Launch Type: you must provision & maintain the infrastructure (the EC2 
instances)
- Each EC2 Instance must run the ECS Agent to register in the ECS Cluster
- AWS takes care of starting / stopping containers

![](https://res.cloudinary.com/boo-it/image/upload/v1680099051/aws/ECS_cluster.png)

![](https://res.cloudinary.com/boo-it/image/upload/v1680324545/aws/flow_ecs.png)

**Fargate Launch Type**
The Fargate launch type allows you to run your containerized applications without the need to provision and manage the backend infrastructure. Just register your task definition and Fargate launches the container for you. (serverless) 
(Kiểu khởi chạy Fargate cho phép bạn chạy ứng dụng được đóng gói thành container mà không cần phải cấu hình và quản lý cơ sở hạ tầng phía sau. Chỉ cần đăng ký định nghĩa tác vụ của bạn và Fargate sẽ khởi chạy container cho bạn. Được gọi là "serverless" - không cần máy chủ riêng)
- Just create task definitions (Chỉ cần tạo các định nghĩa tác vụ task definitions).
- AWS just run ECS tasks for you based on the CPU/RAM you need.
- To scale, just increase the number of tasks.

![](https://res.cloudinary.com/boo-it/image/upload/v1680324369/aws/Fargate.png)

**Load Balancer Integrations**
- Application Load Balancer supported and works for most use cases (Fargate được hỗ trợ bởi Application Load Balancer (ALB) của AWS và hoạt động cho hầu hết các trường hợp sử dụng. ALB giúp phân phối tải đến các tác vụ Fargate một cách thông minh dựa trên các yêu cầu của khách hàng và trạng thái của các tác vụ đang chạy. Bằng cách kết hợp Fargate với ALB, bạn có thể tạo ra một môi trường có khả năng mở rộng linh hoạt để chạy các ứng dụng web phức tạp.)
- Network Load Balancer recommended only for high throughput / high performance use cases, or to pair it with AWS Private Link (Network Load Balancer (NLB) là một giải pháp được khuyến nghị cho các trường hợp sử dụng có lưu lượng cao hoặc đòi hỏi hiệu suất cao hơn. NLB hoạt động ở mức độ kết nối mạng và có thể xử lý hàng triệu kết nối trên giây, làm cho nó phù hợp cho các ứng dụng với yêu cầu về lưu lượng mạng cao. NLB cũng được khuyến nghị để kết hợp với AWS Private Link, một dịch vụ cho phép các khách hàng kết nối với các tài nguyên AWS thông qua kết nối mạng riêng ảo (VPN) của họ. Kết hợp NLB và AWS Private Link cho phép khách hàng truy cập các ứng dụng và dịch vụ của bạn một cách an toàn và bảo mật từ các mạng khác nhau, bao gồm mạng công cộng và mạng riêng.)
-  Elastic Load Balancer supported but not recommended (no advanced features –no Fargate) (ELB không có các tính năng tiên tiến như ALB hoặc NLB, vì vậy nó không phù hợp với các ứng dụng Fargate phức tạp hoặc đòi hỏi tính năng mở rộng và quản lý tải tốt hơn. Ngoài ra, ELB cũng không hỗ trợ Fargate và chỉ hỗ trợ các kiểu khởi chạy container khác như EC2. Vì vậy, nên sử dụng ALB hoặc NLB thay vì ELB để đạt được hiệu quả tối ưu cho ứng dụng)

![](https://res.cloudinary.com/boo-it/image/upload/v1680324946/aws/Load_Balancer_Integrations.png)

**EC2 Launch Type**
- We get a Dynamic Host Port Mapping if you define only the container port in the task definition
- ALB finds the right port on your EC2 instance
- We must allow on EC2 instance's Security Group any port from the ABL's Security Group

**Fargate**
- Each task has a unique private IP
- Only define the container port (host port is not applicable)

**Task definition**
Task definition are metadata in JSON form to tell ECS how to run a Docker container.
It contains crucial information, such as:

- Image name
- Port binding for Container and Host
- Memory and CPU required
- Environment variables
- Networking information
- IAM role
- Logging configuration (CloudWatch)

We can define up to 10 contains in a Task Definition
![](https://res.cloudinary.com/boo-it/image/upload/v1681018722/aws/task_define.png)

**Data Volume EFS**
- Mount EFS file systems onto ECS tasks
- Works for both EC2 and Fargate launch types
- Tasks running in any AZ will share the same data in the EFS file system
- Fargate + EFS = Serverless
- Use cases: persistent multi-AZ shared storage for your containers

**Note:** 
- Amazon S3 cannot be mounted as a file system

<p align="center">
  <img src="https://res.cloudinary.com/boo-it/image/upload/v1680325945/aws/efs.png" />
</p>

**ECS Service Auto Scalling**
- Automatically increase/decrease the desired number of ECS tasks
- Amazon ECS Auto Scaling uses AWS Application Auto Scaling
    - ECS Service Average CPU Utilization
    - ECS Service Average Memory Utilization - Scale on RAM
    - ALB Request Count Per Target – metric coming from the ALB
- Target Tracking – scale based on target value for a specific CloudWatch metric
- Step Scaling – scale based on a specified CloudWatch Alarm
- Scheduled Scaling – scale based on a specified date/time (predictable changes)
- ECS Service Auto Scaling (task level) ≠ EC2 Auto Scaling (EC2 instance level)

![](https://res.cloudinary.com/boo-it/image/upload/v1680968730/aws/ASC_serviceCpu.png)

**ECR (Elastic Container Registry)**
Amazon ECR is a fully managed container registry offering high-performance hosting, so you can reliably deploy application images and artifacts anywhere. (Amazon ECR là một registry container được quản lý hoàn toàn, cung cấp khả năng lưu trữ hiệu suất cao, để bạn có thể triển khai hình ảnh và tài liệu ứng dụng một cách đáng tin cậy ở bất cứ đâu)
![](https://res.cloudinary.com/boo-it/image/upload/v1681018807/aws/ecr.png)

**Login Command**
AWS CLI v2
<code>aws ecr get-login-password --region <span style="color:red">region</span> | docker login --username AWS --password-stdin <span style="color:red">aws_account_id</span>.dkr.ecr.region.amazonaws.com</code>

**Docker Command**
Push
<code>docker push <span style="color:red">aws_account_id</span>.dkr.ecr.<span style="color:red">region</span>.amazonaws.com/demo:latest</code>

Pull
<code>docker pull<span style="color:red"> aws_account_id</span>.dkr.ecr.<span style="color:red">region</span>.amazonaws.com/demo:latest</code>

**EKS (Amazon Elastic Kubernetes)**
Amazon EKS is a managed service that you can use to run Kubernetes on AWS without needing to install, operate, and maintain your own Kubernetes control plane or nodes. (Amazon Elastic Kubernetes Service (Amazon EKS) là một dịch vụ được quản lý mà bạn có thể sử dụng để chạy Kubernetes trên AWS mà không cần cài đặt, vận hành và duy trì bảng điều khiển Kubernetes hoặc các nút của riêng bạn.)
- Kubernetes là một hệ thống mã nguồn mở để triển khai, tự động điều chỉnh và quản lý các ứng dụng được đóng gói theo hình thức container (thông thường là Docker)
- Đây là một sự lựa chọn thay thế cho ECS, mục tiêu tương tự nhưng API khác nhau
- EKS hỗ trợ EC2 nếu bạn muốn triển khai các nút worker hoặc Fargate để triển khai các container serverless
- Use case: nếu công ty của bạn đang sử dụng Kubernetes trên nội bộ hoặc trên một đám mây khác và muốn chuyển đổi sang AWS bằng cách sử dụng Kubernetes
- Kubernetes là độc lập với đám mây (có thể sử dụng trên bất kỳ đám mây nào - Azure, GCP ...)
- Đối với nhiều khu vực, triển khai một cụm EKS cho mỗi khu vực
- Thu thập các nhật ký và số liệu sử dụng CloudWatch Container Insights.

<p align="center">
  <img src="https://res.cloudinary.com/boo-it/image/upload/v1680968917/aws/eks.png" />
</p>

**Amazon EKS – Data Volumes**
- Need to specify StorageClass manifest on your EKS cluster (Cần chỉ định tệp định dạng StorageClass trên cụm EKS của bạn)
- Leverages a Container Storage Interface (CSI) compliant driver

Container Storage Interface (CSI) là một tiêu chuẩn giao diện lập trình ứng dụng (API) cho phép các trình điều khiển lưu trữ có thể tích hợp với các hệ thống quản lý container như Kubernetes.

Khi một trình điều khiển lưu trữ tuân thủ đối với CSI, nó sẽ cung cấp các chức năng như quản lý, cung cấp và gỡ bỏ các khối lưu trữ cho các ứng dụng container chạy trên Kubernetes.

Vì vậy, khi bạn sử dụng một trình điều khiển lưu trữ tuân thủ đối với CSI trong Kubernetes, bạn có thể dễ dàng tích hợp các lưu trữ khác nhau vào các ứng dụng container của mình mà không cần quan tâm đến cách lưu trữ được triển khai.

Support for…
• Amazon EBS
• Amazon EFS (works with Fargate)
• Amazon FSx for Lustre
• Amazon FSx for NetApp ONTAP

#### BeanStalk
**Typical architecture: Web App 3-tier**

![](https://res.cloudinary.com/boo-it/image/upload/v1681483490/aws/beanstalk_typical.png)

**Web Server Tier vs. Worker Tier**
![](https://res.cloudinary.com/boo-it/image/upload/v1681544263/aws/web_env_worker_env.png)

#### SQS, SNS & Kinesis
**What's a queue?**
SQS là viết tắt của "Simple Queue Service", là một dịch vụ được cung cấp bởi Amazon Web Services (AWS), đây là một dịch vụ nhắn tin (messaging) dựa trên hàng đợi. SQS cho phép các ứng dụng gửi và nhận các thông điệp (messages) giữa các thành phần của hệ thống phân tán (distributed system) một cách đáng tin cậy.

SQS được sử dụng để giải quyết vấn đề liên quan đến đồng bộ hóa các thành phần trong kiến trúc phân tán, nơi mà các thành phần cần phải gửi và nhận các thông điệp một cách bất đồng bộ. Các ứng dụng có thể gửi các thông điệp đến hàng đợi SQS, và các thành phần khác trong hệ thống có thể đọc các thông điệp từ hàng đợi đó để xử lý. SQS cung cấp khả năng chuyển tiếp tự động và xử lý lại các thông điệp không thành công, đồng thời đảm bảo tính nhất quán và độ tin cậy trong giao tiếp giữa các thành phần trong kiến trúc phân tán.

SQS có thể được sử dụng trong các ứng dụng đa nền tảng, các ứng dụng đòi hỏi khả năng mở rộng cao, và các ứng dụng cần độ tin cậy cao trong việc gửi và nhận các thông điệp.

![](https://res.cloudinary.com/boo-it/image/upload/v1681569116/aws/sqs_queue.png)

**Standard Queue**
- Unlimited throughput, unlimited number of messages in queue
- Default retention of messages: 4 days, maximum of 14 days
- Low latency  (<10 ms on publish and receive)
- Limitation of 256KB per message sent
- Can have duplicate message (at least once delivery)
- Can have out of orders messages

**SQS - ASG**
![](https://res.cloudinary.com/boo-it/image/upload/v1681571904/aws/sqs_with_asg.png)

**SQS - Decouple**
![](https://res.cloudinary.com/boo-it/image/upload/v1681571932/aws/sqs_decouple.png)

**SQS - Access Policy**
**Visibility timeout**
When the producer sends a message to SQS, it is stored in the queue until consumed by a consumer. When the consumer is ready, it polls SQS for new messages and ultimately receives the message.

Once a message is received by a consumer, SQS doesn’t automatically delete the message. Because there’s no way for SQS to guarantee that the message has been received by the consumer. The message might get lost in transit or the consumer can fail while processing the message.

So the consumer must delete the message from the queue after receiving and processing it.

While a consumer is processing a message in the queue, SQS temporary hides the message from other consumers. This is done by setting a visibility timeout on the message, a period of time during which SQS prevents other consumers from receiving and processing the message.

The default visibility timeout for a message is <code>30</code> seconds.

![](https://res.cloudinary.com/boo-it/image/upload/v1681632402/aws/sqs_visibility.png)

If the message is not processed within the visibility timeout, it will be processed twice. If visibility timeout is high, and consumer crashes, re-processing will take time; if visibility timeout is low, we may get duplicates.


**Dead Letter Queue**
Dead letter queue is a very useful feature in message queuing systems. By using this feature, we can automatically transfer messages, that have exceeded the maximum number of receiving message, to the dead letter queue.

We have a configuration <code>Maximum Receives</code> means the maximum number of retries effectively if the number of retries exceed <code>Maximum Receives</code> value then the message will be sent to <code>Dead Letter Queue</code>

<p align="center">
  <img src="https://res.cloudinary.com/boo-it/image/upload/v1681632951/aws/sqs_dead_letter_queue.png" />
</p>

**Delay Queue**
- Can be configured via <code>Delivery delay</code> configuration
- Delay a message (consumer don't see it immediately) up to 15 minutes
- Default is <code>0</code>
- Can override the default on send using the <code>DelaySeconds</code> in parameter

**Long pooling**
- When a consumer requests messages from SQS, it can optionally wait for messages to arrive if there are none in the queue (this is long pooling).
- The wait time can be between 1 to 20 seconds
- Can configure in queue level via Receive message wait time
- Can enable in API level using WaitTimeSeconds

**FIFO queue**
FIFO (First In First Out) queues have essentially the same features as standard queues, but provide the added benefits of supporting ordering and exactly-once processing and ensure that the order in which messages are sent and received is strictly preserved.

![](https://res.cloudinary.com/boo-it/image/upload/v1681649119/aws/sqs_fifo.png)

- Limited throughput: 300 msg/s without batching, 3000 msg/s with batching
- Exactly once
- Ordering

**Deduplication**

- De-duplication interval is 5 minutes
- Two de-duplication method:
- Content-based de-duplication (SHA-256 hash of the message body)
- Explicitly provide a message

![](https://res.cloudinary.com/boo-it/image/upload/v1681649653/aws/sqs_hash.png)

**Message Group ID**

The tag indicates whether or not a message belongs to a particular message group. Messages from the same message group are always handled one by one, in the sequence in which they were received (however, messages that belong to different message groups might be processed out of order).

![](https://res.cloudinary.com/boo-it/image/upload/v1681649648/aws/sqs_mess.png)

**SNS**
- Pub/sub pattern
- The producer only sends message to one SNS topic
- Many subscribers can listen to that topic
- Up to 12,500,000 subscribers per topic
- 100,000 topics limit

**SNS - How to publish**
- Topic Publish (using the SDK)
    - Create a topic
    - Creae a subscription (or many)
    - Publish to the topic

- Direct Publish (for mobile apps SDK)
    - Create a platform application
    - Create a platform endpoint
    - Publish to the platform endpoint
    - Works with Google GCM, Apple APNS, Amazon ADM...

**Fan Out Pattern**

![](https://res.cloudinary.com/boo-it/image/upload/v1681787733/aws/sns_fanout.png)

**SNS - FIFO**
- Similar features as FIFO queue
- Can only have SQS FIFO queues as subscribers
- Limited throughput (same as FIFO queue)

**SNS - Message filtering**
JSON policy used to filter messages sent to SNS topic's subscriptions

![](https://res.cloudinary.com/boo-it/image/upload/v1681787864/aws/sns_message_filrering.png)

**SNS & SQS - Fan Out Pattern**

![](https://res.cloudinary.com/boo-it/image/upload/v1681787733/aws/sns_fanout.png)

- Push once in SNS, receive in all SQS queues that are subscribers
- Fully decoupled (hoàn toàn tách rời), no data loss
- SQS allows for: data persistence (bền vững), delayed processing and retries of work
- Ability to add more SQS subscribers over time
- Make sure your SQS queue access policy allows for SNS to write

**Kinesis**
Đây là một bộ công cụ của dịch vụ AWS (Amazon Web Services) giúp thu thập, xử lý và phân tích dữ liệu theo thời gian thực. Có thể sử dụng nó để thu thập dữ liệu thời gian thực như: các log ứng dụng, dữ liệu đo lường, luồng nhấp chuột trên trang web, dữ liệu telematics của thiết bị IoT...

- Kinesis Data Streams: cho phép bắt, xử lý và lưu trữ các dòng dữ liệu theo thời gian thực.
- Kinesis Data Firehose: dùng để nạp các dòng dữ liệu theo thời gian thực vào kho dữ liệu trên AWS.
- Kinesis Data Analytics: cho phép phân tích các dòng dữ liệu theo thời gian thực bằng SQL hoặc Apache Flink.
- Kinesis Video Streams: dùng để bắt, xử lý và lưu trữ các luồng video theo thời gian thực.

**SQS & SNS & Kinesis**

![](https://res.cloudinary.com/boo-it/image/upload/v1682237799/aws/go2i6ywwnpcf3ohycphh.png)

#### AWS Serverless
#### Lamda
- Là dịch vụ của AWS cung cấp khả năng tính toán dưới dạng Function Serverless 
- Không cần phải quản lí Servers
- Thời gian thực thi 1 function bị giới hạn (up to 15p)
- Thực thi khi có yêu cầu (Run on-demand)

**Lợi thế**
- Chi phí (Cost) được tính theo những gì thực sự sử dụng:
  - Trả phí cho số lượng Requests + thời gian thực thi function (tính theo đơn vị GB-second of Compute)
- Dễ dàng tích hợp với các dịch vụ khác của AWS
- Hỗ trợ nhiều ngôn ngữ lập trình khác nhau (Go, Python, Java, Ruby, etc)
- Hỗ trợ Ram lên tới 10Gb cho Function

**Limit**

![](https://res.cloudinary.com/boo-it/image/upload/v1684571143/aws/lamda_limit.png)

**Lamda@Edge**
- Được deploy tại Edge Location cùng với CloudFront
- Sử dụng để chạy các Functions gần với người dùng cuối (End User)
- Dùng để filtering/modification/customize yêu cầu từ người dùng tại Edge Location

**Changes req/res**
- Sử dụng Lambda@Edge để thay đổi CloudFront Request/Response
- Sau khi nhận Request từ phía Client
- Trước khi gửi Request tới Origin
- Sau khi nhận Response từ Origin
- Trước khi gửi trả Response cho phía Client

![](https://res.cloudinary.com/boo-it/image/upload/v1684571559/aws/lamda_edge.png)

**Usecases**
- Giảm thiểu Bot Attack tại Edge Locatin (Bot Attack mitigation)
- Xác thực và phân quyền cho User tại Edge
- Website Security and Privacy
- User Tracking and Analytics...

**API Gateway**

#### SAM
- SAM stands for Serverless Application Model. 
- Là tính năng mở rộng của CloudFormation cho phép triển khai ứng dụng Serverless
- Hỗ trợ triển khai API gateway, DynamoDB, Lambda functions
- Chạy ứng dụng Serverless ngay trên Docker tại máy Local
- Đóng gói và triển khai sử dụng CodeDeploy

#### Encryption
Mã hóa (encryption) là quá trình chuyển đổi thông tin từ dạng rõ (plaintext) thành dạng không đọc được (ciphertext) để bảo vệ dữ liệu khỏi việc truy cập trái phép. Quá trình mã hóa thường sử dụng một thuật toán mã hóa cùng với một khóa (key) để biến đổi dữ liệu ban đầu thành một định dạng mà chỉ có người có khóa giải mã (decryption key) mới có thể đọc được.

**Encryption in flight (SSL)**, còn được gọi là mã hóa trong quá trình truyền (SSL), áp dụng mã hóa cho dữ liệu trong quá trình truyền từ một điểm đến điểm khác trên mạng. SSL (Secure Sockets Layer) hoặc phiên bản tiếp theo của nó là TLS (Transport Layer Security) là một giao thức bảo mật được sử dụng để thiết lập kênh liên lạc an toàn giữa hai thiết bị hoặc ứng dụng trên mạng.

- Khi một kết nối SSL/TLS được thiết lập giữa máy tính của người gửi và máy tính của người nhận, dữ liệu được mã hóa trước khi được gửi qua mạng. Quá trình này đảm bảo rằng bất kỳ ai nghe lén trên đường truyền cũng không thể đọc được nội dung của thông tin. Dữ liệu chỉ được giải mã khi nó đến đích và đúng khóa giải mã được sử dụng.

- Encryption in flight (SSL/TLS) là một phương thức quan trọng để bảo vệ dữ liệu trong quá trình truyền trên mạng. Nó đảm bảo tính toàn vẹn, bảo mật và sự riêng tư của thông tin trong khi nó đang được truyền từ nguồn đến đích.

![](https://res.cloudinary.com/boo-it/image/upload/v1685196467/aws/encryp_ssl.png)

**Server side encryption at rest**
- Data is encrypted after being received by the server
- Data is decrypted before being sent
- It is stored in an encrypted form thanks to a key (usually a data key) 
- The encryption / decryption keys must be managed somewhere and the server must have access to it

![](https://res.cloudinary.com/boo-it/image/upload/v1685196774/aws/encryp_sv_rest.png)

**Client side encryption**
- Data is encrypted by the client and never decrypted by the server
- Data will be decrypted by a receiving client
- The server should not be able to decrypt the data
- Could leverage Envelope Encryption

Envelope Encryption (Mã hóa bì) là một phương pháp mã hóa dữ liệu trong đó sử dụng hai cấp khóa để bảo vệ tính bí mật của dữ liệu. Phương pháp này được sử dụng trong các hệ thống và ứng dụng lưu trữ dữ liệu nhạy cảm hoặc yêu cầu mức độ bảo mật cao.

Cơ bản, trong Envelope Encryption, dữ liệu được mã hóa bằng một khóa mã hóa dữ liệu chính (data encryption key - DEK). DEK có thể là một khóa ngẫu nhiên được tạo ra mỗi khi dữ liệu cần được mã hóa hoặc một khóa được tạo từ quá trình khác. Sau khi dữ liệu được mã hóa bằng DEK, DEK lại được mã hóa bằng một khóa mã hóa khóa (key encryption key - KEK).

Khóa mã hóa khóa (KEK) là một khóa bảo mật được lưu trữ hoặc quản lý bởi một phần mềm hoặc hệ thống an ninh. KEK có thể được tạo ra bằng cách sử dụng mật khẩu người dùng hoặc khóa bí mật khác. Với KEK đã mã hóa DEK, dữ liệu và DEK có thể được lưu trữ hoặc truyền đi một cách an toàn hơn.

Khi dữ liệu cần được giải mã, KEK được sử dụng để giải mã DEK, sau đó DEK được sử dụng để giải mã dữ liệu. Quá trình giải mã chỉ diễn ra khi dữ liệu đến đúng đích cuối cùng và trên một thiết bị tin cậy.

Envelope Encryption cung cấp một lớp bảo mật bổ sung cho dữ liệu, cho phép quản lý khóa mã hóa và dữ liệu riêng lẻ một cách hiệu quả. Bằng cách sử dụng hai cấp khóa và phân chia quyền truy cập, Envelope Encryption giúp đảm bảo rằng ngay cả khi một khóa mã hóa bị tiết lộ, dữ liệu vẫn được bảo vệ một cách an toàn.

![](https://res.cloudinary.com/boo-it/image/upload/v1685197038/aws/encryp_client.png)

#### KMS (Key Management Service)
KMS là viết tắt của Key Management Service (Dịch vụ Quản lý Khóa). KMS là một dịch vụ quản lý khóa tiện ích trong AWS, cho phép bạn tạo và kiểm soát việc sử dụng khóa mã hóa trong các dịch vụ khác nhau.

- KMS cung cấp khả năng tạo khóa mã hóa, lưu trữ khóa một cách an toàn và quản lý việc sử dụng khóa trong AWS. Bằng cách sử dụng KMS, bạn có thể mã hóa dữ liệu trong các dịch vụ như Amazon S3, Amazon EBS, Amazon RDS, Amazon Redshift và nhiều dịch vụ khác của AWS.

- KMS cũng hỗ trợ quản lý khóa đối xứng và khóa bất đối xứng. Bạn có thể sử dụng khóa của KMS để mã hóa dữ liệu trong AWS hoặc có thể xuất khóa và sử dụng nó bên ngoài AWS.

- KMS giúp bảo vệ dữ liệu của bạn bằng cách cho phép kiểm soát và quản lý quyền truy cập vào khóa mã hóa. Bạn có thể quản lý và theo dõi các hoạt động sử dụng khóa bằng cách sử dụng các công cụ và API được cung cấp bởi KMS.

*Tóm lại, KMS trong AWS là một dịch vụ quản lý khóa mạnh mẽ và tiện ích, giúp bạn bảo vệ và quản lý việc mã hóa dữ liệu trong các dịch vụ AWS.*