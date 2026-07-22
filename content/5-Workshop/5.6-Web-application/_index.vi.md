---
title: "Web Application"
date: 2026-07-10
weight: 6
chapter: false
pre: "<b>5.6. </b>"
---


## Introduction

Sau khi hoàn thành các thành phần Backend và Video Processing Pipeline, nhóm tiến hành xây dựng giao diện Web nhằm cung cấp môi trường để người dùng tương tác với hệ thống. Ứng dụng web cho phép người dùng đăng nhập, tải video, theo dõi trạng thái xử lý và phát video sau khi quá trình chuyển mã hoàn tất.

Giao diện được thiết kế đơn giản, trực quan và kết nối với các dịch vụ AWS thông qua Amazon API Gateway và AWS Lambda.

---

## Mục tiêu

Sau khi hoàn thành phần này, ứng dụng web sẽ hỗ trợ:

- Đăng nhập người dùng.
- Tải video lên hệ thống.
- Hiển thị danh sách video.
- Theo dõi trạng thái xử lý.
- Phát video trực tuyến.

---

# Chức năng đăng nhập

## Thực hiện

Người dùng truy cập ứng dụng và đăng nhập bằng tài khoản đã được cấu hình trên Amazon Cognito. Sau khi xác thực thành công, hệ thống chuyển người dùng đến trang quản lý video.

![Login](/my-hugo-site/images/5-Workshop/5.5-Policy/30-login-page.png)

*Hình 5.29. Giao diện đăng nhập.*

**Kết quả:** Người dùng được xác thực thành công và có thể sử dụng các chức năng của hệ thống.

---

# Chức năng tải video

## Thực hiện

Người dùng chọn tệp video từ máy tính và thực hiện tải lên thông qua giao diện Upload. Ứng dụng gửi yêu cầu đến API để tạo Presigned URL và tải trực tiếp video lên Amazon S3.

![Upload Page](/my-hugo-site/images/5-Workshop/5.5-Policy/31-upload-page.png)

*Hình 5.30. Giao diện tải video.*

Sau khi quá trình tải lên hoàn tất, hệ thống hiển thị thông báo thành công.

![Upload Success](/my-hugo-site/images/5-Workshop/5.5-Policy/32-upload-success.png)

*Hình 5.31. Tải video thành công.*

**Kết quả:** Video được lưu vào Raw Upload Bucket và Pipeline xử lý được kích hoạt tự động.

---

# Chức năng quản lý video

## Thực hiện

Ứng dụng truy vấn dữ liệu từ Amazon DynamoDB thông qua API và hiển thị danh sách các video đã tải lên cùng trạng thái xử lý.

![Video List](/my-hugo-site/images/5-Workshop/5.5-Policy/33-video-list.png)

*Hình 5.32. Danh sách video.*

**Kết quả:** Người dùng có thể theo dõi thông tin và trạng thái của từng video.

---

# Chức năng phát video

## Thực hiện

Khi video được xử lý hoàn tất, người dùng chọn video trong danh sách để phát trực tiếp trên trình duyệt. Ứng dụng sử dụng đường dẫn HLS được lưu trong Metadata để phát nội dung thông qua Amazon CloudFront.

![Video Player](/my-hugo-site/images/5-Workshop/5.5-Policy/34-video-player.png)

*Hình 5.33. Giao diện phát video.*

**Kết quả:** Video được phát ổn định và có thể xem trực tiếp trên trình duyệt.

---

# Dashboard hệ thống

## Thực hiện

Dashboard hiển thị các thông tin tổng quan của hệ thống như danh sách video, trạng thái xử lý và kết quả sau khi chuyển mã.

![Dashboard](/my-hugo-site/images/5-Workshop/5.5-Policy/35-system-dashboard.png)

*Hình 5.34. Dashboard của ứng dụng.*

**Kết quả:** Người dùng có thể dễ dàng theo dõi toàn bộ quá trình xử lý video thông qua một giao diện thống nhất.

---

## Tổng kết

Ứng dụng web đã được triển khai thành công và kết nối đầy đủ với các dịch vụ AWS trong kiến trúc serverless. Người dùng có thể thực hiện toàn bộ quy trình từ đăng nhập, tải video, theo dõi trạng thái xử lý đến phát video trên cùng một giao diện. Điều này giúp hoàn thiện nền tảng **Serverless Video-on-Demand Platform on AWS** và đáp ứng mục tiêu của đồ án.
