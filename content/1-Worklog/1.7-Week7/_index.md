---
title: "Week 7 Worklog"
date: 2026-05-29
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---


## I. Executive Summary

Week 7 marked an important transition stage in my internship process. After completing the learning and practice activities related to foundational AWS services in the previous weeks, I began focusing on summarizing my knowledge, analyzing requirements, and building the initial direction for my internship topic.

Instead of continuing with separate lab exercises, I spent this week systematizing the AWS knowledge I had learned in order to select a suitable solution for the **Mini Video-on-Demand Platform using AWS Serverless**.

During this week, I analyzed the business requirements of the system, researched AWS architecture models, and evaluated the suitability of each AWS service for different components of the system. Based on the research results, I selected a **Serverless Architecture** combined with an **Event-Driven Architecture** to take advantage of flexible scalability, reduce operational costs, and minimize manual infrastructure management.

At the same time, I also created the first version of the overall architecture and identified the roles of services such as **Amazon S3**, **Amazon API Gateway**, **AWS Lambda**, **Amazon Cognito**, **Amazon DynamoDB**, **Amazon CloudFront**, **Amazon SQS**, and **AWS Elemental MediaConvert** in each stage of the system workflow.

By the end of the week, I had completed the initial architecture draft, identified the roadmap for the next phases, and prepared the necessary documents to support the completion of my internship report.

---

## II. Strategic Objectives

After completing the foundational learning activities, the main objective of Week 7 was to shift from learning individual AWS services to researching and designing a solution for my internship topic.

The main objectives of this week included:

- Summarizing and systematizing all AWS knowledge learned in previous weeks to support system design.
- Analyzing the business requirements of the Video-on-Demand platform and identifying the core functions that need to be implemented.
- Researching AWS architecture models and evaluating the advantages and disadvantages of each approach before selecting the Serverless model.
- Identifying suitable AWS services for each system component, such as user authentication, business logic processing, data storage, video processing, and content delivery.
- Building the first version of the overall architecture design (**Architecture Draft V1**) as a foundation for the following weeks.
- Planning the development content in phases to ensure progress and consistency in the internship report.

Through these objectives, I aimed to form an initial architecture that is feasible and create a solid foundation for continuing to improve the technical content of the report.

---

## III. Activity Log & Detailed Schedule Allocation (From 02/06/2026 to 08/06/2026)

| Time | Activity Category | Detailed Tasks Performed | Results / Evidence Achieved |
| --- | --- | --- | --- |
| Day 1 (02/06) | AWS Knowledge Summary | Reviewed and systematized the knowledge learned about Compute, Storage, Database, Networking, Security, and Serverless during the internship program. | Completed summary notes about AWS services and the technology selection direction for the system. |
| Day 2 (03/06) | Business Requirement Analysis | Analyzed the requirements of the Video-on-Demand system and identified key functions such as user management, video upload, video processing, and online video streaming. | Built a description of the functional scope and business workflow of the system. |
| Day 3 (04/06) | Deployment Solution Research | Studied AWS architecture models and evaluated the applicability of Serverless Architecture and Event-Driven Architecture for video processing. | Selected Serverless Architecture as the main direction for the topic. |
| Day 4 (05/06) | Overall Architecture Design | Identified the main system components and the relationship between the Frontend, Backend, Data Layer, and Video Processing Pipeline. | Completed the first version of the overall system architecture design. |
| Day 5 (06/06) | AWS Service Selection | Evaluated and selected AWS services such as Amazon S3, API Gateway, Lambda, Cognito, DynamoDB, CloudFront, SQS, and MediaConvert for each component. | Completed the list of AWS services planned for the topic. |
| Day 6 (07/06) | Implementation Roadmap Planning | Divided the system into functional modules and built a phased implementation plan to support the completion of the report content. | Completed the implementation roadmap and content development direction. |
| Day 7 (08/06) | Summary and Content Preparation | Reviewed the overall architecture, summarized research materials, and prepared content for the next sections of the worklog. | Completed the Week 7 Worklog content and prepared for the following week. |

---

## IV. Advanced Technical Implementation & Detailed Analysis

After completing the learning activities related to AWS services, I moved into a transition stage from studying individual services to analyzing a complete system for my internship report. Instead of only documenting lab steps, I focused on analyzing the problem, researching the architecture, and selecting a suitable solution for the topic.

Through the research process, I selected the topic **Mini Video-on-Demand Platform using AWS Serverless**. The goal of the system is to build a platform that allows administrators to upload videos, after which the system automatically processes the videos, converts them into multiple quality levels, and delivers the content to users through a CDN.

During this week, I did not focus on implementing each function in detail. Instead, I prioritized building the first version of the overall architecture design (**Architecture Draft V1**). This architecture serves as the direction for completing the content and implementation in the following weeks.

### Initial Serverless Video-on-Demand Architecture

![Week 7 Screenshot](/my-hugo-site/images/1-Workblog/Week-7/image.png)

Based on the initial architecture, I divided the system into multiple functional layers to reduce dependencies between components and make the most of AWS managed and serverless services.

### 1. Frontend Layer

For the user interface layer, I planned to use **React** or **Next.js** to build the web application because these frameworks support modern interface development and can be extended in the future.

The frontend is expected to be deployed as a **Static Website** on Amazon S3 and distributed through Amazon CloudFront. Using CloudFront helps reduce access latency, improve page loading speed, and support better scalability when the number of users increases.

This is also a common deployment model for Serverless applications where the frontend is separated from the backend.

### 2. API and Authentication Layer

For the API service layer, I selected **Amazon API Gateway** as the communication gateway between the frontend and backend.

The APIs are expected to follow a RESTful architecture. They will receive requests from users and forward them to business logic functions running on AWS Lambda.

At the same time, **Amazon Cognito** was selected to handle user authentication and user management. Instead of building a login system manually, Cognito provides User Pool management, JWT Token support, and authorization mechanisms. This helps reduce development time while improving security.

At this stage, I only identified the role of Cognito in the overall architecture and had not yet configured User Pools or detailed Authorization Flows.

### 3. Business Logic Layer

The business logic of the system is planned to be implemented using **AWS Lambda**.

Using Lambda helps remove the need to manage servers and only generates cost when processing requests. This is suitable for a Video-on-Demand system because user traffic may change over time.

In the initial design, I identified several main Lambda function groups, including:

- Business API processing.
- Generating Presigned URLs for video upload.
- Updating video processing status.
- Handling events generated from the processing pipeline.

The Lambda functions are expected to operate independently and communicate through API Gateway, Amazon SQS, or EventBridge to reduce dependencies between system components.

### 4. Data Layer

For the data storage layer, I selected **Amazon DynamoDB** to store video metadata.

The expected information to be stored includes:

- Video ID.
- Video title.
- Processing status.
- Storage path.
- Created time.
- Category.
- Uploader information.

The reason for choosing DynamoDB instead of a relational database is its flexible scalability, high performance, and suitability for Serverless architecture.

During the research stage, I also proposed using **Global Secondary Indexes (GSI)** to support queries by video status and category. However, the detailed table structure will continue to be improved in the following week.

### 5. Video Processing Pipeline

This is considered the most important component of the entire system.

I planned to build the video processing pipeline using an **Event-Driven Architecture** to separate the upload process from the video processing process.

The expected workflow is as follows:

- The administrator requests to upload a video.
- The backend generates a Presigned URL.
- The video is uploaded directly to the Amazon S3 Raw Bucket.
- After the upload is completed, the system generates an event.
- Amazon SQS receives the processing request.
- AWS Lambda starts a job on AWS Elemental MediaConvert.
- MediaConvert converts the video into multiple resolutions.
- The output is stored in the Processed Bucket.
- EventBridge sends a completion event.
- Lambda updates the video status in DynamoDB.

This asynchronous model prevents the backend from waiting for the video conversion process to finish, thereby improving scalability and user experience.

### 6. Content Delivery

After processing is completed, the video will be delivered through **Amazon CloudFront**.

CloudFront acts as a CDN and helps:

- Reduce access latency.
- Improve video streaming speed.
- Reduce direct load on S3.
- Use caching at Edge Locations.

The architecture is also expected to support **HLS Streaming**, allowing users to watch videos at different quality levels depending on their network speed.

### 7. Security and Monitoring Direction

In addition to the main functional components, I also initially identified several supporting services needed to meet operational requirements.

Some services expected to be used include:

- **AWS IAM** to manage access permissions based on the Least Privilege principle.
- **AWS KMS** to encrypt stored data.
- **Amazon CloudWatch** to monitor logs and system activities.
- **AWS CloudTrail** to record activity history.
- **AWS WAF** to strengthen protection for public APIs.

Since this was only the initial research stage, I had not yet implemented detailed configurations for these services. I only identified the role of each component in the overall architecture.

---

## V. Infrastructure Challenges, Error Resolution Logs & Expert Perspectives

Since Week 7 mainly focused on research and initial architecture design, I did not encounter many practical implementation errors like in previous lab weeks. However, during the analysis and system design process, I identified several important challenges that need to be considered before moving into the implementation stage.

The first challenge was related to choosing the appropriate architecture for the video processing workflow. Video files are usually large and require a long processing time. If the system uses a synchronous processing model through API Gateway and Lambda, it may easily cause timeout issues, increase waiting time, and negatively affect user experience. To solve this problem, I planned to use asynchronous processing through Amazon S3, Amazon SQS, AWS Lambda, and AWS Elemental MediaConvert.

The next challenge was uploading large video files from the frontend to the system. If all video data has to pass through the backend or API Gateway, it may increase processing load, data transfer cost, and the risk of exceeding request size limits. Therefore, I proposed using Presigned URLs so that the frontend can upload videos directly to the Amazon S3 Raw Bucket. The backend will only be responsible for authenticating users, generating temporary URLs, and storing necessary metadata.

In addition, the data organization between Amazon S3 and Amazon DynamoDB needs to be designed carefully. Amazon S3 is responsible for storing original and processed video files, while DynamoDB stores metadata, processing status, and query-related information. If the Video ID, Object Key, and processing status are not standardized from the beginning, data may become inconsistent between the two services. Therefore, I planned to use a single Video ID as the linking key between DynamoDB, the storage structure in S3, and MediaConvert jobs.

Another issue discussed was repeated event processing. In an event-driven architecture, a message from Amazon SQS or EventBridge may be delivered again if processing fails. If Lambda is not designed to be idempotent, the same video may create multiple MediaConvert jobs or update the status multiple times. Therefore, I identified the need to check the current video status in DynamoDB before performing important operations and to add a Dead-Letter Queue in a later implementation stage.

From a security perspective, I realized that the initial architecture includes many components communicating with each other, and each component must be granted appropriate permissions. Overly broad permissions may create unauthorized access risks, while overly restricted permissions may prevent the system from working correctly. The planned solution is to create separate IAM Roles for each Lambda function, apply the Least Privilege principle, and limit access permissions by specific Bucket, Table, Queue, or MediaConvert Job.

In addition, content delivery through CloudFront must ensure that the S3 Processed Bucket is not publicly accessible directly. I planned to keep all buckets private and allow CloudFront to access content through Origin Access Control. This helps prevent users from directly accessing S3 Object URLs and increases control over the video delivery flow.

From a professional perspective, I realized that a good Cloud architecture must not only meet functional requirements but also balance performance, scalability, security, reliability, and cost. The current architecture is only the first version, so some components such as AWS WAF, KMS, SNS, SES, X-Ray, and AWS Config are still being considered based on necessity.

---

## VI. Professional Reflections

Week 7 did not focus on detailed technical implementation but served as a transition stage from learning to designing a practical system. Although there was not much programming work during this week, it was still an important week because architectural decisions will directly affect the process of completing the topic in the following weeks.

Through the process of summarizing knowledge and analyzing business requirements, I realized that choosing the right architecture is just as important as implementing features. A clearly designed architecture helps make the development process more directed and reduces the need for major changes when entering the implementation stage.

In addition, I also had the opportunity to review the relationship between the AWS services I had learned throughout the program. Previously, each service was approached through separate labs. However, when placed into a complete system, I understood more clearly how services such as Amazon API Gateway, AWS Lambda, Amazon Cognito, Amazon S3, Amazon DynamoDB, Amazon SQS, Amazon CloudFront, and AWS Elemental MediaConvert can work together to create a unified processing workflow.

Besides technical knowledge, this week also helped me improve my ability to analyze requirements, present ideas, and make design decisions based on criteria such as scalability, performance, security, and operational cost.

After this week, I realized that investing time in the research and architecture design stage creates a solid foundation for future implementation and helps reduce risks when the system becomes more complex.

---

## VII. Strategic Planning & Optimization Roadmap for Next Week

After completing the first version of the overall architecture design, I will move to the stage of refining the technical design and preparing to implement the first components of the system.

In the following week, I plan to review and adjust the architecture based on feedback from the instructor and self-evaluation results to ensure feasibility before starting development. At the same time, I will design more details for important components such as the data model in Amazon DynamoDB, the Amazon S3 Bucket structure, the user authentication process using Amazon Cognito, and business APIs implemented through Amazon API Gateway and AWS Lambda.

In addition, I will prepare the development environment, build the documentation structure, set up a source code repository if necessary, divide the tasks that need to be completed, and build an implementation roadmap for each functional module. This will be an important preparation step before moving into the implementation phase in the following weeks.

The goal of the next week is not only to complete the technical design but also to create a consistent development foundation. This will help the implementation of functions such as Authentication, Video Upload, Video Processing Pipeline, and Video Streaming become more effective and reduce major changes throughout the lifecycle of the topic.
