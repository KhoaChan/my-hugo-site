---
title: "Week 4 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---



# Week 4 Worklog

## I. Overview

Week 4 was the stage where I continued expanding my knowledge from basic networking and operations into more advanced topics related to security, multi-VPC connectivity, centralized routing, and automated application deployment on AWS.

During this week, I focused on important labs including **AWS Security Hub**, **VPC Peering**, **AWS Transit Gateway**, **AWS CodePipeline**, and **AWS WAF**. These services are commonly used in larger cloud systems where security, governance, network connectivity, and automated deployment are important requirements.

Through these labs, I understood that a real AWS system does not only need servers, databases, or individual VPCs. It also requires centralized security auditing, proper network connectivity design, application-layer protection, and CI/CD pipelines to deploy source code faster, more safely, and with fewer manual errors.

---

## II. Weekly Objectives

The main objectives for Week 4 were:

* Enable and explore AWS Security Hub to manage the security posture of an AWS account.
* Analyze security findings and understand how Security Hub evaluates compliance.
* Create a VPC Peering connection between two VPCs with non-overlapping CIDR ranges.
* Update route tables on both sides so that the VPCs can communicate privately.
* Understand AWS Transit Gateway and the Hub-and-Spoke model in multi-VPC networking.
* Attach VPCs to Transit Gateway using Transit Gateway Attachments.
* Understand how to update route tables to send traffic through Transit Gateway.
* Learn the native AWS CI/CD workflow using CodeCommit, CodeBuild, CodeDeploy, and CodePipeline.
* Configure an automated pipeline to deploy an application to EC2.
* Learn AWS WAF and how it protects web applications from common attack patterns.
* Practice cost-control discipline, especially with advanced networking services that may generate hourly charges.

---

## III. Weekly Activity Log

| Time      | Activity Category                   | Tasks Performed                                                                                                            | Results / Evidence Achieved                                               |
| --------- | ----------------------------------- | -------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| Day 1     | Centralized security                | Enabled AWS Security Hub, activated security standards, and checked the overview dashboard.                                | A centralized security auditing system was configured for the AWS account |
| Day 2     | VPC Peering connection              | Created a VPC Peering Connection between two VPCs with non-overlapping CIDR blocks and updated route tables on both sides. | The VPCs could communicate privately through the Peering connection       |
| Day 3 - 4 | Transit Gateway                     | Created AWS Transit Gateway, configured Transit Gateway Attachments, and updated VPC route tables.                         | A centralized Hub-and-Spoke routing model was completed                   |
| Day 5     | Application protection with AWS WAF | Created a Web ACL, configured protection rules, and associated AWS WAF with an Application Load Balancer.                  | The web application was strengthened with edge-layer protection           |
| Day 6     | CI/CD with AWS CodePipeline         | Built an automated deployment workflow using CodeCommit, CodeBuild, CodeDeploy, and CodePipeline.                          | The application could be automatically deployed when source code changed  |
| Day 7     | Summary and documentation           | Documented the practice process, summarized errors, checked resources, and prepared Hugo portfolio content.                | Week 4 Worklog was completed and screenshots/evidence were prepared       |

---

# IV. Detailed Technical Practice Through Labs

---

# LAB 18: AWS Security Hub - Centralized Security Management

## 1. Overview

As the number of AWS resources increases, manually checking each security configuration becomes difficult and error-prone. Therefore, a centralized tool is needed to collect, aggregate, and evaluate security issues across the account.

In this lab, I practiced enabling **AWS Security Hub** to monitor security findings, check compliance status, and review the overall security dashboard. This service helped me understand how AWS supports centralized and automated cloud security posture management.

## 2. Knowledge Learned

### AWS Security Hub

AWS Security Hub is a service that aggregates security findings from multiple AWS sources. It provides a centralized dashboard for monitoring security status, finding severity, and account compliance scores.

### Security Findings

Security findings are detected issues related to configurations or behaviors that may create security risks. Each finding usually has a severity level such as Low, Medium, High, or Critical.

### Security Standards

Security Hub supports security standards such as AWS Foundational Security Best Practices and CIS AWS Foundations Benchmark. These standards allow the system to automatically check resources against predefined security rules.

## 3. Implementation Process

The steps performed:

* Accessed the AWS Security Hub service.
* Enabled Security Hub in the AWS account.
* Allowed the system to create the required service-linked roles.
* Enabled security standards such as AWS Foundational Security Best Practices.
* Enabled CIS AWS Foundations Benchmark if suitable for the lab environment.
* Waited for Security Hub to scan resources.
* Opened the overview dashboard.
* Reviewed findings by severity.
* Checked the compliance score and rule status.
* Took notes on findings that needed remediation or further monitoring.

## 4. Practice Evidence

![Screenshot of creating IAM User Admin-Khoa](static/images/1-Workblog/Week-4/image.png)
![Screenshot of creating IAM User Admin-Khoa](static/images/1-Workblog/Week-4/image copy.png)
![Screenshot of creating IAM User Admin-Khoa](static/images/1-Workblog/Week-4/image copy 2.png)

## 5. Results Achieved

After completing this lab, I successfully enabled AWS Security Hub and understood how it supports centralized security auditing. I also realized that regularly reviewing findings helps detect unsafe configurations early and improves the overall security posture of an AWS account.

---

# LAB 19: VPC Peering - Connecting Two Separate VPC Networks

## 1. Overview

By default, VPCs in AWS are isolated from each other. This improves security, but in many cases, resources in different VPCs need to communicate privately. VPC Peering is a suitable solution for directly connecting two VPCs without sending traffic over the public Internet.

In this lab, I practiced creating a **VPC Peering Connection**, accepting the connection request, and updating route tables on both sides so that resources could communicate through AWS private networking.

## 2. Knowledge Learned

### VPC Peering Connection

A VPC Peering Connection is a one-to-one logical network connection between two VPCs. It allows resources in both VPCs to communicate using private IP addresses.

### Non-overlapping CIDR

A key requirement for VPC Peering is that the two VPCs must not have overlapping CIDR ranges. If the IP ranges overlap, the system cannot route traffic correctly between the networks.

### Route Table Update

After creating a VPC Peering connection, the route tables of both VPCs must be updated. Each side needs a route pointing to the other VPC’s CIDR block, with the Peering Connection as the target.

## 3. Implementation Process

The steps performed:

* Accessed the VPC service.
* Opened Peering connections.
* Selected Create peering connection.
* Selected the requester VPC.
* Selected the accepter VPC.
* Verified that the two VPCs had non-overlapping CIDR ranges.
* Created the Peering request.
* Switched to the accepter side and selected Accept request.
* Verified that the connection status changed to Active.
* Opened the route table of the first VPC.
* Added a route to the CIDR block of the second VPC, using the Peering Connection as the target.
* Opened the route table of the second VPC.
* Added a route to the CIDR block of the first VPC, using the Peering Connection as the target.
* Checked the two-way routing configuration.

## 4. Practice Evidence

![Screenshot of creating IAM User Admin-Khoa](static/images/1-Workblog/Week-4/image copy 3.png)
![Screenshot of creating IAM User Admin-Khoa](static/images/1-Workblog/Week-4/image copy 4.png)
![Screenshot of creating IAM User Admin-Khoa](static/images/1-Workblog/Week-4/image copy 5.png)

## 5. Results Achieved

After completing this lab, I successfully set up a VPC Peering connection between two VPCs. I understood that VPC Peering is suitable for simple connectivity between a small number of VPCs, but it can become difficult to manage when the number of VPCs increases.

---

# LAB 20: AWS Transit Gateway - Building a Centralized Routing Model

## 1. Overview

When an organization has many VPCs, using VPC Peering to connect every pair of VPCs can create a complex and hard-to-manage network. AWS Transit Gateway solves this problem by acting as a central router that allows multiple VPCs to connect through a single hub.

In this lab, I practiced creating **AWS Transit Gateway**, creating Transit Gateway Attachments, and updating VPC route tables so that traffic could pass through Transit Gateway.

## 2. Knowledge Learned

### AWS Transit Gateway

AWS Transit Gateway is a centralized routing service that connects multiple VPCs, VPNs, or Direct Connect Gateways. It is suitable for Hub-and-Spoke network architectures, where Transit Gateway acts as the Hub and VPCs act as Spokes.

### Transit Gateway Attachment

A Transit Gateway Attachment is a logical connection between Transit Gateway and a network resource such as a VPC. Once a VPC is attached to Transit Gateway, it can send traffic to other networks through the central hub.

### Hub-and-Spoke Architecture

The Hub-and-Spoke model simplifies network management. Instead of connecting every VPC directly to every other VPC, all VPCs connect to Transit Gateway, reducing the number of connections that need to be managed.

## 3. Implementation Process

The steps performed:

* Accessed the VPC Dashboard.
* Opened Transit gateways.
* Selected Create Transit Gateway.
* Named the Transit Gateway `Hub-TGW`.
* Configured ASN and basic settings.
* Created the Transit Gateway.
* Opened Transit Gateway Attachments.
* Created an attachment for the first VPC.
* Created an attachment for the second VPC.
* Selected suitable subnets to connect with Transit Gateway.
* Checked the attachment status.
* Updated the route table of each VPC.
* Added a route to the other VPC’s CIDR block, using Transit Gateway as the target.
* Verified traffic routing through Transit Gateway.

## 4. Practice Evidence

![Screenshot of creating IAM User Admin-Khoa](static/images/1-Workblog/Week-4/image copy 6.png)
![Screenshot of creating IAM User Admin-Khoa](static/images/1-Workblog/Week-4/image copy 7.png)
![Screenshot of creating IAM User Admin-Khoa](static/images/1-Workblog/Week-4/image copy 8.png)

## 5. Results Achieved

After completing this lab, I understood how AWS Transit Gateway simplifies multi-VPC connectivity. I also realized that Transit Gateway is more suitable than VPC Peering when many private networks need to be connected and managed centrally.

---

# LAB 23: AWS CodePipeline - Deploying an Application to EC2 with CI/CD

## 1. Overview

In software development, manually deploying applications to servers can cause mistakes and consume time. CI/CD helps automate the process of retrieving source code, building, testing, and deploying applications.

In this lab, I practiced building a pipeline to deploy a Node.js application to EC2 using native AWS services such as **CodeCommit, CodeBuild, CodeDeploy, and CodePipeline**. Through this practice, I better understood how AWS supports automated software delivery.

## 2. Knowledge Learned

### AWS CodePipeline

AWS CodePipeline is a service that orchestrates the CI/CD workflow. A pipeline can include multiple stages such as Source, Build, and Deploy. When source code changes, the pipeline can automatically trigger deployment steps.

### AWS CodeCommit

CodeCommit is an AWS-managed Git repository service. It can act as the Source Stage in a pipeline.

### AWS CodeBuild

CodeBuild is used to build and test source code. The build process is usually defined in a `buildspec.yml` file.

### AWS CodeDeploy

CodeDeploy helps deploy applications to EC2 or other environments. On EC2, the CodeDeploy Agent must be installed to receive deployment commands from AWS.

### AppSpec File

The `appspec.yml` file defines how CodeDeploy deploys the application, including where files should be copied and which scripts should run during each deployment phase.

## 3. Implementation Process

The steps performed:

* Accessed the CodeCommit service.
* Created a repository named `NodeJS-App-Repo`.
* Prepared the Node.js application source code.
* Added the `buildspec.yml` file.
* Added the `appspec.yml` file.
* Pushed the source code to CodeCommit.
* Accessed the CodeDeploy service.
* Created an Application for the project.
* Created a Deployment Group.
* Associated the Deployment Group with the EC2 instance using tags.
* Ensured that the EC2 instance had the CodeDeploy Agent installed.
* Configured the required IAM Role for CodeDeploy.
* Accessed CodePipeline.
* Created a new pipeline.
* Selected CodeCommit as the Source Stage.
* Selected CodeBuild as the Build Stage.
* Selected CodeDeploy as the Deploy Stage.
* Ran the pipeline and monitored each stage status.
* Checked the application after the pipeline completed.

## 4. Practice Evidence

![Screenshot of creating IAM User Admin-Khoa](static/images/1-Workblog/Week-4/image copy 9.png)

## 5. Results Achieved

After completing this lab, I understood the basic CI/CD workflow on AWS. The pipeline can automatically pull source code, build it, and deploy the application to EC2, reducing manual operations and improving deployment stability.

---

## V. Challenges Encountered and Solutions

During Week 4, I encountered several real-world issues while practicing advanced AWS services related to security, networking, and CI/CD.

### 1. IAM Permission Limits in the Sandbox Environment

Some AWS services require permission to create service-linked roles or advanced IAM permissions. In a Sandbox or internship account, these permissions may be limited, causing some Console operations to fail.

To handle this, I checked the error messages carefully, identified the missing permissions, and tried using AWS CLI when allowed. I also learned the importance of IAM Roles when AWS services need to communicate with each other.

### 2. Cost Management for Advanced Networking Services

Services such as Transit Gateway, VPC Peering, or advanced security components may generate charges if left running for a long time. Therefore, it is important to check resources after each lab.

My solution was to document created resources, capture evidence immediately after completing the lab, and delete unused resources in the correct dependency order.

### 3. Route Table Configuration Complexity

When configuring VPC Peering or Transit Gateway, creating the connection alone is not enough. The route tables of each VPC must be updated correctly so that traffic can reach the destination network.

I handled this by checking the CIDR block of each VPC, adding the correct route target, and verifying the connection status before testing communication between networks.

### 4. CI/CD Requires Multiple Connected Components

CodePipeline depends on several services working together, including repository, build project, deployment group, EC2 instance, CodeDeploy Agent, and IAM Role. If any component is misconfigured, the pipeline may fail.

To solve this, I checked each stage separately, reviewed logs in CodeBuild or CodeDeploy, and ensured the EC2 instance was ready to receive deployments.

---

## VI. Reflection After Week 4

Week 4 helped me understand more clearly how an enterprise-level cloud system is operated. Building infrastructure is not only about creating servers or databases, but also about centralized security, controlled network connectivity, large-scale routing, and automated application deployment.

I realized that services such as Security Hub, WAF, Transit Gateway, and CodePipeline all serve very practical business goals: protecting systems, connecting multiple environments, reducing manual operations, and improving operational control.

The most important lesson I learned this week is that cloud engineering requires system-level thinking instead of looking at individual services separately. A small change in an IAM Role, Route Table, or Deployment Group can affect the entire operational flow.

---

## VII. Plan for Next Week

In the next week, I will continue focusing on improving my system architecture mindset and preparing for practical project-related topics.

The planned topics include:

* Holding a team meeting to agree on the graduation project architecture.
* Analyzing the Vietnam travel booking website integrated with an AI consulting chatbot.
* Identifying the main microservices of the system.
* Planning source code structure and repositories.
* Learning how to deploy a real application on AWS.
* Joining technical workshops or events in the bootcamp program.
* Continuing to study reference architectures from AWS Solutions Architects.
* Updating the Hugo portfolio with technical notes and practice screenshots/evidence.

