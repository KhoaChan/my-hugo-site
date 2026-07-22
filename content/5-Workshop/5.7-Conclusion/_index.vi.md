---
title: "Conclusion"
date: 2026-07-10
weight: 7
chapter: false
pre: "<b>5.7. </b>"
---


Trong chương Workshop, nhóm đã trình bày toàn bộ quá trình triển khai hệ thống **Serverless Video-on-Demand Platform on AWS** từ việc chuẩn bị môi trường, xây dựng Backend, thiết lập Video Processing Pipeline, kiểm thử hệ thống cho đến phát triển ứng dụng web.

Backend được xây dựng bằng các dịch vụ serverless của AWS như Amazon API Gateway, AWS Lambda và Amazon DynamoDB để quản lý metadata video. Quá trình tải video sử dụng Presigned URL giúp người dùng tải trực tiếp lên Amazon S3, giảm tải cho Backend và nâng cao hiệu năng hệ thống.

Video Processing Pipeline được triển khai theo kiến trúc hướng sự kiện. Khi người dùng tải video lên Raw Upload Bucket, Amazon EventBridge, Amazon SQS, EventBridge Pipes và AWS Step Functions phối hợp kích hoạt AWS Elemental MediaConvert để tự động chuyển mã video sang định dạng HLS. Sau khi hoàn thành, kết quả được lưu vào Processed Media Bucket và metadata được cập nhật để phục vụ việc phát video.

Kết quả kiểm thử cho thấy toàn bộ quy trình hoạt động ổn định. Người dùng có thể đăng nhập, tải video, theo dõi trạng thái xử lý và phát video trực tiếp trên giao diện web. Các dịch vụ AWS được tích hợp thành một hệ thống hoàn chỉnh, đảm bảo khả năng mở rộng, tính sẵn sàng cao và giảm chi phí vận hành nhờ mô hình serverless.

Thông qua quá trình triển khai và đánh giá, nhóm đã hoàn thành mục tiêu xây dựng một nền tảng xem video theo yêu cầu trên AWS. Hệ thống là cơ sở để tiếp tục phát triển các tính năng nâng cao như quản lý người dùng, phân quyền truy cập, thống kê lượt xem, tối ưu chất lượng phát video và mở rộng quy mô triển khai trong tương lai.
