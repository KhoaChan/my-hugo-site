---
title: "Week 2 Worklog"
date: 2026-04-24
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---



## I. Overview

Week 2 was the stage where I started to explore the core infrastructure components of AWS in greater depth. While the first week mainly focused on becoming familiar with the internship environment, understanding AWS at a general level, and setting up the initial tools, this week allowed me to directly practice designing, configuring, and testing several important services within the cloud computing ecosystem.

The practical activities this week covered multiple AWS service groups, including AWS Global Infrastructure, identity and access management with IAM, virtual network design with VPC, virtual server deployment with EC2, static website storage with S3, and relational database creation with Amazon RDS.

Through these labs, I gained a better understanding of how AWS services connect with each other to form a complete system. In addition to creating resources, I also paid attention to important factors such as account security, access control, network configuration, cost management, and cleaning up resources after practice.

## II. Weekly Objectives

In Week 2, I set the following main objectives:

* Learn how AWS organizes its global infrastructure through Regions, Availability Zones, and Edge Locations.
* Strengthen my knowledge of account security and access management using IAM.
* Create a separate IAM Admin User to avoid using the Root account for daily operations.
* Design an isolated virtual network using Amazon VPC.
* Configure Public Subnet, Internet Gateway, and Route Table to control Internet traffic flow.
* Deploy a Linux virtual server using Amazon EC2.
* Use User Data to automatically install Apache Web Server when launching EC2.
* Create an S3 Bucket and configure Static Website Hosting.
* Deploy a MySQL database using Amazon RDS.
* Identify common configuration errors and learn how to troubleshoot them.
* Control costs by selecting configurations suitable for Free Tier/Sandbox and deleting unnecessary resources after completing the labs.

## III. Weekly Activity Log

| Time      | Activity Category                  | Tasks Performed                                                                                                                                                                                     | Results/Evidence Achieved                                                    |
| --------- | ---------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| Day 1 - 2 | Global infrastructure and security | Studied AWS global infrastructure, including Region, Availability Zone, and Edge Location. Reviewed account security, enabled MFA, and set up an IAM Admin User to use instead of the Root account. | Root account was protected with MFA, and IAM Admin User was ready to use     |
| Day 3     | Identity management with IAM       | Created IAM User, IAM Group, and assigned permissions through Policy. Studied how IAM Policy works and applied the Least Privilege principle.                                                       | Understood how to manage access and user permissions on AWS                  |
| Day 4     | Network design with Amazon VPC     | Created a custom VPC, divided a Public Subnet, configured Internet Gateway, and edited Route Table so that the subnet could connect to the Internet.                                                | Completed a basic virtual network model for EC2 deployment                   |
| Day 5     | EC2 server deployment              | Launched an EC2 instance running Amazon Linux, configured Key Pair, Security Group, and User Data to automatically install Apache Web Server.                                                       | EC2 server worked successfully and was accessible through Public IP          |
| Day 6     | Static website hosting with S3     | Created an S3 Bucket, disabled Block Public Access for hosting purposes, configured Static Website Hosting and Bucket Policy, then uploaded the `index.html` file.                                  | Static website worked through the S3 Website Endpoint                        |
| Day 7     | RDS database and summary           | Created an RDS MySQL instance, selected a Free Tier-friendly configuration, checked the database status, and documented the errors encountered during practice.                                     | Database changed to Available status, and Week 2 documentation was completed |

## IV. Detailed Technical Practice Through Labs

---

# LAB 1: Introduction to AWS Global Infrastructure

## 1. Overview

Before deploying specific services on AWS, I needed to understand how AWS builds and distributes its physical infrastructure globally. This is an important foundational knowledge because every service deployment decision is related to resource location, latency, availability, and system fault tolerance.

This lab helped me visualize how AWS organizes its infrastructure into multiple layers, from the global level to each Region, Availability Zone, and Edge Location. As a result, I understood that when designing a cloud system, it is not enough to simply “create a working service”; it is also necessary to consider deployment location and redundancy.

## 2. Knowledge Learned

### AWS Region

An AWS Region is an independent geographical area where AWS places groups of data centers. Each Region can serve users in a specific area and is usually selected based on factors such as user location, latency, legal requirements, and service cost.

During the practice process, I prioritized using the `us-east-1` Region because it is a popular Region, supports many AWS services, and is suitable for learning and lab practice.

### Availability Zone

Availability Zones are available zones located inside a Region. Each AZ consists of one or more physical data centers with separate power, networking, and cooling systems. AZs within the same Region are connected through high-speed and low-latency networks.

Through this section, I understood that if the entire system is deployed in only one AZ, the application may be interrupted when that AZ encounters an issue. Therefore, in real-world systems, resources are often distributed across multiple AZs to improve availability.

### Edge Location

Edge Locations are AWS points of presence used to bring content closer to users, especially when using services such as Amazon CloudFront. This helps reduce latency and improve access speed for end users.

## 3. Results Achieved

After this lab, I understood the basic layered structure of AWS Global Infrastructure:

**AWS Global Infrastructure → Region → Availability Zone → Data Center → Edge Location**

I also understood that choosing the right Region and designing resources across multiple AZs are important parts of building a system with high availability, scalability, and better fault tolerance.

---

# LAB 2: AWS IAM Access Control - Establishing a Security Foundation

## 1. Overview

Security is one of the most important factors when working in a cloud environment. In AWS, IAM plays the role of managing identities and access permissions for users, user groups, services, and applications.

In this lab, I focused on establishing a safer access environment. Instead of using the Root account for daily tasks, I created a separate IAM Admin User for operations and enabled MFA to strengthen security.

## 2. Knowledge Learned

### Principle of Least Privilege

The Principle of Least Privilege requires granting only the necessary permissions to each user or service. This helps reduce risks if account credentials are exposed or if a user performs an incorrect operation.

### Root Account

The Root account has the highest level of permissions in AWS. This account can change billing information, delete important resources, or close the AWS account. Therefore, the Root account should not be used frequently.

I understood that the Root account should only be used in special cases, while normal practice and administration tasks should be performed through an IAM User or IAM Role.

### IAM Policy

An IAM Policy is a JSON document that defines access permissions in AWS. A policy usually includes the following main components:

* **Effect:** Defines whether the permission is allowed or denied, such as `Allow` or `Deny`.
* **Action:** Specifies the action that is allowed to be performed, such as `ec2:RunInstances` or `s3:CreateBucket`.
* **Resource:** Defines the resource to which the policy applies.
* **Condition:** Adds extra conditions if access needs to be limited by IP address, time, or a specific context.

## 3. Implementation Process

During the practice process, I performed the following steps:

* Logged in to AWS Console using the Root account.
* Accessed the IAM service.
* Went to Users and selected Create user.
* Created an IAM User named `Admin-Khoa`.
* Created an IAM Group named `Admins`.
* Attached the `AdministratorAccess` policy to the `Admins` group.
* Added user `Admin-Khoa` to the `Admins` group.
* Logged out of the Root account and logged in again using the new IAM User.
* Enabled Virtual MFA for both the Root account and IAM User to improve security.
* Checked the IAM Dashboard to verify the basic security status.

## 4. Screenshots/Evidence

![VPC Create Screenshot](/my-hugo-site/images/1-Workblog/Week-2/vpc-create.png)

![Screenshot of IAM Group Admins](/my-hugo-site/images/1-Workblog/Week-2/image%20copy.png)

## 5. Results Achieved

After this lab, I successfully created a separate IAM Admin User for practice. Separating daily operations from the Root account makes the account more secure and aligns better with professional cloud operation principles.

---

# LAB 3: Amazon VPC - Designing an Isolated Virtual Network

## 1. Overview

Amazon VPC is a service that allows users to create a virtual private network on AWS. It is an important networking foundation for deploying resources such as EC2, RDS, Load Balancer, and many other services in a controlled environment.

In this lab, I practiced creating a custom VPC, dividing subnets, attaching an Internet Gateway, and configuring the Route Table so that resources in the public subnet could access the Internet.

## 2. Knowledge Learned

### CIDR Block

When creating a VPC, it is necessary to define the internal IP address range using a CIDR block. I used the `10.0.0.0/16` range to create a private network space. This range can be divided into multiple subnets for different purposes.

### Public Subnet

A subnet is considered public when its Route Table has a route to an Internet Gateway. A public subnet is usually used for resources that need to receive access from the Internet, such as public EC2 instances or Load Balancers.

### Internet Gateway

An Internet Gateway is the connection point between a VPC and the Internet. For EC2 in a public subnet to access the Internet or be accessed from the Internet, the VPC must be attached to an Internet Gateway and the Route Table must have the proper route.

### Route Table

A Route Table determines how traffic moves inside a VPC. If the Route Table does not have a `0.0.0.0/0` route pointing to an Internet Gateway, resources inside the subnet cannot communicate with the Internet.

## 3. Implementation Process

The steps performed:

* Accessed AWS Console and opened the VPC service.
* Selected Create VPC.
* Created a VPC named `MyLabVPC`.
* Entered CIDR block `10.0.0.0/16`.
* Created a subnet named `Public-Subnet-1A`.
* Selected Availability Zone `us-east-1a`.
* Assigned CIDR block `10.0.1.0/24` to the subnet.
* Created an Internet Gateway named `MyLab-IGW`.
* Attached the Internet Gateway to `MyLabVPC`.
* Went to Route Tables and selected the route table of the VPC.
* Added a new route:

  * Destination: `0.0.0.0/0`
  * Target: Internet Gateway `MyLab-IGW`
* Saved the configuration and checked the network status again.

## 4. Screenshots/Evidence

![Screenshot of IAM Group Admins](/my-hugo-site/images/1-Workblog/Week-2/image%20copy%202.png)

![Subnet Configuration Screenshot](/my-hugo-site/images/1-Workblog/Week-2/image%20copy%203.png)

## 5. Results Achieved

After this lab, I successfully built a basic VPC network consisting of a public subnet and an Internet Gateway. This served as the foundation for deploying the EC2 instance in the next lab.

---

# LAB 4: Amazon EC2 - Deploying a Web Server

## 1. Overview

After setting up the VPC and public subnet, I continued deploying a virtual server using Amazon EC2. The goal of this lab was to launch an EC2 instance running Amazon Linux, configure Security Group, and use User Data to automatically install Apache Web Server.

Using User Data helped me understand how to automate software installation when a server is launched, instead of having to SSH into the server and install everything manually.

## 2. Knowledge Learned

### Amazon Linux 2023

Amazon Linux 2023 is a Linux operating system provided and optimized by AWS for the cloud environment. This operating system is suitable for basic EC2 lab practice.

### Instance Type

Instance Type determines the hardware configuration of EC2, such as CPU, RAM, and network performance. In this lab, I selected a small configuration such as `t2.micro` or `t3.micro` to stay within the Free Tier range.

### Security Group

A Security Group acts as a virtual firewall for an EC2 instance. I configured the required inbound rules:

* SSH port `22`: only allows my personal IP address to access the server for administration.
* HTTP port `80`: allows users to access the website through a browser.

### User Data

User Data is a script that runs automatically when EC2 starts for the first time. I used User Data to install Apache Web Server and create an `index.html` file.

## 3. Implementation Process

The steps performed:

* Accessed EC2 Console.
* Selected Launch instance.
* Named the server `MyWebServer`.
* Selected Amazon Linux 2023 AMI.
* Selected instance type `t2.micro` or `t3.micro`.
* Created a Key Pair named `my-key`.
* Selected VPC `MyLabVPC`.
* Selected subnet `Public-Subnet-1A`.
* Enabled Auto-assign Public IP.
* Created a Security Group named `Web-SG`.
* Opened port `22` for SSH from my personal IP address.
* Opened port `80` for HTTP from all IPv4 addresses.
* Added User Data to automatically install Apache.
* Selected Launch instance and waited for the instance to change to Running status.
* Accessed the Public IP through a browser to check the website.

User Data script used:

```bash
#!/bin/bash
sudo dnf update -y
sudo dnf install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
echo "<h1>Welcome to Khoa's AWS Web Server</h1>" > /var/www/html/index.html
```

## 4. Screenshots/Evidence

![EC2 Instance Configuration](/my-hugo-site/images/1-Workblog/Week-2/image%20copy%204.png)

![EC2 Security Group Configuration](/my-hugo-site/images/1-Workblog/Week-2/image%20copy%205.png)

![EC2 Running Instance](/my-hugo-site/images/1-Workblog/Week-2/image%20copy%206.png)

![EC2 Public IP Website Test](/my-hugo-site/images/1-Workblog/Week-2/image%20copy%207.png)

![EC2 Public IP Website Test](/my-hugo-site/images/1-Workblog/Week-2/image%20copy%208.png)

## 5. Results Achieved

After EC2 ran successfully, I accessed the Public IP through a browser and the website displayed the content configured in the `index.html` file. This proved that EC2, Security Group, User Data, and Apache Web Server were working correctly.

---

# LAB 5: Amazon S3 - Static Website Hosting

## 1. Overview

Amazon S3 is an object storage service provided by AWS. In addition to storing files, images, or backups, S3 can also be used to host a static website.

In this lab, I created an S3 Bucket, enabled Static Website Hosting, configured a Bucket Policy to allow public file reading, and uploaded the `index.html` file to test the website.

## 2. Knowledge Learned

### Object Storage

S3 stores data in the form of objects. Each object includes file content, metadata, and a unique key. This storage model is different from the traditional folder-based model on a computer.

### Bucket Name

An S3 Bucket name must be globally unique across the entire AWS system. Therefore, when creating a bucket, I needed to choose a unique name to avoid conflicts with buckets from other accounts.

### Block Public Access

By default, S3 blocks public access to protect data. When using S3 to host a static website, public access must be configured carefully and in a controlled way.

### Bucket Policy

A Bucket Policy is a bucket-level permission policy written in JSON. For a static website to be publicly accessible, the policy needs to allow the `s3:GetObject` action.

## 3. Implementation Process

The steps performed:

* Accessed the S3 service.
* Selected Create bucket.
* Named the bucket, for example `khoa-static-website-2026`.
* Selected Region `us-east-1`.
* Disabled Block Public Access for the bucket.
* Confirmed public access configuration.
* Created the bucket.
* Went to the Properties tab.
* Enabled Static Website Hosting.
* Selected Host a static website.
* Entered `index.html` as the Index document.
* Saved the configuration.
* Went to the Permissions tab.
* Selected Bucket Policy and added a policy to allow public read access.
* Uploaded the `index.html` file.
* Accessed the Bucket Website Endpoint to check the result.

Sample Bucket Policy:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::khoa-static-website-2026/*"
    }
  ]
}
```

## 4. Screenshots/Evidence

![S3 Static Website Hosting](/my-hugo-site/images/1-Workblog/Week-2/image%20copy%209.png)

![S3 Bucket Policy](/my-hugo-site/images/1-Workblog/Week-2/image%20copy%2010.png)

![S3 Website Endpoint Test](/my-hugo-site/images/1-Workblog/Week-2/image%20copy%2011.png)

![RDS MySQL Engine Selection](/my-hugo-site/images/1-Workblog/Week-2/image%20copy%2012.png)


## 5. Results Achieved

After completing the configuration, the static website could be accessed through the endpoint provided by S3. Through this lab, I understood that S3 is a simple and effective solution for deploying static websites without managing servers.

---

# LAB 6: Amazon RDS - Creating a Relational Database

## 1. Overview

Amazon RDS is a managed relational database service provided by AWS. Instead of manually installing MySQL on EC2 and managing backups, updates, or monitoring by myself, RDS automates many database administration tasks.

In this lab, I practiced creating an RDS MySQL instance, selecting a configuration suitable for learning purposes, and checking the operating status of the database.

## 2. Knowledge Learned

### Managed Database Service

RDS reduces database administration work such as installation, backup, system updates, and monitoring. As a result, developers can focus more on data design and query optimization.

### Database Engine

RDS supports many database management systems such as MySQL, PostgreSQL, MariaDB, Oracle, and SQL Server. In this lab, I chose MySQL because it is a popular database engine and suitable for many web applications.

### DB Instance

A DB Instance is the database server provisioned by AWS. When creating a database, it is necessary to select the appropriate instance type, storage, and network settings to avoid unnecessary costs.

### Endpoint

After the database is created successfully, AWS provides a DNS-style endpoint. A backend application can use this endpoint to connect to the database instead of using a fixed IP address.

## 3. Implementation Process

The steps performed:

* Accessed the Amazon RDS service.
* Selected Create database.
* Selected Standard create.
* Selected MySQL as the database engine.
* Selected a template suitable for the learning environment.
* Set the DB instance identifier as `my-khoa-db`.
* Kept the master username as `admin`.
* Selected Credentials management as Self managed.
* Entered a suitable admin password, for example `KhoaDB.2026`.
* Selected instance type `db.t3.micro`.
* Selected General Purpose SSD `gp3` as the storage type.
* Set Allocated storage to `20 GiB`.
* Checked the network and security settings again.
* Selected Create database.
* Waited for the database to change to Available status.
* Recorded the endpoint for future labs.

## 4. Screenshots/Evidence

![RDS Instance Configuration](/my-hugo-site/images/1-Workblog/Week-2/image%20copy%2013.png)

![RDS Storage Configuration](/my-hugo-site/images/1-Workblog/Week-2/image%20copy%2014.png)

![RDS Available Status and Endpoint](/my-hugo-site/images/1-Workblog/Week-2/image%20copy%2015.png)

## 5. Results Achieved

After the configuration was completed, the RDS instance changed to **Available** status. I obtained the connection endpoint and understood how a backend application can use this endpoint to query data.

Through this lab, I realized that the database is a component that must be carefully protected. In a real environment, the database should not be publicly exposed without control. Instead, it should be placed in a private subnet and only allow access from authorized backend components.

---

## V. Challenges Encountered and Solutions

During the practice labs this week, I encountered several real-world issues related to network configuration, access permissions, and cost. These issues helped me understand more clearly how AWS operates.

### 1. Cost Control When Creating Resources

When creating a VPC or network components, AWS may suggest adding a NAT Gateway. After researching, I realized that NAT Gateway can generate hourly charges. Since my goal was to practice within the Free Tier or Sandbox environment, I needed to review carefully before creating resources.

My solution was to create only the components required for the lab, avoid enabling unnecessary services, and delete resources after completing the practice.

### 2. Unable to Access EC2 Using Public IP

During EC2 deployment, there was a time when I could not access the website using the Public IP. I checked each configuration layer and found that the cause could come from the subnet, route table, public IP, security group, or Apache not running.

I solved this by:

* Checking whether EC2 was located in a public subnet.
* Checking whether the instance had been assigned a Public IPv4 address.
* Ensuring that the Route Table had a `0.0.0.0/0` route pointing to the Internet Gateway.
* Checking whether the Security Group opened port `80`.
* Checking whether Apache had been installed and started successfully.

After reviewing each component, the website became accessible normally.

### 3. Error When Configuring RDS

During the RDS creation process, I had to pay close attention to parameters such as password, instance type, storage type, and storage size. If the wrong configuration was selected, the database might not fit within the Free Tier range or could generate additional costs.

I solved this by selecting a small configuration such as `db.t3.micro`, using `gp3` storage with `20 GiB`, and avoiding characters that could cause password-related errors.

### 4. Distinguishing Security Group and Network ACL

While learning VPC, I realized that Security Group and Network ACL are both related to network security, but they operate at different layers.

* Security Group works at the instance level and is stateful.
* Network ACL works at the subnet level and is stateless.

This helped me understand that when troubleshooting network connection issues, it is necessary to check the correct security layer instead of looking at only one configuration.

## VI. Results Achieved in Week 2

After completing Week 2, I achieved the following results:

* Understood the structure of AWS Global Infrastructure.
* Distinguished between Region, Availability Zone, and Edge Location.
* Understood the role of IAM in identity and access management.
* Created IAM Admin User `Admin-Khoa`.
* Enabled MFA to strengthen account security.
* Understood the Least Privilege principle when assigning permissions.
* Created an Amazon VPC with a custom CIDR block.
* Configured Public Subnet, Internet Gateway, and Route Table.
* Successfully deployed an EC2 instance inside the VPC.
* Configured Security Group for EC2.
* Used User Data to automatically install Apache Web Server.
* Successfully accessed the website running on EC2 through Public IP.
* Created and configured S3 Bucket `khoa-static-website-2026`.
* Enabled Static Website Hosting for S3.
* Configured Bucket Policy to allow public file reading.
* Uploaded `index.html` and tested the website endpoint.
* Created Amazon RDS MySQL instance `my-khoa-db`.
* Understood how to obtain and use the database endpoint.
* Identified cost risks when creating AWS resources.
* Documented errors encountered and solutions for the internship portfolio.

## VII. Reflection After Week 2

Week 2 helped me realize that building cloud infrastructure is not simply about creating separate services. AWS components are closely connected to one another. An EC2 instance needs a proper VPC, subnet, Internet Gateway, Route Table, Public IP, and Security Group to be accessible from the Internet. A static website on S3 needs Static Website Hosting, Bucket Policy, and correct public access permissions. An RDS database needs careful network and security group configuration to be used safely.

The most important lesson I learned is that cloud engineering requires system thinking. Practitioners need to understand data flow, permission design, network design, cost control, and troubleshooting by infrastructure layer.

After this week, I feel more confident with foundational services such as IAM, VPC, EC2, S3, and RDS. This is an important preparation step before I continue learning more advanced topics such as Auto Scaling, Load Balancer, CloudWatch, and complete application deployment in the following weeks.

## VIII. Plan for Next Week

In the next week, I will continue expanding my knowledge into more advanced topics related to scalability, load balancing, and system monitoring.

The planned topics include:

* Learning Elastic Load Balancing.
* Practicing Target Group and Application Load Balancer.
* Configuring Auto Scaling Group for EC2.
* Learning CloudWatch to monitor metrics and logs.
* Deploying a simple application on EC2.
* Connecting the application to Amazon RDS.
* Checking how to optimize costs when running multiple resources at the same time.
* Continuing to update the Hugo portfolio and add practice screenshots/evidence.
