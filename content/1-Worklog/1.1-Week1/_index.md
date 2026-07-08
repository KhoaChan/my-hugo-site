---
title: "Week 1 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---


## I. Week 1 Overview

The first week of the **First Cloud AI Journey (FCJ)** internship program was an important stage for me to become familiar with the learning environment, understand the working process, and build an initial foundation in cloud computing. The main objective of this week was not only to understand AWS at a conceptual level, but also to gradually set up a practical working environment, secure my account, and become familiar with the essential tools needed for the upcoming labs.

During this week, I completed several important tasks such as joining the FCJ community, learning about AWS Global Infrastructure, configuring AWS CLI, practicing with some basic AWS services, and applying initial security principles to my AWS account. In particular, I participated in workshops, completed the required labs, and received **$100 AWS Credit** to continue practicing throughout the learning journey.

## II. Weekly Activity Log

| Time      | Activity                                | Task Description                                                                                                                                                                                     | Result                                            |
| --------- | --------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------- |
| Day 1 - 2 | Program onboarding and account security | Joined the FCJ workspace, reviewed the general rules, and became familiar with the program structure. At the same time, I secured the AWS Root account by enabling MFA.                              | AWS Root account was basically secured            |
| Day 3     | Learning AWS infrastructure             | Studied AWS Global Infrastructure, including Region, Availability Zone, and Local Zone. I also learned the role of each component in building a stable cloud system.                                 | Understood the global infrastructure model of AWS |
| Day 4     | Setting up the practice environment     | Installed AWS CLI v2, Session Manager Plugin, and configured local profiles on my personal computer. I verified the connection using STS commands to confirm that the account was working correctly. | CLI environment was ready for practice            |
| Day 5 - 6 | Practicing AWS services                 | Joined the “Explore AWS” workshop and completed labs related to AI/ML, Serverless, Database, and Compute services.                                                                                   | Completed the labs and received $100 AWS Credit   |
| Day 7     | Documentation and summary               | Built the initial structure for my internship portfolio using Hugo, documented my learning process, and checked the initial cost settings in AWS.                                                    | Completed the foundation for my worklog portal    |

## III. Technical Topics Learned

### 1. Identity and Access Management with IAM

During the first week, I realized that security is a very important part of working with AWS. Therefore, I focused on learning and practicing the basic concepts of **IAM - Identity and Access Management**.

The tasks I completed included:

* Creating a separate IAM user instead of using the Root account for daily activities.
* Enabling MFA for the Root account to increase login security.
* Learning the **Least Privilege** principle, which means granting only the permissions required for each user or service.
* Reading and analyzing IAM Policies in JSON format to understand how AWS controls access permissions.
* Understanding the differences between User, Group, Role, and Policy in the AWS permission management system.

Through this section, I understood that directly using the Root account is not a safe practice. Instead, IAM Users or IAM Roles should be created based on specific use cases in order to reduce security risks.

### 2. Getting Familiar with AWS CLI and the Local Environment

After learning about AWS Management Console, I continued by installing and configuring **AWS CLI** on my personal computer. Using CLI helps me work with AWS more efficiently and also prepares me better for automation-related labs in the future.

The tasks I completed included:

* Installing AWS CLI v2.
* Configuring Access Key, Secret Access Key, and default Region.
* Creating a profile to manage login credentials.
* Verifying the AWS account identity using the command `aws sts get-caller-identity`.
* Practicing several basic commands to check AWS services and resources.

Through this part, I understood the difference between working with AWS through the Console and using the command line. The Console is visual and easy to use, while CLI is more suitable for automation or performing quick operations across multiple resources.

### 3. Exploring Basic AWS Services

During the workshop and lab activities, I had the opportunity to explore several common AWS services, including:

#### Amazon EC2

I learned how to launch a virtual server using EC2, choose a suitable instance type, configure Security Groups, and connect to the server using SSH. Through this practice, I gained a better understanding of how to deploy a server environment on the cloud.

#### AWS Lambda

I practiced creating a basic serverless function. What impressed me most was that Lambda allows code to run without directly managing servers. This helped me better understand the serverless model and how developers can focus more on application logic instead of infrastructure management.

#### Amazon RDS

I learned how to deploy a relational database on AWS. Concepts such as Automated Backup, Multi-AZ, and database instance configuration helped me understand more about data durability and availability in a cloud environment.

#### Amazon Bedrock

I became familiar with Amazon Bedrock and Foundation Models. By testing models through the playground, I gained a basic understanding of how to send prompts, receive model responses, and observe factors such as tokens and response latency.

#### AWS Budgets

I created a budget alert to monitor AWS usage costs. This is a very necessary step when using a Free Tier account or AWS Credit, as it helps avoid unexpected charges.

## IV. Issues Encountered and Solutions

While using AWS CLI, I encountered an **AccessDenied** error when trying to list S3 buckets. At first, I thought the problem came from the CLI configuration, but after checking again, I found that the IAM User did not have the required permission.

### Cause

The IAM User I was using had not been granted the `s3:ListAllMyBuckets` permission, so AWS denied the request to access the bucket list.

### Solution

I reviewed the IAM Policy and added the required permission to the user. However, instead of granting unnecessary full access, I only added the permission related to the specific action I needed to perform, following the **Least Privilege** principle.

### Lesson Learned

From this issue, I learned that most actions in AWS depend on IAM permissions. When encountering an AccessDenied error, it is important to check the policy, assigned permissions, and resource scope before assuming that the problem comes from the service or tool itself.

## V. Results Achieved in Week 1

After the first week, I achieved the following results:

* Became familiar with the First Cloud AI Journey program and its learning process.
* Understood the general concept of AWS and its main service categories such as Compute, Storage, Database, Networking, and AI/ML.
* Learned the concepts of Region, Availability Zone, and how AWS organizes its global infrastructure.
* Secured the AWS Root account with MFA.
* Created an IAM User for daily practice activities.
* Successfully installed and configured AWS CLI on my personal computer.
* Practiced checking AWS account information using CLI.
* Became familiar with EC2, Lambda, RDS, Bedrock, and AWS Budgets.
* Completed the required labs in the workshop.
* Received **$100 AWS Credit** to continue practicing in the following weeks.
* Built the initial structure for a portfolio to record my internship progress.

## VI. Reflection After the First Week

The first week helped me gain a clearer view of AWS and how a cloud system is organized. Previously, I only understood cloud computing as a place to host websites or store data. After practicing, I realized that AWS is a much broader ecosystem that includes infrastructure, security, databases, artificial intelligence, automation, and cost management.

The most important lesson I learned this week is that when working with cloud services, it is not enough to simply make a service run successfully. Users also need to consider security, permission management, cost control, scalability, and proper resource governance.

Receiving $100 AWS Credit was also a strong motivation for me to continue practicing more in the upcoming weeks. It gives me the opportunity to experiment with real AWS services without worrying too much about the initial cost.

## VII. Plan for Week 2

In the next week, I will focus more deeply on core AWS infrastructure services. The goal is to understand how to design networks, deploy servers, configure databases, and build scalable systems.

The planned topics for Week 2 include:

* Lab 1: Advanced IAM and Organizational Unit.
* Lab 2: Network design with Amazon VPC.
* Lab 3: Practicing EC2 with Auto Scaling and Load Balancing.
* Lab 4: Launching and configuring Windows/Linux virtual servers on EC2.
* Lab 5: Deploying and managing relational databases with Amazon RDS.
* Lab 6: Deploying the FCJ Management Application with Auto Scaling Group.

