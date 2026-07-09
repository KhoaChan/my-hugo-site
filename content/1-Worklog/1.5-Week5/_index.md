---
title: "Week 5 Worklog"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.5. </b> "
---




## I. Executive Summary

Week 5 focused on **identity governance, data security, and enterprise-grade storage** on AWS. While the previous weeks mainly covered networking, servers, load balancing, and application deployment, this week went deeper into access control, permission boundaries, encryption, and secure authorization models.

The labs this week included **Amazon FSx for Windows File Server**, **Amazon S3 Static Website Hosting**, **IAM Resource Tags**, **IAM Permission Boundary**, **AWS KMS**, **IAM Conditions**, and **IAM Role for EC2 Applications**. These topics are important for building a secure, manageable, and enterprise-ready cloud system.

Through this week’s practice, I learned that AWS security is not only about creating users or assigning basic permissions. A professional cloud system should apply the **Least Privilege** principle, use contextual access control, replace long-term Access Keys with IAM Roles, encrypt sensitive data, and organize resources with tags for better governance.

---

## II. Strategic Objectives

The main objectives for Week 5 were:

* Understand Amazon FSx for Windows File Server and its role in enterprise file storage.
* Learn how FSx integrates with AWS Managed Microsoft Active Directory.
* Configure S3 Static Website Hosting and manage public access carefully.
* Manage EC2 access based on Resource Tags.
* Apply IAM Policy Conditions to limit access based on resource tags.
* Understand and configure IAM Permission Boundary.
* Learn how Permission Boundary limits the maximum permissions of an IAM User or IAM Role.
* Create and use an AWS KMS Customer Managed Key for data encryption.
* Separate key administration permissions from key usage permissions in KMS.
* Understand IAM Conditions based on IP address, time, or request context.
* Replace Access Keys with IAM Roles for applications running on EC2.
* Strengthen cloud security thinking around identity, authorization, and data protection.

---

## III. Weekly Activity Log

| Time  | Activity Category        | Tasks Performed                                                                                                             | Results / Evidence Achieved                                                     |
| ----- | ------------------------ | --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------- |
| Day 1 | Enterprise file storage  | Practiced Amazon FSx for Windows File Server, configured storage capacity, VPC settings, and Active Directory requirements. | Understood how to deploy file storage for Windows-based enterprise environments |
| Day 2 | S3 hosting and security  | Configured S3 bucket, enabled Static Website Hosting, set Bucket Policy, and managed public access.                         | Static website could be served through an S3 endpoint                           |
| Day 3 | IAM and Resource Tags    | Created an IAM Policy using conditions based on Resource Tags to control EC2 actions.                                       | EC2 access was limited based on resource tags                                   |
| Day 4 | Permission Boundary      | Created an IAM Permission Boundary to limit the maximum permission scope of a user or role.                                 | Prevented users from exceeding the allowed permission scope                     |
| Day 5 | Data encryption with KMS | Created a Customer Managed Key, configured key administrators and key users, and practiced data encryption.                 | Understood how to encrypt data using AWS KMS                                    |
| Day 6 | Advanced IAM Conditions  | Learned how to restrict access by IP address, time, or specific IAM policy conditions.                                      | Understood how conditions can strengthen access security                        |
| Day 7 | IAM Role for EC2         | Removed direct Access Key usage and applied IAM Role for applications running on EC2.                                       | EC2 applications could access AWS services more securely through IAM Role       |

---

# IV. Detailed Technical Practice Through Labs

---

# LAB 25: Amazon FSx for Windows File Server

## 1. Overview

Amazon FSx for Windows File Server is a fully managed file storage service provided by AWS. It supports the SMB protocol and is highly compatible with Windows Server environments. This service is suitable for enterprises that rely on Windows, Active Directory, and applications that require traditional shared file storage.

In this lab, I learned how to configure Amazon FSx, select storage capacity, review networking requirements, and understand the role of AWS Managed Microsoft Active Directory in user authentication and file access control.

## 2. Knowledge Learned

### Amazon FSx for Windows File Server

Amazon FSx provides managed file storage, reducing the need to operate traditional file servers manually. It supports SMB, NTFS permissions, Active Directory integration, and automatic backups.

### SMB Protocol

SMB is a common file-sharing protocol in Windows environments. Because FSx supports SMB, Windows machines and Windows-based applications can use it with minimal configuration changes.

### AWS Managed Microsoft Active Directory

FSx for Windows requires Active Directory integration to authenticate users and manage file permissions. This is an important difference compared with storage services such as Amazon S3 or Amazon EFS.

### Backup and Maintenance

FSx supports automatic backups and scheduled maintenance windows. These features help protect data and maintain system stability over time.

## 3. Implementation Process

The steps performed:

* Accessed the Amazon FSx service.
* Created a new file system.
* Selected Windows File Server as the file system type.
* Configured storage capacity, for example `32 GiB`.
* Selected SSD storage for better read/write performance.
* Selected the appropriate VPC and subnet.
* Reviewed the AWS Managed Microsoft Active Directory requirement.
* Configured daily automatic backup.
* Checked the maintenance window settings.
* Reviewed all configuration details before creating the file system.
* Took notes on cost and dependency requirements.

## 4. Practice Evidence

![Screenshot of creating IAM User Admin-Khoa](/images/1-Workblog/Week-5/image.png)

## 5. Results Achieved

After completing this lab, I understood how Amazon FSx supports enterprise file storage in Windows-based environments. I also realized that FSx is suitable for systems that require SMB, NTFS, and Active Directory, but its cost and dependencies should be considered carefully before deployment.

---

# LAB 57: Starting with Amazon S3

## 1. Overview

Amazon S3 is a highly scalable object storage service with strong durability, availability, and security. Besides storing files, S3 can also be used to host a static website through the Static Website Hosting feature.

In this lab, I practiced creating an S3 bucket, configuring controlled public access, enabling Static Website Hosting, and using a Bucket Policy to allow users to access website content.

## 2. Knowledge Learned

### Object Storage

S3 stores data as objects. Each object includes data, metadata, and a unique key. This storage model is suitable for static files, images, logs, backups, and data that does not need to be edited like a traditional file system.

### Static Website Hosting

Static Website Hosting allows an S3 bucket to serve HTML, CSS, and JavaScript files as a static website. This approach does not require server management and is suitable for landing pages, portfolios, or documentation websites.

### Bucket Policy

A Bucket Policy is used to control access at the bucket level. When hosting a static website, the policy must allow the `s3:GetObject` action for objects that should be publicly accessible.

### Public Access Block

By default, S3 blocks public access to protect data. When public website access is required, Block Public Access must be adjusted carefully to avoid exposing unwanted data.

## 3. Implementation Process

The steps performed:

* Accessed the Amazon S3 service.
* Created a new S3 bucket.
* Chose a globally unique bucket name.
* Selected an appropriate region.
* Configured Block Public Access according to the lab objective.
* Enabled Static Website Hosting.
* Set `index.html` as the index document.
* Created or uploaded the `index.html` file.
* Configured a Bucket Policy to allow public read access.
* Checked the S3 website endpoint.
* Accessed the endpoint in a browser to confirm the website was working.

## 4. Practice Evidence

![Screenshot of creating IAM User Admin-Khoa](/images/1-Workblog/Week-5/image%20copy.png)
![Screenshot of creating IAM User Admin-Khoa](/images/1-Workblog/Week-5/image%20copy%202.png)

## 5. Results Achieved

After completing this lab, I understood how to use Amazon S3 to deploy a static website. I also learned that when making a bucket public, the Bucket Policy must be reviewed carefully and only the required files should be publicly accessible.

---

# LAB 28: Managing EC2 Access Through Resource Tags Using IAM

## 1. Overview

In an AWS environment with many resources, assigning permissions to each individual resource can become difficult to manage. Resource Tags help classify resources by department, project, environment, or purpose. When tags are combined with IAM Policy Conditions, access control becomes more flexible and scalable.

In this lab, I practiced creating an IAM Policy that allows EC2 actions based on resource tags. For example, only EC2 instances with the tag `Department: Finance` are allowed to be started or stopped.

## 2. Knowledge Learned

### Resource Tags

Resource Tags are key-value pairs attached to AWS resources. Tags help with management, searching, classification, cost allocation, and access control.

### IAM Policy Condition

A Condition in an IAM Policy allows permissions to be limited based on specific criteria. With EC2, `aws:ResourceTag` can be used to allow actions only on resources with matching tags.

### Department-level Isolation

Tag-based permission control helps isolate resources by department or project team. For example, users from the Finance department can only manage resources tagged as Finance.

## 3. Implementation Process

The steps performed:

* Accessed the IAM service.
* Created a new IAM Policy.
* Wrote a JSON policy using the condition `aws:ResourceTag/Department`.
* Configured permissions for EC2 actions such as StartInstances and StopInstances.
* Created or selected a suitable IAM User/Role.
* Attached the policy to the User or Role.
* Added the tag `Department: Finance` to the target EC2 instance.
* Tested access to EC2 instances with matching tags.
* Tested access to EC2 instances without matching tags.

## 4. Practice Evidence

![Screenshot of creating IAM User Admin-Khoa](/images/1-Workblog/Week-5/image%20copy%203.png)
![Screenshot of creating IAM User Admin-Khoa](/images/1-Workblog/Week-5/image%20copy%204.png)

## 5. Results Achieved

After completing this lab, I understood how to use Resource Tags together with IAM Policies to control access to resources. This method makes permission management more flexible and suitable for environments with multiple departments or project teams.

---

# LAB 30: Limiting User Permissions with IAM Permission Boundary

## 1. Overview

IAM Permission Boundary is an advanced security feature that defines the maximum permission scope an IAM User or IAM Role can have. Even if the entity is attached to a broader policy, its effective permissions cannot exceed the Permission Boundary.

In this lab, I learned how to create a policy that acts as a Permission Boundary, attach it to an IAM User, and test how the boundary prevents actions outside the allowed scope.

## 2. Knowledge Learned

### Permission Boundary

A Permission Boundary does not directly grant permissions. Instead, it defines the maximum permissions that an entity can receive. A user can perform an action only if the action is allowed by both the attached policy and the boundary.

### Least Privilege

Permission Boundary supports the Least Privilege principle at a higher governance level. It is especially useful in enterprise environments where teams may create users or roles but still need to stay within a controlled permission scope.

### Privilege Escalation Prevention

Without boundaries, a user with permission to create policies or roles might grant themselves higher privileges. Permission Boundary helps reduce the risk of unintended privilege escalation.

## 3. Implementation Process

The steps performed:

* Accessed the IAM service.
* Created a new policy to use as a Permission Boundary.
* Defined the maximum service scope that the user could access.
* Created or selected the IAM User to be restricted.
* Attached the Permission Boundary to that user.
* Attached another permission policy to the user for testing.
* Tested actions within the boundary scope.
* Tested actions outside the boundary scope.
* Verified that the boundary blocked unauthorized actions.

## 4. Practice Evidence

![Screenshot of creating IAM User Admin-Khoa](/images/1-Workblog/Week-5/image%20copy%205.png)
![Screenshot of creating IAM User Admin-Khoa](/images/1-Workblog/Week-5/image%20copy%206.png)

## 5. Results Achieved

After completing this lab, I gained a clearer understanding of IAM Permission Boundary and its role in limiting maximum permissions. This mechanism is important for large systems because it helps prevent users or roles from receiving permissions beyond the intended scope.

---

# LAB 33: AWS Key Management Service - KMS

## 1. Overview

AWS Key Management Service is a managed service that helps users create, manage, and use encryption keys on AWS. KMS is commonly used to protect sensitive data stored in services such as S3, EBS, RDS, or custom applications.

In this lab, I practiced creating a Customer Managed Key, configuring key administrators and key users, and learning how to use the KMS key to encrypt data.

## 2. Knowledge Learned

### AWS KMS

AWS KMS is a centralized encryption key management service. Users can create keys, configure key usage permissions, enable or disable keys, rotate keys, and monitor key usage through CloudTrail.

### Customer Managed Key

A Customer Managed Key is created and managed by the user. This type of key provides more detailed control over policy, administration permissions, and usage permissions compared with AWS managed keys.

### Key Administrators and Key Users

In KMS, it is important to separate key administrators from key users. Key Administrators can manage key configuration, while Key Users can only use the key to encrypt or decrypt data.

### Encryption at Rest

Encryption at Rest means encrypting data while it is stored. This is an important security requirement for sensitive data in cloud environments.

## 3. Implementation Process

The steps performed:

* Accessed the AWS KMS service.
* Created a new key.
* Selected Symmetric Key as the key type.
* Named the key, for example `Lab33-My-KMS-Key`.
* Configured Key Administrators.
* Configured Key Users.
* Reviewed the Key Policy.
* Created the KMS key.
* Used the key to encrypt sample data.
* Checked the encrypted output.
* Noted how CloudTrail can be used to trace key-related activities.

## 4. Practice Evidence

![Screenshot of creating IAM User Admin-Khoa](/images/1-Workblog/Week-5/image%20copy%207.png)
![Screenshot of creating IAM User Admin-Khoa](/images/1-Workblog/Week-5/image%20copy%208.png)

## 5. Results Achieved

After completing this lab, I understood how AWS KMS supports data encryption and centralized key management. I also realized that separating key administration from key usage is important for protecting sensitive data.

---

# LAB 44: IAM Role with IP and Time Conditions

## 1. Overview

In highly secure systems, simply granting permissions to a user or role is not enough. Access should also be limited by conditions such as IP address, access time, or request context.

In this lab, I learned how to use IAM Policy Conditions to restrict access based on IP address or time. This helps reduce risk if credentials are used outside an approved environment.

## 2. Knowledge Learned

### IAM Conditions

IAM Conditions add extra requirements to a policy. An action is allowed only if the request satisfies the defined conditions.

### IP-based Access Control

IP-based conditions can restrict access to trusted IP ranges, such as company office IP addresses or personal IP addresses.

### Time-based Access Control

Time-based conditions can limit access to a specific time period. This is useful for temporary tasks or permissions that should only be used during working hours.

## 3. Implementation Process

The steps performed:

* Accessed the IAM service.
* Created or edited an IAM Policy.
* Added an IP-based condition if required.
* Added a time-based condition if required.
* Attached the policy to a user or role.
* Tested access when the condition was satisfied.
* Tested access when the condition was not satisfied.
* Recorded the results to understand how policy conditions work.

## 4. Practice Evidence


## 5. Results Achieved

After completing this lab, I understood how IAM Conditions strengthen access security. Restricting access by IP address or time can reduce the risk of accounts being misused outside the intended scope.

---

# LAB 48: Authorization with IAM Role for EC2

## 1. Overview

In the past, many applications accessed AWS by storing Access Key and Secret Key directly on the server or in configuration files. This approach creates a high security risk because if the keys are exposed, attackers may gain access to AWS resources.

In this lab, I learned how to use IAM Role for EC2 to grant permissions to applications running on an instance without storing Access Keys directly. This is a safer approach and follows AWS best practices.

## 2. Knowledge Learned

### IAM Role for EC2

IAM Role for EC2 allows an EC2 instance to receive temporary permissions to access AWS services. Applications running on the instance can use these permissions through instance metadata without storing fixed keys.

### Temporary Credentials

Temporary credentials have a limited lifetime and are automatically rotated by AWS. This is safer than long-term Access Keys.

### Removing Access Keys

Removing Access Keys from applications reduces the risk of credential leakage. This is an important step when deploying real applications in the cloud.

## 3. Implementation Process

The steps performed:

* Accessed the IAM service.
* Created a new IAM Role for EC2.
* Attached an appropriate policy to the role.
* Attached the IAM Role to the EC2 instance.
* Accessed the EC2 instance to test permissions.
* Ran AWS CLI commands from EC2 to verify that the role worked.
* Removed direct Access Key usage from the application.
* Verified that the application could access AWS services through the role.

## 4. Practice Evidence
## 5. Results Achieved

After completing this lab, I understood that IAM Role is a safer way to grant permissions to applications running on EC2. Instead of storing Access Keys directly, applications can use temporary role credentials, reducing the risk of credential exposure.

---

## V. Challenges Encountered and Solutions

During Week 5, I encountered several practical challenges related to enterprise storage, advanced permissions, and data encryption.

### 1. Active Directory Requirement for FSx

While practicing Amazon FSx for Windows File Server, I found that the service requires integration with AWS Managed Microsoft Active Directory. If a suitable Directory is not available, the file system creation process may be blocked.

The solution is to prepare Active Directory before deploying FSx or consider using another storage service that better matches the practice environment.

### 2. Cost Management for Enterprise Services

Some services such as FSx or Managed Microsoft AD can generate costs if left running for too long. Therefore, configurations should be reviewed carefully and resources should be cleaned up after the lab is completed.

My solution was to document created resources, capture evidence immediately after completion, and delete unnecessary resources to avoid unexpected costs.

### 3. Complexity of IAM Conditions

When writing policies with conditions, incorrect condition keys or JSON syntax can cause the policy to behave incorrectly. This requires careful review before applying the policy.

The solution was to test each condition separately, use sample resources, and verify the result through real actions.

### 4. KMS Permission Separation

In KMS, if users do not have proper permissions, they may not be able to use a key for encryption or decryption. On the other hand, if permissions are too broad, the key may be misused.

I learned that Key Administrators and Key Users should be clearly separated to ensure secure key management.

---

## VI. Reflection After Week 5

Week 5 changed the way I think about security on AWS. Previously, I often thought that Administrator permissions were enough for quick practice. However, in real environments, overly broad permissions can create serious security risks.

I realized that mechanisms such as Permission Boundary, IAM Conditions, Resource Tags, IAM Role, and KMS all aim toward the same goal: controlling access carefully and protecting important data. A secure cloud system does not only depend on strong passwords, but also on permission design, data encryption, and the removal of long-term credentials.

The most important lesson this week is that security should be designed from the beginning, not added after the system is completed. Applying Least Privilege, Separation of Duties, and Encryption at Rest helps make the system more reliable and suitable for enterprise environments.

---

## VII. Plan for Next Week

In the next week, I will continue strengthening my technical knowledge and begin shaping a clearer direction for the practical project.

The planned activities include:

* Completing labs in the next module.
* Continuing to study and practice at AWS Vietnam office.
* Discussing with mentors or technical experts if issues appear during practice.
* Starting the idea for the final project.
* Researching the Vietnam travel booking website integrated with an AI consulting chatbot.
* Analyzing system architecture, data flow, and main components.
* Learning more about Microservices and AI-driven applications.
* Applying IAM Role, Permission Boundary, and KMS knowledge to the project’s security design.
* Updating the Hugo portfolio with technical notes and practice screenshots/evidence.

