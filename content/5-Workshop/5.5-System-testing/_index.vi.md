---
title: "System Testing"
date: 2026-07-10
weight: 5
chapter: false
pre: "<b>5.5. </b>"
---


## Introduction

Sau khi hoàn thành Backend và Video Processing Pipeline, bước tiếp theo là kiểm thử toàn bộ hệ thống nhằm xác nhận các thành phần đã được tích hợp chính xác. Quá trình kiểm thử được thực hiện theo đúng luồng hoạt động của ứng dụng, từ khi người dùng đăng nhập, tải video lên hệ thống cho đến khi video được xử lý và phát thành công trên giao diện web.

Thông qua các bài kiểm thử, nhóm đánh giá khả năng hoạt động của từng dịch vụ AWS cũng như tính ổn định của toàn bộ kiến trúc serverless.

---

## Mục tiêu

Sau phần kiểm thử, hệ thống cần đáp ứng các yêu cầu sau:

- Người dùng đăng nhập thành công.
- Video được tải lên Amazon S3.
- Pipeline tự động xử lý video.
- Metadata được cập nhật.
- Video có thể phát trên giao diện web.

---

# Kiểm thử đăng nhập

## Thực hiện

Mở ứng dụng web và thực hiện đăng nhập bằng tài khoản đã được cấu hình trên Amazon Cognito.

![Login Page](/my-hugo-site/images/5-Workshop/5.5-Policy/30-login-page.png)

*Hình 5.23. Giao diện đăng nhập hệ thống.*

## Kết quả

Người dùng đăng nhập thành công và được chuyển đến trang quản lý video.

---

# Kiểm thử tải video

## Thực hiện

Chọn một tệp video từ máy tính và tải lên hệ thống thông qua chức năng Upload.

![Upload Page](/my-hugo-site/images/5-Workshop/5.5-Policy/31-upload-page.png)

*Hình 5.24. Giao diện tải video.*

Sau khi tải lên hoàn tất, hệ thống hiển thị thông báo thành công.

![Upload Success](/my-hugo-site/images/5-Workshop/5.5-Policy/32-upload-success.png)

*Hình 5.25. Video được tải lên thành công.*

## Kết quả

Video được lưu vào Raw Upload Bucket và quy trình xử lý tự động được kích hoạt.

---

# Kiểm thử danh sách video

## Thực hiện

Sau khi MediaConvert hoàn tất xử lý, truy cập trang danh sách video để kiểm tra dữ liệu.

![Video List](/my-hugo-site/images/5-Workshop/5.5-Policy/33-video-list.png)

*Hình 5.26. Danh sách video trên hệ thống.*

## Kết quả

Metadata được hiển thị chính xác và trạng thái video được cập nhật thành công.

---

# Kiểm thử phát video

## Thực hiện

Chọn một video trong danh sách và thực hiện phát trực tiếp trên trình duyệt.

![Video Player](/my-hugo-site/images/5-Workshop/5.5-Policy/34-video-player.png)

*Hình 5.27. Phát video trên giao diện web.*

## Kết quả

Video được phát thành công bằng dữ liệu HLS lưu trên Amazon S3 và phân phối thông qua Amazon CloudFront.

---

# Kiểm tra Dashboard

## Thực hiện

Quan sát Dashboard của hệ thống để kiểm tra trạng thái xử lý và các thông tin của video.

![Dashboard](/my-hugo-site/images/5-Workshop/5.5-Policy/35-system-dashboard.png)

*Hình 5.28. Dashboard theo dõi hệ thống.*

## Kết quả

Toàn bộ thông tin của video được hiển thị đầy đủ và phản ánh đúng trạng thái xử lý thực tế.

---

## Tổng kết

Kết quả kiểm thử cho thấy hệ thống hoạt động đúng theo thiết kế. Người dùng có thể đăng nhập, tải video, theo dõi trạng thái xử lý và phát video trực tiếp trên giao diện web. Đồng thời, các dịch vụ AWS phối hợp ổn định trong suốt quá trình xử lý, đáp ứng yêu cầu của nền tảng **Serverless Video-on-Demand Platform on AWS**.
