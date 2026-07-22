---
title: "Proposal"
date: 2026-06-05
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

## Introduction

With the rapid growth of digital transformation in education, entertainment, corporate training, and online media services, the demand for Video-on-Demand (VoD) platforms has increased significantly. Modern users expect to upload, manage, and stream video content anytime and anywhere with high availability, fast response time, and support for multiple devices.

However, developing a Video-on-Demand platform using a traditional server-based architecture presents several challenges. Application servers are responsible for handling user requests, storing video files, managing metadata, transcoding media, and delivering content simultaneously. As the number of users and uploaded videos increases, the system becomes more difficult to scale and requires continuous infrastructure expansion, resulting in higher operational costs and maintenance efforts.

Furthermore, video transcoding is a computationally intensive process. Executing transcoding tasks directly on the application server consumes significant computing resources and may negatively affect the responsiveness of other services. This architecture also increases system complexity and reduces flexibility when handling concurrent workloads.

To address these challenges, the group proposes the development of a **Serverless Video-on-Demand Platform on Amazon Web Services (AWS)**. The proposed solution adopts a **Serverless Architecture** combined with an **Event-Driven Architecture**, enabling the entire workflow of uploading, processing, storing, and delivering video content to be automated using fully managed AWS services.

The proposed platform leverages Amazon Cognito, Amazon API Gateway, AWS Lambda, Amazon S3, Amazon DynamoDB, Amazon EventBridge, Amazon SQS, Amazon EventBridge Pipes, AWS Step Functions, AWS Elemental MediaConvert, Amazon CloudFront, AWS WAF, and Amazon CloudWatch. By integrating these services, the system is expected to provide high scalability, improved reliability, enhanced security, simplified infrastructure management, and cost optimization through a pay-as-you-go model.

This proposal serves as the foundation for the analysis, design, implementation, testing, and evaluation phases presented in the following chapters of this internship report.

## Problem Statement and Proposed Solution

### Problem Statement

Modern Video-on-Demand platforms must support numerous functions, including user authentication, video uploading, metadata management, media transcoding, content distribution, and playback services. When these responsibilities are handled by a single monolithic application, the system quickly becomes difficult to maintain and scale as traffic increases.

Traditional architectures often require manual server provisioning, load balancing configuration, operating system maintenance, and infrastructure monitoring. These tasks increase both deployment complexity and operational expenses while reducing development efficiency.

Another significant challenge is the media transcoding process. Uploaded videos usually need to be converted into multiple resolutions and streaming formats before being delivered to end users. Performing these computationally intensive operations directly on the application server may degrade overall system performance and reduce service availability.

In addition, the platform should support asynchronous processing to ensure that multiple uploaded videos can be processed simultaneously without blocking other user requests. This capability is essential for modern cloud-native media applications.

### Proposed Solution

To overcome these limitations, the group proposes implementing the system using a **Serverless Video-on-Demand Platform** built entirely on Amazon Web Services.

User authentication is performed through Amazon Cognito. After successful authentication, users submit requests to Amazon API Gateway. AWS Lambda processes business logic and generates pre-signed URLs that allow users to upload video files directly to the Amazon S3 Raw Upload Bucket without routing large files through the backend application.

Once a video has been successfully uploaded, Amazon EventBridge detects the object creation event and forwards it to Amazon SQS. Amazon EventBridge Pipes then transfers the message to AWS Step Functions, which orchestrates the complete media processing workflow. AWS Elemental MediaConvert performs video transcoding and converts uploaded videos into HTTP Live Streaming (HLS) format for adaptive video delivery.

Processed media files are stored in the Amazon S3 Processed Media Bucket. At the same time, AWS Lambda updates video processing status and metadata in Amazon DynamoDB, allowing users to monitor processing progress through the web interface.

Finally, Amazon CloudFront distributes processed video content globally through AWS edge locations, providing low-latency video playback and improved user experience. Amazon CloudWatch continuously collects logs, metrics, and operational events to support system monitoring and performance analysis.

The proposed Serverless and Event-Driven Architecture enables the platform to consume computing resources only when required, minimizes infrastructure management efforts, improves scalability, and provides a flexible foundation for future system expansion.

## Proposed Architecture

The proposed system architecture follows a layered cloud-native design in which each AWS service is responsible for a dedicated function. Communication between services is performed through REST APIs or event-driven messaging, allowing individual components to evolve independently while maintaining loose coupling across the entire platform.

![Screenshot](/my-hugo-site/images/2-Proposal/image.png)

**Figure 2.1. Proposed architecture of the Serverless Video-on-Demand Platform on AWS.**

The architecture consists of several logical layers, including the Identity and Access Layer, Edge and Content Delivery Layer, Application Layer, Data Layer, Event Processing Layer, Media Processing Layer, and Monitoring Layer.

## Proposed Architecture Description

The first layer of the proposed architecture is the **Identity and Access Layer**, where Amazon Cognito is responsible for user authentication and authorization. Amazon Cognito provides user registration, sign-in, identity management, and JSON Web Token (JWT) generation. After successful authentication, the generated JWT is attached to every API request, ensuring that only authorized users are allowed to access backend services.

The **Edge and Content Delivery Layer** consists of Amazon CloudFront, AWS WAF, and the Amazon S3 Frontend Bucket. The web application is hosted as a static website in Amazon S3, while Amazon CloudFront distributes both the frontend assets and processed video content through AWS Edge Locations worldwide. AWS WAF is deployed in front of CloudFront to inspect incoming HTTP requests, mitigate common web application attacks, and improve the overall security posture of the platform.

The **Application Layer** is implemented using Amazon API Gateway and AWS Lambda. Amazon API Gateway acts as the unified entry point for all client requests and securely routes them to the corresponding Lambda functions. Each Lambda function performs a dedicated business operation, including generating pre-signed upload URLs, managing video metadata, retrieving video information, updating processing status, and communicating with Amazon DynamoDB. Since Lambda follows the Function-as-a-Service (FaaS) model, computing resources are allocated only when requests are received, significantly reducing operational costs.

Within the **Data Layer**, Amazon DynamoDB is proposed as the primary NoSQL database for storing video metadata. Each record contains essential information such as the video identifier, file name, upload timestamp, processing status, playback URL, thumbnail location, and other application-related metadata. DynamoDB is selected because of its high availability, low latency, automatic scaling capability, and seamless integration with other AWS serverless services.

Uploaded video files are initially stored in the **Amazon S3 Raw Upload Bucket**. Whenever a new object is created, Amazon EventBridge automatically detects the upload event and generates an event notification. The event is delivered to Amazon SQS, which serves as a message queue separating the upload process from downstream media processing tasks. This asynchronous communication mechanism enables the platform to process multiple uploaded videos simultaneously without affecting frontend responsiveness.

Amazon EventBridge Pipes continuously retrieves messages from Amazon SQS and forwards them to AWS Step Functions. Step Functions orchestrates the entire media processing workflow by coordinating each processing stage, monitoring execution progress, handling failures, and maintaining workflow consistency. This orchestration layer simplifies workflow management while improving maintainability and fault tolerance.

The **Media Processing Layer** is powered by AWS Elemental MediaConvert. MediaConvert automatically transcodes uploaded videos into HTTP Live Streaming (HLS) format with multiple resolutions suitable for adaptive bitrate streaming. The generated output files are stored in the Amazon S3 Processed Media Bucket, allowing users to stream video content efficiently under different network conditions and across multiple device types.

The final layer is the **Monitoring and Observability Layer**, which utilizes Amazon CloudWatch to collect logs, metrics, and operational events generated by every AWS service involved in the platform. CloudWatch enables developers to monitor system health, analyze application performance, detect failures, and receive alerts when abnormal activities occur, thereby supporting stable and reliable system operation.

## Implementation Plan

The proposed implementation is divided into multiple phases to reduce project risks and simplify system integration. The first phase focuses on establishing the core infrastructure, including user authentication, cloud storage, metadata management, and RESTful APIs. These components provide the fundamental services required by the platform.

The second phase introduces the asynchronous event-driven workflow by integrating Amazon EventBridge, Amazon SQS, and Amazon EventBridge Pipes. This stage enables automatic event processing whenever users upload new video files to Amazon S3.

After completing the event-driven infrastructure, AWS Step Functions and AWS Elemental MediaConvert are integrated to automate the entire media transcoding workflow. The generated HLS content is then stored in Amazon S3 and distributed globally through Amazon CloudFront.

Finally, Amazon CloudWatch is configured to monitor system activities, collect application logs, evaluate service performance, and support troubleshooting during deployment and operation. Each component is individually tested before full system integration to ensure reliability and reduce implementation risks.

## Estimated Cost

The proposed platform prioritizes fully managed serverless services to minimize infrastructure investment and operational expenses. Services such as AWS Lambda, Amazon API Gateway, Amazon EventBridge, Amazon SQS, and AWS Step Functions follow a pay-as-you-use pricing model, allowing computing resources to be consumed only when requests are processed.

Amazon S3 provides highly durable and cost-effective storage for both uploaded and processed media files, while Amazon CloudFront charges based on actual content delivery traffic. Because infrastructure resources scale automatically according to workload, the proposed architecture is expected to maintain relatively low operating costs during development while remaining capable of supporting future business growth.

## Expected Outcomes

The proposed architecture is expected to provide a modern cloud-native Video-on-Demand platform capable of supporting secure user authentication, efficient video uploads, automated media processing, and high-performance content delivery.

Users are expected to upload videos directly to Amazon S3 using pre-signed URLs, monitor processing progress through the web application, and stream HLS content distributed by Amazon CloudFront. The entire media processing pipeline will operate automatically through an event-driven workflow without requiring manual intervention.

Furthermore, the adoption of AWS serverless technologies is expected to reduce infrastructure management efforts, improve scalability, increase system availability, and optimize operational costs compared with traditional server-based architectures.

## Conclusion

The proposed architecture satisfies the functional and non-functional requirements of a cloud-based Video-on-Demand platform by combining Serverless Computing with Event-Driven Architecture. Each AWS service performs a clearly defined responsibility, resulting in a loosely coupled, scalable, maintainable, and highly available system.

This proposal establishes a solid architectural foundation for the subsequent analysis, system design, implementation, testing, and evaluation phases presented in the following chapters. By leveraging fully managed AWS services, the proposed platform aims to deliver a secure, scalable, and cost-effective Video-on-Demand solution while demonstrating the practical advantages of modern cloud-native application development.
