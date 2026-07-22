---
title: "Build Video Processing Pipeline"
date: 2026-07-10
weight: 4
chapter: false
pre: "<b>5.4. </b>"
---

## Introduction

After completing the Backend, the next step is to build the **Video Processing Pipeline**, which automatically processes uploaded videos. The pipeline follows an event-driven architecture, allowing AWS services to work together to perform video transcoding without manual intervention.

In this project, the workflow starts when a user uploads a video to the **Amazon S3 Raw Upload Bucket**. Amazon EventBridge captures the upload event and forwards it to Amazon SQS. EventBridge Pipes then triggers AWS Step Functions, which creates an AWS Elemental MediaConvert job. Finally, the transcoded HLS files are stored in the Processed Media Bucket for video streaming.

---

## Objectives

After completing this section, the system will be able to:

- Detect newly uploaded videos.
- Automatically trigger the processing workflow.
- Transcode videos using MediaConvert.
- Store processed videos in Amazon S3.
- Prepare videos for online streaming.

---

# Create a Dead Letter Queue

## Procedure

Open **Amazon SQS** and create a **Dead Letter Queue (DLQ)** to store messages that cannot be processed successfully.

![Create DLQ](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/17-create-processing-dlq.png)

*Figure 5.10. Creating a Dead Letter Queue.*

**Result:** The DLQ is created successfully and is ready to receive failed messages.

---

# Create an Amazon SQS Queue

## Procedure

Create a **Standard Queue** to receive events from Amazon EventBridge before triggering the processing workflow.

![Create Queue](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/18-create-processing-queue.png)

*Figure 5.11. Creating an Amazon SQS queue.*

**Result:** The queue is successfully created and ready to receive messages.

---

# Enable EventBridge Notifications

## Procedure

Open the **Raw Upload Bucket**, navigate to **Properties**, and enable **Amazon EventBridge notifications**.

![Enable EventBridge](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/19-enable-s3-eventbridge.png)

*Figure 5.12. Enabling EventBridge notifications.*

**Result:** Amazon S3 can publish object creation events to Amazon EventBridge.

---

# Configure an EventBridge Rule

## Procedure

Create an **EventBridge Rule** with Amazon S3 as the event source and Amazon SQS as the target.

![EventBridge Rule](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/20-eventbridge-target-sqs.png)

*Figure 5.13. Configuring the EventBridge rule.*

**Result:** Upload events are successfully delivered to Amazon SQS.

---

# Verify Amazon SQS

## Procedure

Upload a sample video to the Raw Upload Bucket and verify that a new message appears in Amazon SQS.

![SQS Message](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/21-sqs-message-received.png)

*Figure 5.14. Amazon SQS receives the upload event.*

**Result:** The queue successfully receives the upload notification.

---

# Create AWS Step Functions

## Procedure

Create a new **State Machine** in AWS Step Functions to coordinate the video processing workflow.

![Step Functions](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/22-stepfunctions-create.png)

*Figure 5.15. Creating AWS Step Functions.*

**Result:** The workflow is successfully deployed.

---

# Configure the MediaConvert Task

## Procedure

Add an **AWS Elemental MediaConvert** task to the workflow and configure its input and output settings.

![MediaConvert Task](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/23.png)

*Figure 5.16. Adding the MediaConvert task.*

![MediaConvert Input](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/24-mediaconvert-input.png)

*Figure 5.17. Configuring the MediaConvert input.*

![MediaConvert Output](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/25-mediaconvert-output.png)

*Figure 5.18. Configuring the MediaConvert output.*

**Result:** The workflow can create MediaConvert jobs for uploaded videos.

---

# Create EventBridge Pipes

## Procedure

Create an **EventBridge Pipe** using Amazon SQS as the source and AWS Step Functions as the target.

![EventBridge Pipes](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/26-eventbridge-pipe-running.png)

*Figure 5.19. Creating EventBridge Pipes.*

**Result:** EventBridge Pipes automatically forwards messages to AWS Step Functions.

---

# Verify the Processing Pipeline

## Procedure

Upload a test video and monitor the execution in AWS Step Functions.

![Workflow Success](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/27-stepfunctions-execution-succeeded.png)

*Figure 5.20. Successful Step Functions execution.*

Next, verify that the MediaConvert job finishes successfully.

![MediaConvert Complete](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/28-mediaconvert-job-complete.png)

*Figure 5.21. MediaConvert job completed.*

Finally, open the **Processed Media Bucket** and verify the generated HLS files.

![Processed Output](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/29-processed-video-output.png)

*Figure 5.22. Processed video stored in Amazon S3.*

**Result:** The uploaded video is successfully transcoded into HLS format and stored in the Processed Media Bucket. The video processing pipeline is fully operational and ready for video streaming.

---

## Summary

In this section, the complete video processing pipeline was implemented using Amazon EventBridge, Amazon SQS, EventBridge Pipes, AWS Step Functions, and AWS Elemental MediaConvert. Once deployed, every uploaded video is processed automatically, stored in Amazon S3, and prepared for playback in the web application.
