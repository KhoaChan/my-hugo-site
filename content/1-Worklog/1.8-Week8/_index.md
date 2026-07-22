---
title: "Week 8 Worklog"
date: 2026-06-05
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---


## I. Executive Summary

After completing the overall system architecture design in the previous week, in Week 8 I continued moving into the technical design refinement stage before starting the first implementation activities of the internship topic. The main objective of this week was to review the entire architecture, analyze each system component in more detail, and identify how each module could be implemented during the development process.

During the research process, I continued referring to AWS Documentation, AWS Prescriptive Guidance, and several Video-on-Demand architecture models on AWS to evaluate whether the selected architecture was suitable. Through self-review and analysis, I adjusted several components to improve scalability, reduce dependencies between services, and make the implementation process more practical.

At the same time, I started designing the data model for Amazon DynamoDB, building a list of business APIs expected to be implemented through Amazon API Gateway and AWS Lambda, and defining the data flow between system components.

By the end of the week, I had completed the basic technical design documents, creating a foundation for moving into the system implementation stage in the following week.

---

## II. Strategic Objectives

After defining the overall architecture, the objective of Week 8 was to make the technical components more specific in preparation for system development.

The main objectives included:

- Review and refine the overall architecture based on research and analysis.
- Design the data model for Amazon DynamoDB.
- Identify business APIs and the communication method between the Frontend and Backend.
- Prepare the project structure and divide the system into functional modules.
- Build a detailed implementation plan for each development phase.
- Ensure that design decisions meet requirements for scalability, performance, and operational cost.

Through these objectives, I aimed to create a consistent technical foundation before starting the first development activities of the system.

---

## III. Activity Log & Detailed Schedule Allocation (From 09/06/2026 to 15/06/2026)

| Time | Activity Category | Detailed Tasks Performed | Results / Evidence Achieved |
| --- | --- | --- | --- |
| Day 1 (09/06) | Architecture Review | Reviewed Architecture Draft V1, checked the data flow, the role of each AWS service, and the relationship between system components. | Completed an improved architecture version for the technical design stage. |
| Day 2 (10/06) | Database Design | Designed the data model on Amazon DynamoDB, defined the Partition Key, Sort Key, and metadata fields required for video records. | Completed the initial data design for the system. |
| Day 3 (11/06) | API Design | Identified the APIs to be implemented, including HTTP methods, Request/Response structure, and the expected authentication flow through Amazon Cognito. | Completed the API list for the main system functions. |
| Day 4 (12/06) | Processing Workflow Design | Analyzed the Video Upload, Video Processing, and Video Streaming flows; identified data exchange between Amazon S3, SQS, Lambda, MediaConvert, and DynamoDB. | Completed the business processing flow of the system. |
| Day 5 (13/06) | Development Environment Preparation | Set up the project structure, organized the source code folders, and prepared the development environment for both Backend and Frontend. | Completed the initial project structure. |
| Day 6 (14/06) | Implementation Planning | Divided the system into functional modules such as Authentication, Video Upload, Video Processing, and Video Streaming; built a phased development plan. | Completed the detailed implementation roadmap for the following weeks. |
| Day 7 (15/06) | Design Review and Summary | Reviewed all technical documents, checked the consistency of architecture, API, data model, and implementation direction. | Completed the technical design documents and prepared for the programming stage. |

---

## IV. Advanced Technical Implementation & Detailed Analysis

After completing the overall architecture design in the previous week, I continued focusing on refining the technical components at the design level to prepare for the implementation stage. The objective of this week was not to develop specific features yet, but to convert the high-level architecture into technical documents that could be used directly during programming.

During the research process, I continued referring to official AWS Documentation, the AWS Well-Architected Framework, and several open-source Video-on-Demand models to evaluate the suitability of the architecture. By comparing different implementation approaches, I adjusted several components to simplify the processing flow, reduce dependencies between services, and ensure future scalability.

One of the key tasks of this week was designing the data model for the system. After analyzing the business requirements, I continued choosing Amazon DynamoDB as the main database for storing video metadata. The expected data fields include Video ID, Video Name, Upload Time, Processing Status, Owner, Category, and the paths to the processed video files.

In addition, I initially researched the use of Global Secondary Indexes (GSI) to support queries by processing status or video category. Designing the data model based on access patterns from the beginning helps reduce the need to change the table structure when the system expands.

At the same time, I identified the list of APIs that would be implemented in the first phase of the topic. The APIs were divided into several groups, including Authentication, Video Management, and Video Streaming. For each API, I defined the HTTP method, input data, response data, and the expected authentication mechanism through Amazon Cognito.

For the video processing workflow, I continued analyzing the data flow between Amazon S3, Amazon SQS, AWS Lambda, and AWS Elemental MediaConvert. Instead of letting the Backend directly handle the entire upload and video conversion process, I kept the asynchronous processing direction to reduce API load and improve system scalability.

Besides designing the data model and APIs, I also started building the project folder structure to create consistency during implementation. The Backend and Frontend were separated into two independent parts, making it easier to develop and test each module. I also clearly defined the boundaries between modules such as Authentication, Video Upload, Video Processing, and Video Streaming to prepare for the implementation stage.

By the end of the week, although I had not yet started coding specific features, I had completed most of the necessary technical design documents. This became an important foundation to reduce major changes during development and support the system implementation in the following weeks.

---

## V. Infrastructure Challenges, Error Resolution Logs & Expert Perspectives

During the process of refining the technical design, I realized that building a Serverless system is not simply about choosing AWS services. It also requires clearly defining the responsibility boundaries between each component. Without a consistent design from the beginning, developing modules independently may lead to incompatibility between the Backend, Database, and event processing workflow.

One of the difficulties I focused on was data organization in Amazon DynamoDB. Unlike relational databases, DynamoDB requires design based on query patterns instead of only data normalization. This required me to spend time identifying the Partition Key, Sort Key, indexed attributes, and future data expansion strategy.

In addition, building the API list also raised several questions related to consistency in the programming interface. I decided to follow the RESTful API approach and define naming conventions for endpoints, JSON Request/Response structures, and HTTP status codes to create clarity before programming.

Besides design-related issues, I also focused on the system’s future scalability. Instead of designing only for the current functions, I tried to build an architecture that could support additional features such as Video Recommendation, Comment, or Playlist without requiring major changes to the existing components.

Through the research and analysis process, I realized that investing time in the design stage can significantly reduce technical risks during implementation and create a stable foundation for developing system functions.

---

## VI. Professional Reflections

Week 8 helped me better understand the role of the design stage in the development process of a Cloud system. Although I did not directly build business functions yet, analyzing data, designing APIs, and defining the processing flow between AWS services helped me clearly visualize how the components would work together during implementation.

By referring to official AWS documents and real-world projects, I realized that a good architecture should not only satisfy current requirements but also support future expansion and maintenance. The decisions made during the design stage will directly affect system performance, operational cost, and future development capability.

In addition, this working week helped me improve my skills in requirement analysis, solution design, and technical documentation organization. Establishing technical conventions before programming is necessary to reduce major changes when the topic enters the development stage.

Overall, although no specific functions were built during this week, I created a relatively complete technical design foundation and was ready to move into the implementation of the first system components.

---

## VII. Strategic Planning & Optimization Roadmap for Next Week

In the following week, I plan to officially start the implementation stage based on the technical documents created during the last two weeks.

The main focus of the next week will be preparing the AWS infrastructure for development, including configuring basic services such as Amazon Cognito, Amazon S3, Amazon DynamoDB, Amazon API Gateway, and AWS Lambda. At the same time, I will build the first foundational modules of the system, such as user authentication, database connection, and basic APIs for video management.

Alongside Backend development, I will also test each component immediately after completion to ensure stability before integrating it with other modules. Implementing the system step by step will help me detect issues more easily and adjust the architecture when necessary without affecting the entire system.

The objective of the following week is to complete the initial technical foundation of the system, creating a basis for developing the Video Upload workflow and Video Processing Pipeline in the next phases of the topic.
