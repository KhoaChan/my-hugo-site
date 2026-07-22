---
title: "Build Video Processing Pipeline"
date: 2026-07-10
weight: 4
chapter: false
pre: "<b>5.4. </b>"
---


## Introduction

Sau khi Backend được triển khai thành công, bước tiếp theo là xây dựng **Video Processing Pipeline** nhằm tự động xử lý video sau khi người dùng tải lên hệ thống. Pipeline được xây dựng theo kiến trúc hướng sự kiện (Event-driven Architecture), trong đó mỗi dịch vụ AWS đảm nhận một nhiệm vụ riêng và phối hợp với nhau để thực hiện quá trình chuyển mã video.

Trong đồ án này, quy trình xử lý bắt đầu khi video được tải lên **Amazon S3 Raw Upload Bucket**. Sự kiện từ Amazon S3 sẽ được gửi đến Amazon EventBridge, chuyển vào Amazon SQS, sau đó thông qua EventBridge Pipes kích hoạt AWS Step Functions. Cuối cùng, AWS Elemental MediaConvert thực hiện chuyển mã video sang định dạng HLS và lưu kết quả vào Processed Media Bucket.

---

## Mục tiêu

Sau khi hoàn thành phần này, hệ thống sẽ có khả năng:

- Tự động phát hiện video mới.
- Kích hoạt quy trình xử lý video.
- Chuyển mã video bằng MediaConvert.
- Lưu video sau xử lý vào Amazon S3.
- Chuẩn bị dữ liệu cho chức năng phát video.

---

# Tạo Dead Letter Queue

## Thực hiện

Truy cập **Amazon SQS** và tạo một **Dead Letter Queue (DLQ)** để lưu các thông điệp không thể xử lý thành công.

![Create DLQ](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/17-create-processing-dlq.png)

*Hình 5.10. Tạo Dead Letter Queue.*

**Kết quả:** DLQ được tạo thành công và sẵn sàng tiếp nhận các thông điệp lỗi nếu xảy ra trong quá trình xử lý.

---

# Tạo Amazon SQS

## Thực hiện

Tiếp tục tạo một **Standard Queue** dùng để nhận sự kiện từ Amazon EventBridge trước khi chuyển đến AWS Step Functions.

![Create Queue](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/18-create-processing-queue.png)

*Hình 5.11. Tạo Amazon SQS.*

**Kết quả:** Queue hoạt động và sẵn sàng nhận thông điệp.

---

# Bật EventBridge cho Amazon S3

## Thực hiện

Mở **Raw Upload Bucket**, chọn **Properties** và kích hoạt **Amazon EventBridge notifications** để Bucket có thể gửi sự kiện khi xuất hiện tệp mới.

![Enable EventBridge](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/19-enable-s3-eventbridge.png)

*Hình 5.12. Bật EventBridge cho Amazon S3.*

**Kết quả:** Amazon S3 có thể gửi sự kiện Object Created đến EventBridge.

---

# Cấu hình EventBridge Rule

## Thực hiện

Tạo một **Rule** mới trong Amazon EventBridge với nguồn sự kiện là Amazon S3 và Target là Amazon SQS vừa tạo.

![EventBridge Rule](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/20-eventbridge-target-sqs.png)

*Hình 5.13. Cấu hình EventBridge Rule.*

**Kết quả:** Sự kiện từ Amazon S3 được chuyển đến Amazon SQS.

---

# Kiểm tra Amazon SQS

## Thực hiện

Tải thử một video lên Raw Upload Bucket, sau đó mở Amazon SQS để kiểm tra số lượng Message đã nhận.

![SQS Message](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/21-sqs-message-received.png)

*Hình 5.14. Amazon SQS nhận thông điệp.*

**Kết quả:** Queue nhận thành công thông điệp từ EventBridge.

---

# Tạo AWS Step Functions

## Thực hiện

Tạo một **State Machine** mới trên AWS Step Functions để điều phối quy trình xử lý video.

![Step Functions](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/22-stepfunctions-create.png)

*Hình 5.15. Tạo AWS Step Functions.*

**Kết quả:** Workflow được tạo thành công.

---

# Cấu hình MediaConvert Task

## Thực hiện

Trong State Machine, thêm bước gọi **AWS Elemental MediaConvert** và cấu hình Input, Output cho Job chuyển mã.

![MediaConvert Task](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/23.png)

*Hình 5.16. Thêm MediaConvert Task.*

![MediaConvert Input](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/24-mediaconvert-input.png)

*Hình 5.17. Cấu hình Input của MediaConvert.*

![MediaConvert Output](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/25-mediaconvert-output.png)

*Hình 5.18. Cấu hình Output của MediaConvert.*

**Kết quả:** Workflow có thể tạo MediaConvert Job để xử lý video.

---

# Tạo EventBridge Pipes

## Thực hiện

Tạo **EventBridge Pipe**, chọn Amazon SQS làm Source và AWS Step Functions làm Target để tự động chuyển thông điệp giữa hai dịch vụ.

![EventBridge Pipes](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/26-eventbridge-pipe-running.png)

*Hình 5.19. Tạo EventBridge Pipes.*

**Kết quả:** EventBridge Pipes hoạt động và tự động kích hoạt Workflow.

---

# Kiểm tra Pipeline

## Thực hiện

Tải thử một video lên hệ thống và theo dõi quá trình thực thi trên AWS Step Functions.

![Workflow Success](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/27-stepfunctions-execution-succeeded.png)

*Hình 5.20. Workflow thực thi thành công.*

Tiếp tục kiểm tra MediaConvert để xác nhận Job đã hoàn tất.

![MediaConvert Complete](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/28-mediaconvert-job-complete.png)

*Hình 5.21. MediaConvert hoàn thành chuyển mã.*

Cuối cùng, mở **Processed Media Bucket** và kiểm tra các tệp đầu ra.

![Processed Output](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/29-processed-video-output.png)

*Hình 5.22. Video sau xử lý được lưu trên Amazon S3.*

**Kết quả:** Video được chuyển mã thành công và lưu vào Processed Media Bucket dưới định dạng HLS. Pipeline xử lý video đã hoạt động hoàn chỉnh và sẵn sàng phục vụ chức năng phát video trên ứng dụng web.

---

## Tổng kết

Trong phần này, nhóm đã triển khai thành công Video Processing Pipeline bằng cách kết hợp Amazon EventBridge, Amazon SQS, EventBridge Pipes, AWS Step Functions và AWS Elemental MediaConvert. Sau khi hoàn thành, toàn bộ quá trình xử lý video được thực hiện tự động ngay khi người dùng tải video lên hệ thống. Kết quả đầu ra được lưu trên Amazon S3 và sẵn sàng cho việc phát trực tuyến ở phần Web Application.
