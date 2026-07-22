---
title: "System Testing"
date: 2026-07-10
weight: 5
chapter: false
pre: "<b>5.5. </b>"
---


## Introduction

After completing the Backend and the Video Processing Pipeline, the next step is to test the entire system to ensure that all components are correctly integrated. The testing process follows the complete workflow, from user authentication and video upload to automatic video processing and playback on the web application.

The test results are used to verify the functionality, stability, and reliability of the serverless architecture.

---

## Objectives

After this section, the system should be able to:

- Authenticate users successfully.
- Upload videos to Amazon S3.
- Trigger the processing pipeline automatically.
- Update video metadata correctly.
- Play processed videos on the web application.

---

# Test User Authentication

## Procedure

Open the web application and sign in using an account configured in Amazon Cognito.

![Login Page](/my-hugo-site/images/5-Workshop/5.5-Policy/30-login-page.png)

*Figure 5.23. User login page.*

## Result

The user is authenticated successfully and redirected to the video management page.

---

# Test Video Upload

## Procedure

Select a video file from the local computer and upload it through the web interface.

![Upload Page](/my-hugo-site/images/5-Workshop/5.5-Policy/31-upload-page.png)

*Figure 5.24. Video upload page.*

After the upload is completed, the application displays a success notification.

![Upload Success](/my-hugo-site/images/5-Workshop/5.5-Policy/32-upload-success.png)

*Figure 5.25. Successful video upload.*

## Result

The uploaded video is stored in the Raw Upload Bucket, and the processing pipeline starts automatically.

---

# Test Video List

## Procedure

After the MediaConvert job is completed, open the video list page to verify that the uploaded video is displayed.

![Video List](/my-hugo-site/images/5-Workshop/5.5-Policy/33-video-list.png)

*Figure 5.26. Video list page.*

## Result

The application correctly displays the video metadata and processing status.

---

# Test Video Playback

## Procedure

Select a processed video from the list and play it directly in the browser.

![Video Player](/my-hugo-site/images/5-Workshop/5.5-Policy/34-video-player.png)

*Figure 5.27. Video playback page.*

## Result

The video is streamed successfully using HLS files stored in Amazon S3 and delivered through Amazon CloudFront.

---

# Verify the Dashboard

## Procedure

Open the system dashboard to review video information and processing status.

![Dashboard](/my-hugo-site/images/5-Workshop/5.5-Policy/35-system-dashboard.png)

*Figure 5.28. System dashboard.*

## Result

The dashboard displays accurate information about uploaded videos and their processing status.

---

## Summary

The testing results confirm that the system functions as expected. Users can sign in, upload videos, monitor processing progress, and stream processed videos through the web application. All AWS services operate together successfully, demonstrating that the **Serverless Video-on-Demand Platform on AWS** has been implemented correctly.
