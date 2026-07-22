---
title: "Conclusion"
date: 2026-07-10
weight: 7
chapter: false
pre: "<b>5.7. </b>"
---


In this workshop, the complete implementation process of the **Serverless Video-on-Demand Platform on AWS** has been presented, including environment preparation, backend development, video processing pipeline deployment, system testing, and web application implementation.

The backend was developed using fully managed AWS services, including Amazon API Gateway, AWS Lambda, Amazon DynamoDB, and Amazon S3. By using Presigned URLs, video files are uploaded directly to Amazon S3, reducing backend workload and improving overall system performance.

The video processing pipeline follows an event-driven architecture. When a new video is uploaded to the Raw Upload Bucket, Amazon EventBridge captures the event and forwards it to Amazon SQS. EventBridge Pipes then triggers AWS Step Functions, which creates an AWS Elemental MediaConvert job to transcode the video into HLS format. The processed files are stored in the Processed Media Bucket, while the video metadata is updated for playback.

System testing confirms that all components work together as expected. Users can authenticate through Amazon Cognito, upload videos, monitor processing progress, and stream processed videos through the web application. The serverless architecture provides automatic scalability, high availability, and reduced operational overhead without requiring server management.

Overall, the project successfully demonstrates how AWS serverless services can be integrated to build a complete Video-on-Demand platform. The implemented system satisfies the project's objectives and provides a solid foundation for future enhancements, such as user management, access control, video analytics, recommendation features, and large-scale deployment.
