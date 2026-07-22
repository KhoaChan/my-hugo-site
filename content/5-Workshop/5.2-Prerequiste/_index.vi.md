---
title: "Điều kiện tiên quyết"
date: 2026-07-10
weight: 2
chapter: false
pre: "<b>5.2. </b>"
---


## Giới thiệu

Trước khi bắt đầu triển khai **Serverless Video-on-Demand Platform on AWS**, cần chuẩn bị đầy đủ môi trường triển khai, tài khoản AWS và các công cụ phát triển cần thiết. Việc chuẩn bị trước các điều kiện này giúp quá trình cấu hình diễn ra liên tục, hạn chế lỗi phát sinh và đảm bảo tất cả các dịch vụ AWS có thể hoạt động đồng bộ với nhau.

Trong phạm vi Workshop này, toàn bộ tài nguyên được triển khai trực tiếp trên Amazon Web Services thông qua AWS Management Console. Một số bước sẽ yêu cầu người thực hiện có quyền tạo tài nguyên mới, cấu hình quyền truy cập và theo dõi trạng thái hoạt động của hệ thống.

---

## Tài khoản AWS

Để triển khai hệ thống, người thực hiện cần có một tài khoản AWS đang hoạt động và có quyền tạo các dịch vụ cần thiết.

Các dịch vụ sẽ được sử dụng trong Workshop bao gồm:

- Amazon S3
- Amazon CloudFront
- Amazon Cognito
- Amazon API Gateway
- AWS Lambda
- Amazon DynamoDB
- Amazon EventBridge
- Amazon SQS
- EventBridge Pipes
- AWS Step Functions
- AWS Elemental MediaConvert
- Amazon CloudWatch
- AWS Identity and Access Management (IAM)

Nhằm thuận tiện cho việc quản lý tài nguyên và theo dõi chi phí, nên sử dụng một tài khoản AWS riêng dành cho quá trình học tập hoặc nghiên cứu thay vì tài khoản đang phục vụ các hệ thống khác.

---

## Quyền truy cập

Người triển khai cần sử dụng IAM User hoặc IAM Role có đầy đủ quyền tạo và cấu hình các dịch vụ AWS.

Các quyền tối thiểu bao gồm:

- Amazon S3 Full Access
- Amazon DynamoDB Full Access
- AWS Lambda Full Access
- Amazon API Gateway Administrator
- AWS Step Functions Full Access
- Amazon EventBridge Full Access
- Amazon SQS Full Access
- AWS Elemental MediaConvert Full Access
- Amazon CloudWatch Full Access
- IAM Full Access

Nếu thiếu một trong các quyền trên, quá trình triển khai có thể gặp lỗi khi tạo tài nguyên hoặc kết nối giữa các dịch vụ.

---

## Môi trường phát triển

Workshop được thực hiện chủ yếu thông qua AWS Management Console. Ngoài ra, cần chuẩn bị một số phần mềm để chỉnh sửa mã nguồn và triển khai giao diện web.

| Phần mềm | Mục đích |
|----------|----------|
| Google Chrome | Truy cập AWS Management Console |
| Visual Studio Code | Chỉnh sửa mã nguồn |
| Node.js LTS | Chạy và build ứng dụng web |
| Git | Quản lý mã nguồn |
| AWS CLI (Tùy chọn) | Quản lý AWS bằng dòng lệnh |

Mặc dù AWS CLI không bắt buộc, việc cài đặt công cụ này sẽ thuận tiện hơn trong quá trình kiểm tra và quản lý tài nguyên.

---

## Kết nối mạng

Do toàn bộ hệ thống được triển khai trên nền tảng đám mây, máy tính sử dụng cần có kết nối Internet ổn định trong suốt quá trình thực hiện Workshop.

Ngoài ra, cần đảm bảo rằng trình duyệt hoặc tường lửa không chặn việc truy cập vào các dịch vụ của AWS. Trong môi trường doanh nghiệp, có thể cần cấu hình cho phép kết nối HTTPS đến các miền dịch vụ của Amazon Web Services.

---

## Thời gian triển khai dự kiến

Tổng thời gian triển khai toàn bộ hệ thống phụ thuộc vào tốc độ mạng và mức độ quen thuộc với AWS. Trong điều kiện bình thường, Workshop có thể hoàn thành trong khoảng hai đến ba giờ.

Thời gian tham khảo cho từng phần như sau:

| Nội dung | Thời gian |
|----------|-----------:|
| Xây dựng Backend | 45–60 phút |
| Xây dựng Video Processing Pipeline | 45–60 phút |
| Kiểm thử hệ thống | 20–30 phút |
| Triển khai Web Application | 20–30 phút |

---

## Khu vực triển khai AWS

Trong quá trình triển khai, tất cả các tài nguyên nên được tạo trong cùng một **AWS Region** để đảm bảo khả năng tương thích giữa các dịch vụ và giảm độ trễ trong quá trình trao đổi dữ liệu.

Đối với đồ án này, toàn bộ tài nguyên được triển khai trong cùng một Region nhằm đảm bảo Amazon S3, AWS Lambda, Amazon DynamoDB, Amazon EventBridge, Amazon SQS, AWS Step Functions và AWS Elemental MediaConvert có thể kết nối và hoạt động ổn định với nhau.

Trước khi tạo bất kỳ tài nguyên nào, người thực hiện nên kiểm tra Region đang được chọn trên AWS Management Console. Việc thay đổi Region sau khi đã triển khai hệ thống sẽ yêu cầu tạo lại nhiều tài nguyên và cấu hình liên quan.

---

## Quy ước đặt tên tài nguyên

Để thuận tiện cho việc quản lý và bảo trì hệ thống, các tài nguyên AWS được đặt tên theo quy ước thống nhất trong suốt quá trình triển khai.

Một số tài nguyên chính bao gồm:

| Tài nguyên | Tên sử dụng |
|------------|-------------|
| DynamoDB Table | Videos |
| S3 Bucket chứa video gốc | Raw Upload Bucket |
| S3 Bucket chứa video sau xử lý | Processed Media Bucket |
| Lambda Function | Upload Lambda |
| REST API | Video API |
| Step Functions | Video Processing Workflow |

Việc sử dụng tên tài nguyên rõ ràng giúp quá trình theo dõi log, cấu hình IAM và kiểm tra hệ thống trở nên dễ dàng hơn.

---

## Chi phí triển khai

Hệ thống được xây dựng dưới dạng nguyên mẫu (Prototype) nên chi phí triển khai tương đối thấp. Phần lớn các dịch vụ AWS được sử dụng đều áp dụng mô hình **Pay-as-you-go**, tức là người dùng chỉ thanh toán theo mức tài nguyên thực tế đã sử dụng.

Các dịch vụ có thể phát sinh chi phí bao gồm:

- Amazon S3 lưu trữ dữ liệu.
- AWS Elemental MediaConvert xử lý video.
- Amazon CloudFront phân phối nội dung.
- AWS Lambda thực thi hàm.
- Amazon API Gateway xử lý yêu cầu từ người dùng.

Trong quá trình thử nghiệm, số lượng video tải lên không nhiều nên tổng chi phí vận hành ở mức thấp. Tuy nhiên, người triển khai vẫn nên thường xuyên theo dõi mục **Billing & Cost Management** trên AWS để kiểm soát chi phí và xóa các tài nguyên không còn sử dụng sau khi hoàn thành Workshop.

---

## Chuẩn bị mã nguồn

Trước khi bắt đầu cấu hình các dịch vụ AWS, toàn bộ mã nguồn của dự án cần được chuẩn bị trên máy tính.

Dự án bao gồm hai thành phần chính:

- Backend được triển khai bằng các hàm AWS Lambda.
- Frontend là ứng dụng web phục vụ việc đăng nhập, tải video lên hệ thống và phát video sau khi xử lý.

Mã nguồn Backend chịu trách nhiệm tạo Presigned URL, quản lý metadata video và xử lý các nghiệp vụ của hệ thống. Trong khi đó, Frontend cung cấp giao diện giúp người dùng tương tác với nền tảng Video-on-Demand thông qua trình duyệt web.

Việc chuẩn bị đầy đủ mã nguồn trước khi triển khai sẽ giúp quá trình cấu hình và kiểm thử diễn ra thuận lợi hơn.

---

## Danh sách kiểm tra trước khi triển khai

Trước khi chuyển sang bước xây dựng Backend, cần xác nhận rằng toàn bộ điều kiện tiên quyết đã được chuẩn bị đầy đủ.

Danh sách kiểm tra bao gồm:

- Đã có tài khoản AWS đang hoạt động.
- IAM User hoặc IAM Role có đầy đủ quyền truy cập.
- Đã lựa chọn đúng AWS Region.
- Đã cài đặt các phần mềm cần thiết.
- Máy tính có kết nối Internet ổn định.
- Mã nguồn dự án đã được chuẩn bị.
- Có thể đăng nhập và truy cập AWS Management Console.

Việc hoàn thành đầy đủ các bước trên sẽ giúp giảm thiểu lỗi phát sinh trong quá trình cấu hình các dịch vụ AWS ở các phần tiếp theo.

---

## Tổng kết

Trong phần này, các điều kiện cần thiết trước khi triển khai hệ thống **Serverless Video-on-Demand Platform on AWS** đã được giới thiệu. Các nội dung bao gồm tài khoản AWS, quyền truy cập, môi trường phát triển, lựa chọn Region, quy ước đặt tên tài nguyên, chi phí triển khai và các yêu cầu về mã nguồn.

Sau khi hoàn thành các bước chuẩn bị, hệ thống đã sẵn sàng để triển khai hạ tầng Backend. Phần tiếp theo của Workshop sẽ hướng dẫn chi tiết cách tạo Amazon S3 Bucket, cấu hình Amazon DynamoDB, triển khai AWS Lambda và tích hợp Amazon API Gateway để xây dựng nền tảng backend cho hệ thống.
