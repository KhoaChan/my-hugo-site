---
title: "Week 3 Worklog"
date: 2026-05-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---



## I. Overview

Week 3 marked an important step forward in my internship journey in the **First Cloud AI Journey – Workforce Bootcamp 2026** program. During this week, I moved from building basic single-layer infrastructure to designing more advanced, multi-layer cloud networking systems and hybrid connectivity models on AWS.

The learning and practice activities covered several major labs, including AWS Budgets and CloudWatch monitoring, Route 53 Private Hosted Zone, Route 53 Resolver for Hybrid DNS, AWS CLI infrastructure management, Advanced VPC Topology, and Elastic Load Balancing.

Through these labs, I gained a deeper understanding of how enterprise cloud infrastructure is designed, monitored, secured, and operated. Instead of only creating individual services, I started to understand how networking, DNS, routing, monitoring, automation, and load balancing work together to build a more reliable and production-ready system.

---

## II. Strategic Objectives for the Week

The main objectives for Week 3 were:

* Design a more advanced multi-layer network architecture using Amazon VPC.
* Separate public, application, and database layers to improve security.
* Configure monitoring and cost control using AWS Budgets and Amazon CloudWatch.
* Set up internal DNS management using Route 53 Private Hosted Zone.
* Understand the role of Route 53 Resolver in hybrid cloud DNS architectures.
* Practice managing AWS resources through AWS CLI instead of relying only on the AWS Console.
* Use JMESPath query filters to extract useful infrastructure information from AWS CLI output.
* Deploy Elastic Load Balancing to distribute traffic and reduce single points of failure.
* Strengthen cost management discipline by reviewing and deleting unnecessary resources after labs.

---

## III. Weekly Activity Log

| Time      | Activity Category                  | Detailed Tasks Performed                                                                                       | Results / Evidence Achieved                                                         |
| --------- | ---------------------------------- | -------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- |
| Day 1     | Monitoring and Cost Control        | Configured AWS Budgets, created cost thresholds, and set up CloudWatch Dashboard and Alarm for EC2 monitoring. | Budget tracking and CPU monitoring system were successfully configured.             |
| Day 2     | Internal DNS Management            | Created Route 53 Private Hosted Zone and associated it with the custom VPC.                                    | Internal domain resolution environment was prepared.                                |
| Day 3 - 4 | Hybrid DNS Connectivity            | Configured Route 53 Resolver Inbound and Outbound Endpoints across multiple Availability Zones.                | Hybrid DNS resolution model between cloud and internal networks was established.    |
| Day 4     | AWS CLI Management                 | Used AWS CLI and JMESPath query filters to retrieve EC2 infrastructure information in table format.            | AWS resources were successfully managed through command line.                       |
| Day 5     | Advanced VPC Topology              | Designed additional private database subnet and separate route table for the database layer.                   | Database subnet was logically isolated from direct Internet access.                 |
| Day 6     | Elastic Load Balancing             | Created an Application Load Balancer, Target Group, and registered EC2 instances for traffic distribution.     | The application architecture achieved better availability and traffic distribution. |
| Day 7     | Documentation and Portfolio Update | Documented the technical process, summarized errors, and prepared screenshots/evidence for the Hugo portfolio. | Week 3 worklog was completed and ready for portfolio publishing.                    |

---

# IV. Detailed Technical Practice Through Labs

---

# LAB 7: Amazon CloudWatch & AWS Budgets - Setting Up Cost Control and Monitoring

## 1. Overview

In any cloud infrastructure deployment, cost control and system monitoring are extremely important. Without proper monitoring, resources may generate unexpected costs or performance issues without being detected early.

In this lab, I practiced configuring **AWS Budgets** to track cloud spending and **Amazon CloudWatch** to monitor EC2 performance metrics. This helped me understand how cloud engineers control both financial usage and technical health in an AWS environment.

## 2. Core Concepts

### AWS Budgets

AWS Budgets allows users to create custom cost limits and receive alerts when usage approaches or exceeds the configured threshold. This is an important FinOps tool that helps prevent unexpected spending, especially in learning or sandbox environments.

### Amazon CloudWatch

Amazon CloudWatch is used to collect and monitor operational metrics from AWS resources. For EC2, one of the most common metrics is `CPUUtilization`, which shows how much CPU capacity an instance is using.

### CloudWatch Alarm

A CloudWatch Alarm is used to trigger an alert when a metric crosses a defined threshold. For example, if CPU usage exceeds 80% for a certain period, the alarm can switch to the `ALARM` state and notify the administrator.

## 3. Implementation Process

The steps performed:

* Opened the AWS Budgets service.
* Created a monthly cost budget.
* Set the budget limit to `20.00 USD`.
* Configured the budget to focus on EC2-related usage.
* Added multiple alert thresholds, such as 80%, 100% forecasted cost, and 100% actual cost.
* Verified that the budget was created successfully.
* Opened Amazon CloudWatch.
* Created a dashboard named `EC2-Monitor-Dashboard`.
* Added a widget to monitor the `CPUUtilization` metric of the EC2 instance.
* Created a CloudWatch Alarm named `EC2-High-CPU-Alarm`.
* Set the alarm condition to trigger when CPU utilization exceeds 80%.
* Reviewed the alarm configuration and saved it.

## 4. Practice Evidence
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%202.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%203.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%204.png)

## 5. Results Achieved

After completing this lab, I successfully configured a cost monitoring system using AWS Budgets and a technical monitoring system using Amazon CloudWatch. I also understood how budget alerts and performance alarms help cloud engineers detect cost and resource issues earlier.

---

# LAB 8: Amazon Route 53 - Creating a Private Hosted Zone

## 1. Overview

In a secure internal network, services should not always communicate with each other using raw IP addresses because IP addresses can change over time. Instead, internal DNS names make the system easier to manage and more readable.

In this lab, I practiced creating a **Route 53 Private Hosted Zone** to manage internal domain names inside a VPC. This helped me understand how AWS supports private DNS resolution for resources that should not be exposed to the public Internet.

## 2. Core Concepts

### Private Hosted Zone

A Private Hosted Zone is a DNS zone that is only accessible inside associated VPCs. Unlike public hosted zones, private hosted zones are not available on the Internet.

### DNS Resolution

DNS resolution allows resources to translate domain names into IP addresses. In AWS, VPC DNS settings must be enabled so EC2 instances and other resources can resolve internal DNS names.

### VPC Association

A private hosted zone must be associated with one or more VPCs. Only resources inside the associated VPC can resolve records from that private hosted zone.

## 3. Implementation Process

The steps performed:

* Opened the Route 53 service.
* Selected Hosted zones.
* Created a new hosted zone.
* Entered the internal domain name `hutech.local`.
* Selected the hosted zone type as Private hosted zone.
* Associated the hosted zone with the custom VPC.
* Verified that the hosted zone was created successfully.
* Reviewed the default DNS records created by AWS.

## 4. Practice Evidence

![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%205.png)

## 5. Results Achieved

After completing this lab, I successfully created a Private Hosted Zone for internal DNS management. I understood that private DNS is useful for internal service communication because it avoids exposing domain records to the public Internet.

---

# LAB 10: Amazon Route 53 Resolver - Hybrid DNS Connectivity

## 1. Overview

In hybrid cloud architectures, cloud resources and on-premises systems often need to resolve each other’s internal domain names. This requires a DNS forwarding mechanism that can securely connect DNS queries between AWS and external networks.

In this lab, I practiced configuring **Amazon Route 53 Resolver Endpoints**, including both Inbound and Outbound Endpoints. This helped me understand how hybrid DNS resolution works between AWS VPCs and on-premises environments.

## 2. Core Concepts

### Inbound Endpoint

An Inbound Endpoint allows DNS queries from an external network, such as an on-premises data center, to be forwarded into AWS. It uses Elastic Network Interfaces inside the VPC and listens for DNS queries from outside.

### Outbound Endpoint

An Outbound Endpoint allows DNS queries from AWS resources to be forwarded to external DNS servers. This is useful when AWS resources need to resolve internal domains that exist outside AWS.

### Multi-AZ Design

For high availability, Resolver Endpoints should be deployed across at least two Availability Zones. This helps avoid a single point of failure if one AZ has an issue.

## 3. Implementation Process

The steps performed:

* Opened the Route 53 Resolver service.
* Created an Inbound Endpoint named `Hutech-Inbound-Endpoint`.
* Selected the custom VPC.
* Assigned endpoint IP addresses across multiple Availability Zones.
* Confirmed that the Inbound Endpoint was created successfully.
* Created an Outbound Endpoint named `Hutech-Outbound-Endpoint`.
* Used a similar Multi-AZ deployment model.
* Verified that the endpoint status changed to `Operational`.
* Reviewed the endpoint configuration and associated network interfaces.
* Checked the cost impact and planned cleanup after practice.

## 4. Practice Evidence

![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%206.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%207.png)

## 5. Results Achieved

After completing this lab, I understood how Route 53 Resolver supports hybrid DNS resolution between AWS and external networks. I also learned that Resolver Endpoints can generate costs, so they should be monitored and deleted after practice if no longer needed.

---

# LAB 11: Managing AWS Infrastructure Using AWS CLI

## 1. Overview

AWS CLI allows users to manage AWS resources through command-line commands instead of relying only on the AWS Console. This is important because command-line operations can be automated, repeated, and integrated into scripts.

In this lab, I practiced using AWS CLI to retrieve EC2 instance information and format the output using a JMESPath query. This helped me understand how cloud engineers extract useful information from large AWS API responses.

## 2. Core Concepts

### AWS CloudShell

AWS CloudShell is a browser-based terminal provided by AWS. It comes with AWS CLI pre-installed and automatically uses the permissions of the currently logged-in IAM user.

### AWS CLI

AWS CLI is a command-line tool that allows users to interact with AWS services by executing commands. It is useful for automation, troubleshooting, and infrastructure management.

### JMESPath Query

JMESPath is a query language used to filter JSON output. With the `--query` option, AWS CLI can return only the specific fields needed, instead of showing a large raw JSON response.

## 3. Implementation Process

The steps performed:

* Opened AWS CloudShell.
* Verified that the CLI environment was ready.
* Used AWS CLI to describe EC2 instances.
* Added a JMESPath query to filter the output.
* Extracted important fields such as Instance ID, instance state, and instance type.
* Displayed the output in table format for easier reading.

The command used:

```bash
aws ec2 describe-instances --query "Reservations[*].Instances[*].[InstanceId,State.Name,InstanceType]" --output table
```

## 4. Practice Evidence

![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%208.png)

## 5. Results Achieved

After completing this lab, I successfully retrieved EC2 instance information using AWS CLI and displayed it in a clean table format. I understood how CLI and JMESPath can help manage AWS resources more efficiently than manually checking each resource through the Console.

---

# LAB 13: Advanced VPC Topology - Multi-Tier Network Isolation

## 1. Overview

As cloud systems become more complex, a flat network design is no longer enough to protect sensitive components. A production system should separate the public layer, application layer, and database layer to improve security and control traffic flow.

In this lab, I practiced building an advanced VPC topology by creating an isolated private subnet for the database layer and associating it with a separate route table. This helped me understand how multi-tier architecture improves cloud security.

## 2. Core Concepts

### 3-Tier Architecture

A 3-tier architecture separates the system into three logical layers:

* Public/Web Tier: Receives traffic from the Internet.
* Application Tier: Processes business logic.
* Database Tier: Stores sensitive data and should be isolated from direct Internet access.

### Route Table Isolation

Route Tables control how network traffic moves inside a VPC. By using a separate route table for the database subnet and not adding a route to the Internet Gateway, the database layer can remain isolated from the public Internet.

### CIDR Planning

CIDR planning helps divide IP address ranges clearly and avoid conflicts. Proper subnet planning also makes it easier to apply Security Group and Network ACL rules later.

## 3. Implementation Process

The steps performed:

* Opened the VPC service.
* Selected the existing VPC `MyLabVPC`.
* Created a new private subnet for the database layer.
* Named the subnet `Private-DB-Subnet-1A`.
* Assigned the CIDR block `10.0.3.0/24`.
* Verified that the subnet was created successfully.
* Created a new route table named `DB-Route-Table`.
* Associated `Private-DB-Subnet-1A` with `DB-Route-Table`.
* Ensured that the route table did not include a `0.0.0.0/0` route to the Internet Gateway.
* Reviewed the subnet association to confirm network isolation.

## 4. Practice Evidence

![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%209.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%2010.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%2011.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%2012.png)

## 5. Results Achieved

After completing this lab, I successfully created an isolated database subnet and associated it with a separate route table. I understood how subnet and route table design can prevent sensitive resources from being directly exposed to the Internet.

---

# LAB 14: Elastic Load Balancing - Deploying a High Availability Traffic Distribution System

## 1. Overview

In a production environment, running an application on only one server creates a high risk of downtime. If the server fails or receives too much traffic, the entire application may become unavailable.

In this lab, I practiced deploying an **Application Load Balancer - ALB** to distribute traffic across EC2 instances. This helped me understand how Elastic Load Balancing improves high availability and reduces the risk of a single point of failure.

## 2. Core Concepts

### Application Load Balancer

An Application Load Balancer operates at Layer 7 of the OSI model. It can inspect HTTP/HTTPS traffic and route requests based on conditions such as path, host, or headers.

### Target Group

A Target Group is a logical group of resources, usually EC2 instances, that receive traffic from the load balancer. The ALB forwards user requests to healthy targets inside the target group.

### Health Check

Health Check is a mechanism used by the load balancer to monitor whether targets are healthy. If an EC2 instance becomes unhealthy, the load balancer stops sending traffic to that instance and redirects traffic to healthy instances.

## 3. Implementation Process

The steps performed:

* Opened the EC2 service.
* Went to Load Balancers.
* Created a new Application Load Balancer.
* Named the load balancer `MyWebALB`.
* Selected the scheme as Internet-facing.
* Selected IPv4 as the IP address type.
* Mapped the ALB to `MyLabVPC`.
* Selected public subnets across multiple Availability Zones.
* Created a Target Group named `Web-TG`.
* Selected HTTP protocol and port `80`.
* Configured the health check path as `/`.
* Registered existing EC2 instances into the Target Group.
* Reviewed the configuration and created the load balancer.
* Checked the ALB DNS name after creation.
* Tested access through the ALB DNS endpoint.

## 4. Practice Evidence

![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%209.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%2010.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%2011.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%2012.png)

## 5. Results Achieved

After completing this lab, I successfully configured an Application Load Balancer and Target Group. I understood how ALB distributes traffic to EC2 instances and how Health Checks help maintain application availability by detecting unhealthy targets.

---

## V. Challenges Encountered and Solutions

During Week 3, I encountered several technical and operational challenges while working with advanced AWS infrastructure components.

### 1. Hidden Cost Risk from NAT Gateway and Resolver Endpoints

When designing VPC networking and hybrid DNS components, AWS may recommend services such as NAT Gateway or Route 53 Resolver Endpoints. These services can generate hourly charges if left running.

To handle this, I carefully reviewed the required components before creating them and planned to delete unused resources after completing the labs. This helped me strengthen my cost management discipline.

### 2. Limitations When Using CloudShell

In some cases, the CloudShell environment may have region limitations or temporary access constraints. This showed me that cloud operations may not always follow a perfect path.

To solve this, I learned that engineers should be ready to switch execution environments, use a local terminal, configure AWS CLI locally, or use an alternative shell to continue the workflow.

### 3. Network Isolation Complexity

When building an advanced VPC topology, it is important to ensure that private subnets are truly isolated from public Internet access. A wrong route table association can accidentally expose sensitive resources.

To avoid this issue, I carefully checked the route table associations and confirmed that the database subnet did not have a route to the Internet Gateway.

### 4. Load Balancer Health Check Configuration

When configuring ALB and Target Group, the Health Check path must match the application response path. If the application does not respond correctly to the configured path, the target may be marked as unhealthy.

To solve this, I checked the web server configuration and ensured that the health check path `/` returned a valid HTTP response.

---

## VI. Professional Reflection

Week 3 helped me understand that cloud engineering is not only about creating AWS services one by one. It is about designing the path that data takes through the system.

A request from the Internet must pass through the Internet Gateway, follow the correct Route Table, reach the Application Load Balancer, and then be forwarded to healthy EC2 instances. At the same time, internal DNS, monitoring, budget alerts, and private network isolation must be configured correctly to support a reliable system.

The most valuable lesson I gained this week is that a cloud engineer needs to think in layers: network layer, security layer, compute layer, DNS layer, monitoring layer, and cost layer. Each layer affects the stability and security of the entire system.

This week also helped me become more aware of the importance of AWS Well-Architected principles, especially operational excellence, security, reliability, and cost optimization.

---

## VII. Plan for Next Week

In the next week, I will continue learning and practicing more advanced infrastructure topics to improve system scalability and performance.

The planned topics include:

* Practicing Auto Scaling Groups.
* Learning how to automatically add or remove EC2 instances based on demand.
* Studying Amazon CloudFront for content delivery and CDN optimization.
* Understanding how CDN reduces latency for users in different regions.
* Continuing to practice monitoring and cost control.
* Updating my Hugo portfolio with technical notes and screenshots.
* Reviewing AWS architecture videos and documentation to strengthen my cloud design mindset.

