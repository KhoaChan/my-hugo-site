---
title: "Workshop Overview"
date: 2026-07-10
weight: 1
chapter: false
pre: "<b>5.1. </b>"
---


## Giới thiệu

Chương Workshop trình bày toàn bộ quá trình triển khai hệ thống **Serverless Video-on-Demand Platform on AWS** bằng kiến trúc serverless trên nền tảng Amazon Web Services (AWS). Nội dung được xây dựng dựa trên quá trình triển khai thực tế của nhóm trong thời gian thực tập và được sắp xếp theo đúng trình tự từ chuẩn bị môi trường, xây dựng backend, cấu hình pipeline xử lý video cho đến kiểm thử và vận hành hệ thống.

Khác với phần thiết kế và phân tích ở các chương trước, chương này tập trung vào các bước triển khai thực tế. Mỗi bước đều đi kèm hình ảnh minh họa, giải thích cấu hình, mục đích thực hiện và kết quả đạt được để người đọc có thể dễ dàng tái hiện hệ thống trên tài khoản AWS của mình.

Toàn bộ hệ thống được xây dựng theo mô hình serverless nhằm giảm chi phí vận hành, tự động mở rộng tài nguyên và hạn chế việc quản lý máy chủ. Các dịch vụ AWS được kết nối với nhau thông qua API và cơ chế sự kiện, tạo thành một quy trình xử lý video hoàn toàn tự động từ khi người dùng tải video lên cho đến khi video sẵn sàng để phát trên website.

---

## Mục tiêu của Workshop

Sau khi hoàn thành toàn bộ Workshop, hệ thống sẽ đáp ứng đầy đủ các chức năng của một nền tảng Video-on-Demand cơ bản.

Các mục tiêu chính bao gồm:

- Xây dựng hạ tầng backend theo kiến trúc serverless.
- Tạo các Amazon S3 Bucket phục vụ lưu trữ video gốc và video sau xử lý.
- Xây dựng cơ sở dữ liệu metadata bằng Amazon DynamoDB.
- Triển khai AWS Lambda để xử lý các nghiệp vụ của hệ thống.
- Cung cấp REST API thông qua Amazon API Gateway.
- Tự động xử lý video bằng AWS Elemental MediaConvert.
- Kết nối các dịch vụ bằng Amazon EventBridge, Amazon SQS và EventBridge Pipes.
- Điều phối quy trình xử lý bằng AWS Step Functions.
- Theo dõi trạng thái hoạt động của hệ thống bằng Amazon CloudWatch.
- Triển khai giao diện web cho phép tải lên và phát video trực tuyến.

---

## Kiến trúc triển khai

Hệ thống được chia thành ba thành phần chính gồm Frontend, Backend và Video Processing Pipeline.

Frontend được triển khai trên Amazon S3 và phân phối thông qua Amazon CloudFront. Người dùng xác thực bằng Amazon Cognito trước khi sử dụng các chức năng của hệ thống.

Backend sử dụng Amazon API Gateway làm cổng tiếp nhận yêu cầu từ phía client. Mọi nghiệp vụ được xử lý thông qua AWS Lambda. Metadata của video được lưu trữ trong Amazon DynamoDB.

Pipeline xử lý video hoạt động độc lập với backend. Sau khi người dùng tải video lên Amazon S3 Raw Upload Bucket, sự kiện tạo đối tượng sẽ kích hoạt quy trình xử lý thông qua Amazon EventBridge, Amazon SQS, EventBridge Pipes và AWS Step Functions. AWS Elemental MediaConvert thực hiện chuyển mã video sang định dạng HLS, sau đó kết quả được lưu vào Amazon S3 Processed Media Bucket và cập nhật trạng thái trong DynamoDB.

Việc tách riêng pipeline xử lý giúp backend phản hồi nhanh hơn, đồng thời hệ thống có khả năng mở rộng linh hoạt khi số lượng video tăng lên.

---

## Các dịch vụ AWS sử dụng

Trong Workshop này, nhóm sử dụng các dịch vụ chính sau:

| Dịch vụ | Vai trò |
|---------|---------|
| Amazon S3 | Lưu trữ video và website |
| Amazon CloudFront | Phân phối nội dung |
| Amazon Cognito | Xác thực người dùng |
| Amazon API Gateway | Cung cấp REST API |
| AWS Lambda | Xử lý nghiệp vụ |
| Amazon DynamoDB | Lưu metadata video |
| Amazon EventBridge | Tiếp nhận sự kiện từ S3 |
| Amazon SQS | Hàng đợi xử lý |
| EventBridge Pipes | Kết nối SQS với Step Functions |
| AWS Step Functions | Điều phối workflow |
| AWS Elemental MediaConvert | Chuyển mã video |
| Amazon CloudWatch | Giám sát và ghi log |

Mỗi dịch vụ sẽ được cấu hình chi tiết trong các phần tiếp theo của Workshop.

---

## Quy trình hoạt động của hệ thống

Quy trình xử lý video được thực hiện theo các bước sau:

1. Người dùng đăng nhập vào hệ thống.
2. Frontend gửi yêu cầu tạo Presigned URL thông qua Amazon API Gateway.
3. AWS Lambda tạo Presigned URL và lưu metadata ban đầu vào bảng **Videos** trên Amazon DynamoDB.
4. Frontend tải video trực tiếp lên Amazon S3 Raw Upload Bucket.
5. Amazon S3 phát sinh sự kiện Object Created.
6. Amazon EventBridge nhận sự kiện từ S3.
7. Sự kiện được chuyển đến Amazon SQS.
8. EventBridge Pipes chuyển thông điệp sang AWS Step Functions.
9. AWS Step Functions khởi tạo AWS Elemental MediaConvert.
10. MediaConvert thực hiện chuyển mã video sang định dạng HLS.
11. Video đầu ra được lưu trong Amazon S3 Processed Media Bucket.
12. AWS Lambda cập nhật trạng thái xử lý vào bảng Videos.
13. Người dùng có thể phát video thông qua Amazon CloudFront.

Toàn bộ quy trình trên diễn ra hoàn toàn tự động mà không cần bất kỳ thao tác thủ công nào sau khi video được tải lên thành công.

---

## Nội dung của Workshop

Workshop được chia thành sáu phần chính nhằm đảm bảo quá trình triển khai diễn ra theo đúng trình tự.

- Chuẩn bị môi trường triển khai.
- Xây dựng Backend.
- Xây dựng Video Processing Pipeline.
- Kiểm thử toàn bộ hệ thống.
- Triển khai và sử dụng Web Application.
- Đánh giá kết quả triển khai.

Mỗi phần đều bao gồm mục tiêu, các bước thực hiện, hình ảnh minh họa, giải thích cấu hình và kết quả mong đợi. Điều này giúp người đọc không chỉ thực hiện thành công các thao tác mà còn hiểu được vai trò của từng dịch vụ trong kiến trúc tổng thể.

Ở các mục tiếp theo, Workshop sẽ hướng dẫn chi tiết cách cấu hình từng dịch vụ AWS, triển khai backend, xây dựng pipeline xử lý video và kiểm thử toàn bộ hệ thống để hoàn thiện nền tảng Serverless Video-on-Demand.

---

## Kết quả mong đợi

Sau khi hoàn thành toàn bộ Workshop, người đọc sẽ xây dựng được một hệ thống Video-on-Demand hoạt động theo kiến trúc serverless trên nền tảng AWS. Hệ thống không chỉ đáp ứng các chức năng cơ bản như tải video lên, xử lý video và phát trực tuyến mà còn thể hiện khả năng mở rộng, tính sẵn sàng cao và mô hình xử lý bất đồng bộ đặc trưng của các ứng dụng hiện đại.

Các kết quả mong đợi bao gồm:

- Backend hoạt động ổn định thông qua Amazon API Gateway và AWS Lambda.
- Metadata video được lưu trữ và quản lý trong bảng **Videos** của Amazon DynamoDB.
- Video được tải trực tiếp lên Amazon S3 Raw Upload Bucket bằng Presigned URL.
- Pipeline xử lý video tự động được kích hoạt khi có tệp mới được tải lên.
- AWS Elemental MediaConvert tạo thành công các tệp HLS phục vụ phát trực tuyến.
- Video sau xử lý được lưu trữ tại Amazon S3 Processed Media Bucket.
- Người dùng có thể xem danh sách video và phát video trên giao diện web thông qua Amazon CloudFront.
- Toàn bộ quá trình được giám sát bằng Amazon CloudWatch để phục vụ theo dõi và xử lý sự cố.

Những kết quả trên cũng chính là tiêu chí đánh giá việc triển khai thành công hệ thống trong phạm vi đồ án thực tập.

---

## Quy ước trong Workshop

Để đảm bảo tính nhất quán trong toàn bộ tài liệu, Workshop sử dụng một số quy ước chung khi trình bày.

- Tên dịch vụ AWS được giữ nguyên theo tên chính thức của Amazon Web Services.
- Tên các Bucket Amazon S3, bảng DynamoDB và hàm AWS Lambda được giữ nguyên như khi triển khai.
- Các hình minh họa đều được đánh số theo thứ tự xuất hiện trong tài liệu.
- Mọi thao tác cấu hình đều được thực hiện trên AWS Management Console.
- Các đoạn mã nguồn chỉ trình bày những phần liên quan trực tiếp đến chức năng đang được triển khai.

Ngoài ra, một số giao diện trên AWS Management Console có thể thay đổi theo từng thời điểm cập nhật của AWS. Tuy nhiên, nguyên tắc cấu hình và quy trình triển khai vẫn được giữ nguyên.

---

## Hình ảnh minh họa

Các hình ảnh trong Workshop được chụp trực tiếp trong quá trình triển khai hệ thống thực tế. Mỗi hình minh họa đều thể hiện một bước cấu hình hoặc một kết quả đạt được sau khi hoàn thành thao tác tương ứng.

Những hình ảnh này đóng vai trò quan trọng trong việc đối chiếu giữa tài liệu hướng dẫn và môi trường triển khai thực tế, giúp người đọc dễ dàng kiểm tra và xác nhận rằng các bước cấu hình đã được thực hiện chính xác.

Trong các phần tiếp theo, mỗi bước triển khai sẽ đi kèm ít nhất một hình minh họa tương ứng.

---

## Cấu trúc các phần tiếp theo

Sau phần giới thiệu tổng quan, Workshop sẽ tiếp tục với các nội dung sau:

### 5.2 Prerequisite

Giới thiệu các yêu cầu về tài khoản AWS, quyền truy cập, phần mềm và các tài nguyên cần chuẩn bị trước khi bắt đầu triển khai.

### 5.3 Build Backend

Hướng dẫn xây dựng backend của hệ thống, bao gồm việc tạo Amazon S3 Bucket, Amazon DynamoDB, AWS Lambda, Amazon API Gateway và kiểm tra khả năng tải video lên hệ thống.

### 5.4 Build Video Processing Pipeline

Triển khai pipeline xử lý video sử dụng Amazon EventBridge, Amazon SQS, EventBridge Pipes, AWS Step Functions và AWS Elemental MediaConvert nhằm tự động chuyển mã video sau khi tải lên.

### 5.5 System Testing

Kiểm thử toàn bộ quy trình hoạt động của hệ thống từ khi tải video lên cho đến khi video hoàn tất xử lý và có thể phát trực tuyến trên giao diện web.

### 5.6 Web Application

Triển khai giao diện người dùng, thực hiện đăng nhập, tải video, theo dõi trạng thái xử lý và phát video trực tiếp thông qua Amazon CloudFront.

### 5.7 Conclusion

Tổng kết kết quả triển khai, đánh giá các chức năng đã hoàn thành, những ưu điểm của kiến trúc serverless và định hướng phát triển trong tương lai.

---

## Tổng kết

Phần **Workshop Overview** đã giới thiệu tổng quan về mục tiêu, kiến trúc, các dịch vụ AWS sử dụng cũng như quy trình triển khai của hệ thống **Serverless Video-on-Demand Platform on AWS**.

Trong các phần tiếp theo, tài liệu sẽ trình bày chi tiết từng bước cấu hình và triển khai trên AWS Management Console, từ việc chuẩn bị hạ tầng, xây dựng backend, thiết lập pipeline xử lý video cho đến kiểm thử toàn bộ hệ thống. Mỗi bước đều được minh họa bằng hình ảnh thực tế và giải thích cụ thể nhằm giúp người đọc có thể tái hiện đầy đủ hệ thống theo đúng quy trình mà nhóm đã thực hiện.
