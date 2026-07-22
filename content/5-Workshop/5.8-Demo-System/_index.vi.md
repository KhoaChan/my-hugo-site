---
title: "Demo hệ thống"
date: 2026-07-10
weight: 8
chapter: false
pre: "<b>5.8. </b>"
---


## Tổng quan

Sau khi hoàn thành quá trình triển khai và kiểm thử, hệ thống **Serverless Video-on-Demand Platform on AWS** đã đáp ứng đầy đủ các chức năng cần thiết của phiên bản MVP. Video demo dưới đây minh họa toàn bộ quy trình hoạt động của hệ thống, từ lúc người dùng tải video lên, hệ thống xử lý trên nền tảng AWS cho đến khi video sẵn sàng để phát trên website.

---

## Nội dung Demo

Video demo trình bày đầy đủ quy trình hoạt động của hệ thống dựa trên kiến trúc đã triển khai, bao gồm:

- Đăng nhập vào hệ thống.
- Tải video lên thông qua Web Application.
- Kiểm tra video đã upload trong Amazon S3 Raw Upload Bucket.
- Theo dõi quá trình xử lý video bằng AWS Elemental MediaConvert.
- Kiểm tra video đầu ra trong Amazon S3 Processed Media Bucket.
- Xác nhận metadata được cập nhật trong Amazon DynamoDB.
- Phát video trực tiếp trên website sau khi quá trình xử lý hoàn tất.

---

## Video Demo

👉 **Tài liệu đính kèm: Video Demo & Source Code**

https://drive.google.com/drive/folders/1wSLqL126i6bPS9XuX5uhclzD6j7tYVGf?usp=sharing

Thư mục Google Drive bao gồm:

- Source code của dự án **Serverless Video-on-Demand Platform on AWS**.
- Video demo minh họa toàn bộ quá trình triển khai và vận hành của hệ thống.

---

## Kết quả

Video demo xác nhận rằng hệ thống hoạt động đúng theo kiến trúc Serverless đã thiết kế. Sau khi người dùng tải video lên, hệ thống tự động xử lý video, lưu trữ các file đầu ra, cập nhật metadata và cung cấp video sẵn sàng để phát trên website.

Kết quả này cho thấy tất cả các thành phần trong hệ thống đã được tích hợp thành công và hoạt động xuyên suốt theo đúng quy trình đã đề xuất.
