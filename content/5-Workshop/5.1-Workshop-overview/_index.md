---
title: "Workshop Overview"
date: 2026-07-10
weight: 1
chapter: false
pre: "<b>5.1. </b>"
---


## Introduction

This chapter presents the complete implementation process of the **Serverless Video-on-Demand Platform on AWS** using a serverless architecture. The workshop is based on the actual implementation carried out during the internship and provides a step-by-step guide to building a scalable Video-on-Demand system on Amazon Web Services (AWS).

Unlike the previous chapters, which focus on system analysis and design, this chapter emphasizes the practical deployment process. Each section includes detailed configuration steps, explanations, screenshots, and verification results, allowing readers to reproduce the entire system in their own AWS environment.

The platform is designed to minimize operational overhead by leveraging fully managed AWS services. Instead of maintaining traditional servers, the application relies on event-driven services that automatically scale based on workload, providing better availability, lower operational costs, and simplified system management.

---

## Workshop Objectives

After completing this workshop, readers will be able to deploy a complete serverless Video-on-Demand platform with the following capabilities:

- Build a serverless backend using AWS services.
- Store uploaded videos in Amazon S3.
- Manage video metadata using Amazon DynamoDB.
- Develop RESTful APIs with Amazon API Gateway.
- Process application logic using AWS Lambda.
- Automatically trigger video processing after uploads.
- Convert uploaded videos into HLS streaming format using AWS Elemental MediaConvert.
- Coordinate processing workflows with AWS Step Functions.
- Connect event-driven services through Amazon EventBridge, Amazon SQS, and EventBridge Pipes.
- Monitor application activities using Amazon CloudWatch.
- Deploy a web application for uploading and streaming videos.

Upon completion, the entire system will operate automatically without requiring manual intervention during video processing.

---

## System Architecture

The platform is divided into three major components:

- Frontend
- Backend
- Video Processing Pipeline

The frontend provides the user interface for authentication, video uploading, and video playback. Static web assets are hosted on Amazon S3 and distributed globally through Amazon CloudFront to improve performance and reduce latency.

The backend exposes REST APIs through Amazon API Gateway. Business logic is implemented using AWS Lambda, while video metadata is stored in the **Videos** table in Amazon DynamoDB.

The video processing pipeline operates independently from the backend. Once a user uploads a video to the Amazon S3 Raw Upload Bucket, an event-driven workflow automatically starts, processes the uploaded video, stores the output in the processed media bucket, and updates the processing status in DynamoDB.

Separating the processing workflow from the backend significantly improves scalability and reduces response time for user requests.

---

## AWS Services Used

This workshop utilizes the following AWS services throughout the implementation process.

| AWS Service | Purpose |
|-------------|---------|
| Amazon S3 | Store website files and video assets |
| Amazon CloudFront | Deliver web content globally |
| Amazon Cognito | Authenticate users |
| Amazon API Gateway | Provide REST APIs |
| AWS Lambda | Execute backend business logic |
| Amazon DynamoDB | Store video metadata |
| Amazon EventBridge | Receive storage events |
| Amazon SQS | Queue processing requests |
| EventBridge Pipes | Forward messages to Step Functions |
| AWS Step Functions | Coordinate processing workflow |
| AWS Elemental MediaConvert | Transcode uploaded videos |
| Amazon CloudWatch | Monitor logs and system activities |

Each service will be configured in detail throughout the following sections of this workshop.

---

## Overall Workflow

The system processes uploaded videos through an automated workflow.

1. A user signs in to the web application.
2. The frontend requests a Presigned URL from Amazon API Gateway.
3. AWS Lambda generates the Presigned URL and creates an initial metadata record in the **Videos** DynamoDB table.
4. The frontend uploads the selected video directly to the Amazon S3 Raw Upload Bucket.
5. Amazon S3 generates an Object Created event.
6. Amazon EventBridge receives the storage event.
7. The event is forwarded to Amazon SQS.
8. EventBridge Pipes transfers the queued message to AWS Step Functions.
9. AWS Step Functions starts an AWS Elemental MediaConvert job.
10. MediaConvert transcodes the uploaded video into HLS format.
11. The processed files are stored in the Amazon S3 Processed Media Bucket.
12. The video processing status is updated in DynamoDB.
13. The processed video becomes available for streaming through Amazon CloudFront.

This event-driven workflow enables automatic video processing while keeping the backend responsive and scalable.

---

## Workshop Structure

To simplify the implementation process, this workshop is organized into several sequential sections.

The first section introduces the prerequisites required before deployment, including AWS account preparation and necessary development tools.

The second section focuses on building the backend infrastructure, including Amazon S3, Amazon DynamoDB, AWS Lambda, and Amazon API Gateway.

Next, the workshop explains how to construct the video processing pipeline using Amazon EventBridge, Amazon SQS, EventBridge Pipes, AWS Step Functions, and AWS Elemental MediaConvert.

After completing the infrastructure deployment, the entire platform will be validated through a series of functional tests to verify that uploads, processing, metadata updates, and video playback operate correctly.

Finally, the workshop demonstrates the deployment of the web application and summarizes the implementation results obtained during the internship project.

---

## Expected Outcomes

After completing this workshop, readers will have successfully deployed a fully functional **Serverless Video-on-Demand Platform** on Amazon Web Services. The completed system demonstrates how multiple managed AWS services can be integrated into a scalable, event-driven architecture capable of handling video uploads, automated transcoding, metadata management, and online streaming without maintaining any application servers.

The implementation also illustrates how serverless technologies simplify infrastructure management while improving scalability, reliability, and operational efficiency.

The expected outcomes of this workshop include:

- A serverless backend deployed on AWS.
- A functional REST API accessible through Amazon API Gateway.
- Video metadata successfully stored in the **Videos** DynamoDB table.
- Direct client uploads to the Amazon S3 Raw Upload Bucket using Presigned URLs.
- Automatic execution of the video processing workflow after each upload.
- Successful HLS transcoding using AWS Elemental MediaConvert.
- Processed streaming files stored in the Amazon S3 Processed Media Bucket.
- Automatic metadata updates after video processing completes.
- A web application capable of displaying and streaming processed videos.
- Centralized monitoring through Amazon CloudWatch.

Together, these components form a complete serverless media processing platform that reflects a real-world cloud-native application.

---

## Workshop Conventions

To maintain consistency throughout this document, several conventions are adopted during the implementation process.

- Official AWS service names are preserved throughout the documentation.
- Resource names such as Amazon S3 buckets, DynamoDB tables, and Lambda functions remain consistent with the deployed environment.
- All screenshots are captured directly from the AWS Management Console during the actual implementation.
- Configuration procedures are demonstrated using the AWS Management Console instead of Infrastructure as Code tools.
- Source code examples focus only on the functionality required for each implementation step.

Readers may notice slight differences in the AWS console interface over time due to service updates. However, the configuration concepts and deployment workflow remain unchanged.

---

## Documentation Approach

This workshop follows a step-by-step implementation methodology. Every major deployment task begins with a brief objective, followed by detailed configuration instructions, screenshots, explanations, and verification results.

Instead of presenting only the final system, the workshop documents the complete deployment process from an empty AWS environment to a fully operational Video-on-Demand platform.

Each section has been organized to allow readers to reproduce the implementation independently while understanding the purpose of every AWS service involved.

Whenever a new AWS resource is created, its role within the overall architecture is explained before proceeding to the next deployment step. This approach helps readers understand not only *how* to configure each service but also *why* it is required.

---

## Workshop Organization

The remainder of this chapter is organized into the following sections.

### 5.2 Prerequisite

This section introduces the required AWS account, development tools, permissions, and resources that should be prepared before deployment begins.

### 5.3 Build Backend

The backend implementation covers the creation of Amazon S3 buckets, the DynamoDB metadata table, AWS Lambda functions, IAM permissions, and Amazon API Gateway integration. At the end of this section, the backend will be capable of generating Presigned URLs and accepting video uploads.

### 5.4 Build Video Processing Pipeline

This section explains how to construct the automated processing workflow using Amazon EventBridge, Amazon SQS, EventBridge Pipes, AWS Step Functions, and AWS Elemental MediaConvert. The workflow enables uploaded videos to be processed automatically without manual intervention.

### 5.5 System Testing

After deployment, the complete system is validated through a series of functional tests. Each component is verified to ensure that uploads, metadata creation, workflow execution, transcoding, and streaming operate correctly.

### 5.6 Web Application

The frontend application is deployed and connected to the backend services. Readers will learn how users authenticate, upload videos, monitor processing progress, browse available content, and stream processed videos through Amazon CloudFront.

### 5.7 Conclusion

The final section summarizes the implementation process, reviews the completed system, discusses the advantages of the serverless architecture, and outlines potential improvements for future development.

---

## Summary

This section introduced the objectives, architecture, workflow, AWS services, and organization of the implementation workshop for the **Serverless Video-on-Demand Platform on AWS**.

The following sections will guide readers through every deployment step in detail, beginning with environment preparation and backend configuration before progressing to the automated video processing pipeline, system testing, and web application deployment. By following this workshop sequentially, readers will be able to reproduce the complete platform and gain practical experience in building modern serverless applications on AWS.
