---
title: "Blog 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# AUTOMATICALLY REPLICATING S3 BUCKET CONFIGURATIONS ACROSS AWS REGIONS

When enterprises operate systems on AWS for a long period of time, it is very common to have hundreds or even thousands of Amazon S3 buckets in the same Region. Each bucket may have been created at different times, by different teams, and through different methods such as the AWS Management Console, internal scripts, or older automation tools.

Over time, each bucket accumulates its own configurations such as bucket policies, lifecycle rules, encryption settings, access control, logging, tags, Object Lock, or Static Website Hosting. These configurations can directly affect security, compliance, cost, and how data is managed within the organization.

The problem appears when an enterprise needs to recreate those buckets in another AWS Region with the same configuration as the original buckets. If this is done manually, the operations team must open each bucket, inspect every configuration, and then reapply each setting in the new Region. This approach is not only time-consuming but also highly error-prone, especially when the number of buckets continues to grow.

Amazon S3 already provides features such as Cross-Region Replication to replicate new objects between Regions, or S3 Batch Operations to copy existing objects at scale. However, these features mainly focus on the object data inside the bucket. Bucket-level configurations still need to be handled through separate APIs.

To solve this problem, AWS introduces a solution using AWS Step Functions and AWS Lambda to automatically replicate the configuration of an S3 bucket from a source Region to a destination Region. This solution can create a new bucket in the destination Region, read configurations from the source bucket, apply those configurations to the destination bucket, and record the entire process in Amazon DynamoDB and Amazon CloudWatch for auditing.

This architecture is suitable for enterprises that need to migrate, expand, or recreate bucket configurations across Regions without manually repeating each step.

━━━━━━━━━━━━━━━

## KEY HIGHLIGHTS

━━━━━━━━━━━━━━━

### Automatically create a bucket in the destination Region

The solution allows users to provide the source bucket and the destination Region. If the destination bucket name is not specified, the system can automatically generate a new bucket name with a specific prefix to avoid naming conflicts. This makes the deployment process more flexible, especially when many buckets need to be replicated.

### Copy bucket configurations using AWS Lambda

A Lambda function reads the configurations of the source bucket through S3 APIs and then applies each configuration to the destination bucket. As a result, important settings such as bucket policy, lifecycle rules, encryption, CORS, tags, logging, and Object Lock can be recreated in a systematic way.

### Orchestrate the workflow with AWS Step Functions

AWS Step Functions acts as the orchestrator for the entire workflow. Instead of running multiple separate operations manually, Step Functions divides the process into clear steps such as creating the bucket, copying configurations, checking status, and handling errors. If any step does not return a successful status, the workflow moves to a failed state and records the result.

### Centralized logging and auditing

Each workflow execution is recorded in Amazon DynamoDB. The information includes the source bucket, destination bucket, execution status, and a snapshot of the copied configuration. In addition, Amazon CloudWatch records detailed logs from each Lambda function and the execution history of Step Functions. This is very important for enterprises that require a clear audit trail.

### Suitable for cross-Region environments within the same AWS account

This solution is designed to replicate bucket configurations between Regions within the same AWS account. The Lambda functions use IAM roles created by AWS CDK to read configurations from the source bucket and apply configurations to the destination bucket.

━━━━━━━━━━━━━━━

## HOW DOES THE SOLUTION ARCHITECTURE WORK?

━━━━━━━━━━━━━━━

At a high level, the workflow consists of two main Lambda functions orchestrated by AWS Step Functions.

The first Lambda function is responsible for creating the destination bucket in the target Region. After the bucket is created, this function writes an initial record into DynamoDB to mark that the replication process has started.

The second Lambda function reads the configurations from the source bucket and then applies those configurations to the destination bucket. If the source bucket has server access logging enabled, this function can also create a dedicated logging bucket in the destination Region to ensure that the copied logging configuration points to a valid bucket.

AWS Step Functions checks the returned status of each step. If the bucket creation step succeeds, the workflow continues to the configuration copying step. If the configuration copying step completes successfully, the workflow ends with a successful status. Otherwise, if any error occurs in any step, the workflow moves to a failed state and updates the result in DynamoDB.

The workflow can be described as follows:

Source S3 Bucket → Step Functions Workflow → Lambda Create Bucket → Destination S3 Bucket → Lambda Copy Configuration → DynamoDB Audit Table & CloudWatch Logs

In this architecture:

- The Source Bucket is the original bucket that contains the configurations to be copied.
- The Destination Bucket is the new bucket created in the destination Region.
- AWS Step Functions orchestrates the entire process.
- AWS Lambda performs the bucket creation and configuration copying operations.
- Amazon DynamoDB stores execution history and configuration snapshots.
- Amazon CloudWatch records detailed logs for monitoring and debugging.

━━━━━━━━━━━━━━━

## S3 CONFIGURATIONS THAT CAN BE COPIED

━━━━━━━━━━━━━━━

The strength of this solution is that it does not only create a new bucket, but also attempts to recreate many important bucket-level configurations.

Supported configurations include:

- Bucket policy.
- Lifecycle rules.
- Versioning, including MFA Delete settings under certain conditions.
- Server-side encryption, including SSE-S3 and SSE-KMS.
- CORS rules.
- Server access logging.
- Bucket tags.
- Block Public Access settings.
- Bucket ownership controls.
- Bucket ACLs where applicable.
- Object Lock configuration.
- Requester Pays.
- Static website hosting configuration.

As a result, the destination bucket will have a configuration that is as close as possible to the source bucket, instead of being only an empty bucket in a new Region.

━━━━━━━━━━━━━━━

## REAL-WORLD SCENARIO

━━━━━━━━━━━━━━━

Assume that a financial enterprise is operating thousands of S3 buckets in the `us-east-1` Region. These buckets contain reports, system logs, analytics files, and data used by multiple departments.

Due to new data compliance or data residency requirements, the enterprise needs to recreate the corresponding buckets in another Region, such as `us-east-2`, to support infrastructure expansion or regional disaster recovery.

If this is done manually, the Cloud operations team must check each bucket one by one:

- Which lifecycle rules does this bucket have?
- Is encryption enabled?
- Which IAM roles are allowed in the bucket policy?
- Is access logging or Object Lock enabled?
- Are there tags used for cost allocation?

For a few buckets, this may still be acceptable. However, for hundreds or thousands of buckets, manual work becomes nearly impossible.

By using the AWS Step Functions and Lambda solution, the enterprise only needs to provide the source bucket and destination Region. The workflow automatically creates the destination bucket, copies configurations, records logs, and stores audit results.

This greatly reduces operational effort, minimizes manual configuration errors, and creates a repeatable process.

━━━━━━━━━━━━━━━

## WHY NOT ONLY USE INFRASTRUCTURE AS CODE?

━━━━━━━━━━━━━━━

One interesting point in the AWS article is that this solution is not positioned as a complete replacement for Infrastructure as Code.

If an enterprise is building a new system from scratch, the best approach is still to use AWS CloudFormation or AWS CDK to define buckets and their configurations as templates. This makes infrastructure consistent, easier to version control, and easier to redeploy.

However, in reality, enterprises do not always start with a clean environment. Many buckets may have existed for years, been created by different teams, no longer have the original scripts, or are not managed through IaC.

In such cases, copying configurations from the actual running bucket is more useful. This solution is suitable for legacy buckets, long-running environments, or scenarios where existing bucket configurations need to be replicated quickly to a new Region.

In other words:

- IaC is suitable for new systems that are designed properly from the beginning.
- The configuration replication workflow is suitable for existing systems that need to copy their current real-world state.

━━━━━━━━━━━━━━━

## IMPORTANT CONSIDERATIONS BEFORE DEPLOYMENT

━━━━━━━━━━━━━━━

Although this solution is useful, there are still several important considerations.

First, AWS KMS keys are Region-dependent resources. If the source bucket uses SSE-KMS, the destination Region must have a suitable KMS key so that Lambda can apply the encryption configuration. The key cannot simply be copied mechanically from the source Region to the destination Region.

Second, if the source bucket uses Object Lock and the destination bucket also needs Object Lock enabled, the destination bucket must be created with Object Lock from the beginning. This is a special S3 setting and cannot always be enabled later after the bucket has already been created.

Third, bucket policies are copied as raw JSON. This means that if the source bucket policy contains Resource ARNs that directly reference the source bucket, the copied policy may still point to the old ARN after being applied to the destination bucket. Therefore, the operations team must review and update bucket policies when they contain ARNs tightly coupled to the source bucket.

Fourth, the workflow does not automatically roll back if a configuration fails during the copying process. Configurations are applied in order. If one step fails, the workflow stops and marks the execution as `FAILED` in DynamoDB. The operator then needs to check CloudWatch Logs, resolve the root cause, and rerun the workflow.

Fifth, for buckets with very large configurations, such as long bucket policies or many complex lifecycle rules, Lambda timeout or memory settings may need to be adjusted to match the actual workload.

━━━━━━━━━━━━━━━

## WHAT I LEARNED FROM THIS ARTICLE

━━━━━━━━━━━━━━━

After reading this article, I realized a very practical point: managing S3 at enterprise scale is not simply about creating buckets and uploading objects. Bucket-level configuration is where many important factors related to security, compliance, cost, and operations are stored.

S3 Cross-Region Replication can help replicate object data between Regions, but bucket configuration remains a separate problem. Without an automation process, manually recreating configurations can easily create inconsistencies between the source and destination environments.

What I find interesting in this architecture is that AWS uses Step Functions to turn a complex sequence of operations into a clear workflow with states, error checking, and an audit trail. Lambda handles the configuration copying logic, DynamoDB stores history, and CloudWatch supports observability and debugging. This is a practical combination of serverless services on AWS.

In my opinion, this article is very useful for anyone studying to become a Cloud Engineer, DevOps Engineer, or Solutions Architect. It shows how AWS solves a real operational problem: not only deploying new resources, but also standardizing and automating existing resources in a running system.

━━━━━━━━━━━━━━━

## CONCLUSION

━━━━━━━━━━━━━━━

The solution for replicating Amazon S3 bucket configurations across AWS Regions using AWS Step Functions and AWS Lambda is a practical model for enterprises that operate many long-running buckets on AWS.

Instead of opening each bucket and manually copying every configuration, this workflow automatically creates the destination bucket, reapplies important settings, records detailed logs, and stores execution history for auditing.

This solution does not completely replace Infrastructure as Code, but it complements IaC very well in scenarios where enterprises need to handle legacy buckets, migrate to a new Region, meet data compliance requirements, or standardize configurations at scale.

Through this article, I clearly learned an important lesson in Cloud system design: automation is not only for application deployment, but also necessary for configuration management, security, and long-term infrastructure operations.

A good Cloud system is not only a system that works. It must also be repeatable, controllable, observable, and able to minimize manual operational errors.

![alt text](/my-hugo-site/images/3-Blogpost/khoa.png)


## Practice Evidence

![alt text](/my-hugo-site/images/3-Blogpost/image.png)

Reference link:  
https://www.facebook.com/groups/660548818043427/my_pending_content