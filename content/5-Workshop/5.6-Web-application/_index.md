---
title: "Web Application"
date: 2026-07-10
weight: 6
chapter: false
pre: "<b>5.6. </b>"
---


## Introduction

After implementing the Backend and the Video Processing Pipeline, the final step is to develop the web application. The application provides a user-friendly interface that allows users to interact with the system, including authentication, video upload, video management, and online video playback.

The web application communicates with AWS services through Amazon API Gateway and AWS Lambda, while video content is delivered from Amazon S3 via Amazon CloudFront.

---

## Objectives

After completing this section, the web application will support the following features:

- User authentication.
- Video upload.
- Video management.
- Video playback.
- Viewing processing status.

---

# User Authentication

## Procedure

Users access the application and sign in using an account configured in Amazon Cognito. After successful authentication, they are redirected to the main dashboard.

![Login Page](/my-hugo-site/images/5-Workshop/5.5-Policy/30-login-page.png)

*Figure 5.29. User login page.*

**Result:** Users are authenticated successfully and can access all application features.

---

# Video Upload

## Procedure

Users select a video from their local computer and upload it through the upload page. The application requests a Presigned URL from the Backend API and uploads the video directly to Amazon S3.

![Upload Page](/my-hugo-site/images/5-Workshop/5.5-Policy/31-upload-page.png)

*Figure 5.30. Video upload page.*

After the upload is completed, the application displays a confirmation message.

![Upload Success](/my-hugo-site/images/5-Workshop/5.5-Policy/32-upload-success.png)

*Figure 5.31. Successful video upload.*

**Result:** The video is uploaded successfully, and the processing pipeline starts automatically.

---

# Video Management

## Procedure

The application retrieves video metadata from Amazon DynamoDB through the Backend API and displays a list of uploaded videos together with their processing status.

![Video List](/my-hugo-site/images/5-Workshop/5.5-Policy/33-video-list.png)

*Figure 5.32. Video management page.*

**Result:** Users can view video information and monitor the processing progress.

---

# Video Playback

## Procedure

After a video has been processed successfully, users can select it from the list and play it directly in the browser. The application streams the HLS content delivered through Amazon CloudFront.

![Video Player](/my-hugo-site/images/5-Workshop/5.5-Policy/34-video-player.png)

*Figure 5.33. Video playback page.*

**Result:** Processed videos are streamed smoothly through the web application.

---

# System Dashboard

## Procedure

The dashboard provides an overview of uploaded videos, processing status, and available media content.

![Dashboard](/my-hugo-site/images/5-Workshop/5.5-Policy/35-system-dashboard.png)

*Figure 5.34. System dashboard.*

**Result:** Users can easily monitor the current status of the entire video processing workflow.

---

## Summary

The web application was successfully integrated with the serverless backend and AWS services. Users can authenticate, upload videos, monitor processing progress, manage uploaded content, and stream processed videos through a single interface. This completes the implementation of the **Serverless Video-on-Demand Platform on AWS** and demonstrates the practicality of a serverless architecture for building scalable video streaming applications.
