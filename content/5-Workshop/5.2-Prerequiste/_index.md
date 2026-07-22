---
title: "Prerequisite"
date: 2026-07-10
weight: 2
chapter: false
pre: "<b>5.2. </b>"
---


## Introduction

Before deploying the **Serverless Video-on-Demand Platform on AWS**, several prerequisites must be prepared to ensure that every service can be configured correctly. This section introduces the AWS resources, software, permissions, and development environment required throughout the implementation process.

Preparing these resources in advance helps avoid configuration errors and ensures that the deployment workflow can be completed without interruption. Although AWS automatically provisions most managed services, appropriate permissions and supporting tools are still required before the implementation begins.

---

## AWS Account

An active AWS account is required to deploy the platform. The account should have sufficient permissions to create and configure the AWS services used in this workshop.

The implementation requires access to the following services:

- Amazon S3
- Amazon CloudFront
- Amazon Cognito
- Amazon API Gateway
- AWS Lambda
- Amazon DynamoDB
- Amazon EventBridge
- Amazon SQS
- EventBridge Pipes
- AWS Step Functions
- AWS Elemental MediaConvert
- Amazon CloudWatch
- AWS Identity and Access Management (IAM)

Using a dedicated AWS account for development is recommended to simplify resource management and prevent interference with existing cloud workloads.

---

## Required Permissions

The IAM user or IAM role used during deployment should have sufficient permissions to create, modify, and delete AWS resources.

At a minimum, permissions should include:

- Amazon S3 Full Access
- Amazon DynamoDB Full Access
- AWS Lambda Full Access
- Amazon API Gateway Administrator
- AWS Step Functions Full Access
- Amazon EventBridge Full Access
- Amazon SQS Full Access
- AWS Elemental MediaConvert Full Access
- Amazon CloudWatch Full Access
- IAM Full Access

These permissions allow every deployment step in this workshop to be completed successfully without authorization errors.

---

## Development Environment

The implementation is performed through the AWS Management Console using a modern web browser.

The following software is recommended:

| Software | Purpose |
|----------|---------|
| Google Chrome | Access AWS Management Console |
| Visual Studio Code | Edit frontend source code |
| Node.js LTS | Build and run the web application |
| Git | Source code management |
| AWS CLI (Optional) | Command-line interaction with AWS |

Although most configurations are completed through the AWS console, installing these tools provides a more efficient development experience.

---

## Network Requirements

A stable Internet connection is required throughout the deployment process because all AWS services are configured remotely through the AWS Management Console.

Readers should also ensure that browser extensions or firewall policies do not block AWS service endpoints during deployment.

For organizations using restricted networks, outbound HTTPS access to AWS services should be allowed.

---

## Estimated Deployment Time

The complete workshop can typically be completed within two to three hours depending on network conditions and familiarity with AWS services.

Approximate durations are listed below.

| Section | Estimated Time |
|---------|---------------:|
| Backend Deployment | 45–60 minutes |
| Video Processing Pipeline | 45–60 minutes |
| System Testing | 20–30 minutes |
| Web Application Deployment | 20–30 minutes |

The longest waiting period usually occurs while AWS Elemental MediaConvert processes uploaded videos.

---

## AWS Region

Selecting an appropriate AWS Region is important because all services deployed in this workshop should reside in the same Region whenever possible. Keeping resources within a single Region reduces network latency, simplifies service integration, and minimizes data transfer costs.

For this project, all resources were deployed in the same AWS Region to ensure compatibility between Amazon S3, AWS Lambda, Amazon DynamoDB, AWS Step Functions, Amazon EventBridge, Amazon SQS, and AWS Elemental MediaConvert.

Readers should verify the selected Region before creating any AWS resource, as changing Regions later may require recreating several services.

---

## Resource Naming Convention

To improve readability and simplify resource management, all AWS resources should follow a consistent naming convention throughout the project.

Typical naming examples include:

| Resource | Example |
|----------|---------|
| DynamoDB Table | Videos |
| Raw Video Bucket | Raw Upload Bucket |
| Processed Video Bucket | Processed Media Bucket |
| Lambda Function | Upload Lambda |
| API Gateway | Video API |
| Step Functions | Video Processing Workflow |

Using meaningful resource names makes monitoring, troubleshooting, and future maintenance significantly easier.

---

## Estimated AWS Costs

The implementation described in this workshop is designed as a prototype and therefore incurs relatively low operational costs.

Most AWS services used in this project support a pay-as-you-go pricing model, meaning charges are based on actual resource consumption rather than fixed infrastructure costs.

The primary cost sources include:

- Amazon S3 storage.
- AWS Elemental MediaConvert processing jobs.
- Amazon CloudFront content delivery.
- AWS Lambda execution duration.
- Amazon API Gateway requests.

During development and testing, these costs remain relatively low because only a limited number of videos are processed.

Nevertheless, readers are encouraged to monitor their AWS Billing Dashboard regularly and remove unused resources after completing the workshop to avoid unnecessary charges.

---

## Source Code Preparation

Before deploying the backend, the project source code should be prepared locally.

The project consists of two major components:

- Backend source code implemented with AWS Lambda.
- Frontend web application for uploading and streaming videos.

The backend source code contains the Lambda functions responsible for generating Presigned URLs, managing video metadata, and supporting communication between AWS services.

The frontend application provides the user interface that allows authenticated users to upload videos, monitor processing progress, browse available content, and stream processed videos.

Keeping the project files organized before deployment simplifies the configuration process in the following sections.

---

## Verification Checklist

Before proceeding to the backend deployment, readers should verify that all prerequisites have been completed successfully.

The following checklist can be used as a quick reference.

- AWS account is active.
- Required IAM permissions are available.
- AWS Region has been selected.
- Development software has been installed.
- Stable Internet connectivity is available.
- Project source code has been prepared.
- AWS Management Console can be accessed successfully.

Completing this checklist helps reduce deployment issues in the later stages of the workshop.

---

## Summary

In this section, the required environment for deploying the **Serverless Video-on-Demand Platform on AWS** has been prepared. The prerequisites include an AWS account with appropriate permissions, a consistent AWS Region, the necessary development software, project source code, and Internet connectivity.

With these preparations completed, the deployment can proceed to the next stage, where the backend infrastructure will be implemented using Amazon S3, Amazon DynamoDB, AWS Lambda, IAM, and Amazon API Gateway. These services form the foundation of the entire serverless Video-on-Demand platform and will be configured step by step in the following section.
