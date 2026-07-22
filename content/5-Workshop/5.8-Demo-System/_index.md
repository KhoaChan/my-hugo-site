---
title: "Demo-System"
date: 2026-07-10
weight: 8
chapter: false
pre: "<b>5.8. </b>"
---


# System Demo

## Overview

After completing the deployment and testing process, the **Serverless Video-on-Demand Platform on AWS** successfully met all the required functions of the MVP version. The video below demonstrates the complete system workflow, from the moment a user uploads a video, through processing on AWS, until the video is ready for playback on the website.

---

## Demo Content

The demo video presents the full operating process of the system based on the implemented architecture, including:

- Logging in to the system.
- Uploading a video through the Web Application.
- Checking the uploaded video in the Amazon S3 Raw Upload Bucket.
- Monitoring the processing workflow using AWS Elemental MediaConvert.
- Checking the output video in the Amazon S3 Processed Media Bucket.
- Confirming that metadata is updated in Amazon DynamoDB.
- Playing the video directly on the website after the processing is completed.

---

## Demo Video

👉 **Attached Materials: Video Demo & Source Code**

https://drive.google.com/drive/folders/1wSLqL126i6bPS9XuX5uhclzD6j7tYVGf?usp=sharing

The Google Drive folder includes:

- Source code of the **Serverless Video-on-Demand Platform on AWS** project.
- Demo video showing the full deployment and operation process of the system.

---

## Result

The demo video confirms that the system operates correctly according to the designed Serverless architecture. After a user uploads a video, the system automatically processes the video, stores the output files, updates the metadata, and makes the video available for playback on the website.

This result shows that all system components have been successfully integrated and operate continuously according to the proposed workflow.
