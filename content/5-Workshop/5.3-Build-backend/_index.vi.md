---
title: "Xây dựng Backend"
date: 2026-07-10
weight: 3
chapter: false
pre: "<b>5.3. </b>"
---


## Giới thiệu

Backend đóng vai trò là thành phần trung tâm của hệ thống **Serverless Video-on-Demand Platform on AWS**. Thành phần này chịu trách nhiệm tiếp nhận yêu cầu từ giao diện người dùng, tạo đường dẫn tải video an toàn, lưu trữ metadata và cung cấp các API phục vụ cho toàn bộ ứng dụng.

Khác với mô hình truyền thống sử dụng máy chủ ứng dụng, Backend trong đồ án được xây dựng hoàn toàn theo kiến trúc **serverless**. Các nghiệp vụ được xử lý thông qua AWS Lambda, trong khi dữ liệu được lưu trữ trên Amazon DynamoDB và các tệp video được lưu trên Amazon S3. Nhờ đó, hệ thống có khả năng tự động mở rộng theo số lượng yêu cầu mà không cần quản lý hạ tầng máy chủ.

Trong phần này, Backend sẽ được triển khai lần lượt thông qua các dịch vụ của AWS, từ tạo Bucket lưu trữ, xây dựng cơ sở dữ liệu, triển khai Lambda đến cấu hình Amazon API Gateway. Sau khi hoàn thành, Backend sẽ có khả năng sinh Presigned URL và tiếp nhận video tải lên từ người dùng.

---

## Mục tiêu

Sau khi hoàn thành phần này, hệ thống sẽ đáp ứng các chức năng sau:

- Tạo Amazon S3 Bucket để lưu trữ video gốc.
- Tạo Amazon S3 Bucket để lưu trữ video sau xử lý.
- Xây dựng bảng **Videos** trên Amazon DynamoDB.
- Tạo IAM Role cho AWS Lambda.
- Triển khai Lambda Backend.
- Cấu hình Environment Variables.
- Cấp quyền truy cập cho Lambda.
- Cập nhật mã nguồn Lambda.
- Tích hợp Lambda với Amazon API Gateway.
- Triển khai REST API.
- Sinh Presigned URL.
- Tải video lên Amazon S3 thành công.
- Kiểm tra video đã được lưu trong Raw Upload Bucket.

---

## Quy trình triển khai

Quá trình xây dựng Backend được thực hiện theo trình tự sau:

1. Tạo Amazon S3 Buckets.
2. Tạo bảng Amazon DynamoDB.
3. Kiểm tra bảng DynamoDB.
4. Tạo IAM Role cho AWS Lambda.
5. Tạo Lambda Function.
6. Cấu hình Environment Variables.
7. Cấp IAM Policy.
8. Triển khai mã nguồn Lambda.
9. Kiểm tra dữ liệu metadata.
10. Tích hợp với Amazon API Gateway.
11. Deploy API.
12. Kiểm thử API.
13. Sinh Presigned URL.
14. Tải video lên Amazon S3.
15. Kiểm tra dữ liệu trong Raw Upload Bucket.

Mỗi bước đều được minh họa bằng hình ảnh thực tế và sẽ được thực hiện theo đúng thứ tự nhằm đảm bảo các dịch vụ có thể kết nối và hoạt động chính xác.

---

## Kết quả mong đợi

Sau khi hoàn thành toàn bộ phần này, Backend của hệ thống sẽ được triển khai đầy đủ trên AWS và sẵn sàng phục vụ các chức năng của nền tảng Video-on-Demand.

Người dùng có thể gửi yêu cầu thông qua API để nhận Presigned URL, tải video trực tiếp lên Amazon S3 và tạo metadata ban đầu trong bảng **Videos**. Đây cũng là tiền đề để xây dựng quy trình xử lý video tự động ở phần **5.4 Build Video Processing Pipeline**.

---

# Tạo Amazon S3 Buckets

## Mục đích

Amazon S3 được sử dụng làm dịch vụ lưu trữ của hệ thống. Trong đồ án này, hai Bucket được tạo nhằm tách biệt dữ liệu đầu vào và dữ liệu đầu ra của quá trình xử lý video.

- **Raw Upload Bucket** dùng để lưu trữ video gốc do người dùng tải lên.
- **Processed Media Bucket** dùng để lưu trữ các tệp HLS sau khi được AWS Elemental MediaConvert xử lý.

Việc tách riêng hai Bucket giúp quá trình quản lý dữ liệu rõ ràng hơn, đồng thời thuận tiện cho việc cấp quyền truy cập và triển khai quy trình xử lý video ở các bước tiếp theo.

## Thực hiện

Truy cập dịch vụ **Amazon S3** trên AWS Management Console và lần lượt tạo hai Bucket.

Trong quá trình tạo Bucket, lựa chọn cùng một **AWS Region** với các dịch vụ còn lại của hệ thống để đảm bảo khả năng kết nối và giảm độ trễ khi truyền dữ liệu.

Sau khi hoàn tất, kiểm tra danh sách Bucket để xác nhận cả hai Bucket đã được tạo thành công.

![Create S3 Buckets](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/01-create-s3-buckets.png)

*Hình 5.1. Tạo hai Amazon S3 Bucket phục vụ lưu trữ video.*

## Kết quả

Hai Amazon S3 Bucket đã được tạo thành công và sẵn sàng sử dụng. Đây sẽ là nơi lưu trữ video gốc của người dùng và các video sau khi hoàn tất quá trình chuyển mã.

---

# Tạo bảng Amazon DynamoDB

## Mục đích

Amazon DynamoDB được sử dụng để lưu trữ metadata của video. Thay vì lưu thông tin trực tiếp trong mã nguồn, mọi thông tin về video sẽ được quản lý tập trung trong bảng **Videos**.

Mỗi bản ghi sẽ lưu các thông tin như mã video, tên tệp, trạng thái xử lý, thời gian tải lên và đường dẫn đến video sau khi xử lý.

## Thực hiện

Truy cập dịch vụ **Amazon DynamoDB**, chọn **Create table** và cấu hình bảng với các thông tin sau:

- **Table name:** Videos
- **Partition key:** videoId
- **Data type:** String

Các thông số còn lại giữ nguyên theo mặc định của AWS.

Sau khi tạo bảng, chờ trạng thái chuyển sang **Active** trước khi tiếp tục.

![Create DynamoDB Table](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/02-create-dynamodb-table.png)

*Hình 5.2. Tạo bảng Videos trên Amazon DynamoDB.*

## Kết quả

Bảng **Videos** đã được tạo thành công và sẵn sàng lưu trữ metadata của video trong quá trình hệ thống hoạt động.

---

# Kiểm tra bảng DynamoDB

## Mục đích

Sau khi tạo bảng, cần kiểm tra lại cấu hình để đảm bảo bảng đã được khởi tạo đúng và có thể sử dụng trong các bước tiếp theo.

## Thực hiện

Mở bảng **Videos** trong Amazon DynamoDB và kiểm tra:

- Trạng thái bảng là **Active**.
- Partition Key là **videoId**.
- Chưa có dữ liệu nào được lưu.

Do Backend chưa được triển khai nên bảng vẫn đang ở trạng thái rỗng.

![Verify DynamoDB Table](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/03-dynamodb-item.png)

*Hình 5.3. Kiểm tra bảng Videos sau khi tạo.*

## Kết quả

Việc kiểm tra xác nhận bảng DynamoDB đã được cấu hình chính xác và sẵn sàng nhận dữ liệu metadata từ AWS Lambda ở các bước tiếp theo.

---

# Tạo IAM Role cho AWS Lambda

## Mục đích

Để AWS Lambda có thể truy cập và thao tác với các dịch vụ AWS khác, cần tạo một **IAM Role** làm Execution Role cho Lambda. IAM Role sẽ cấp các quyền cần thiết để Lambda ghi log lên Amazon CloudWatch, đọc và ghi dữ liệu trên Amazon S3, đồng thời thao tác với bảng **Videos** trên Amazon DynamoDB.

Việc sử dụng IAM Role thay cho Access Key giúp tăng cường tính bảo mật và tuân thủ mô hình phân quyền của AWS.

## Thực hiện

Truy cập dịch vụ **AWS Identity and Access Management (IAM)** và chọn **Roles**.

Tạo một Role mới với **Trusted Entity** là **AWS Service**, sau đó chọn **Lambda** làm dịch vụ sử dụng Role này.

Sau khi hoàn tất, đặt tên Role phù hợp để dễ dàng quản lý và sử dụng trong quá trình triển khai Backend.

![Create Lambda Role](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/04-create-lambda-role.png)

*Hình 5.4. Tạo IAM Role cho AWS Lambda.*

## Kết quả

IAM Role đã được tạo thành công và sẵn sàng sử dụng cho AWS Lambda trong các bước cấu hình tiếp theo.

---

# Tạo AWS Lambda

## Mục đích

AWS Lambda là thành phần xử lý chính của Backend. Hàm Lambda tiếp nhận yêu cầu từ Amazon API Gateway, tạo Presigned URL để tải video lên Amazon S3, đồng thời khởi tạo metadata ban đầu trong bảng **Videos**.

Việc sử dụng Lambda giúp hệ thống chỉ sử dụng tài nguyên khi có yêu cầu xử lý, góp phần tối ưu chi phí và khả năng mở rộng của nền tảng.

## Thực hiện

Mở dịch vụ **AWS Lambda**, chọn **Create function** và sử dụng tùy chọn **Author from scratch**.

Khai báo tên hàm, lựa chọn Runtime phù hợp và gán IAM Role vừa tạo làm Execution Role.

Sau khi hoàn tất cấu hình, chọn **Create Function** và chờ Lambda được khởi tạo.

![Create Lambda Function](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/05-create-lambda.png)

*Hình 5.5. Tạo AWS Lambda cho Backend.*

## Kết quả

AWS Lambda đã được tạo thành công và sẵn sàng cấu hình Environment Variables cũng như triển khai mã nguồn.

---

# Cấu hình Environment Variables

## Mục đích

Environment Variables được sử dụng để lưu các thông tin cấu hình mà Lambda cần sử dụng trong quá trình thực thi, giúp mã nguồn không phải khai báo cứng tên tài nguyên hoặc các tham số của hệ thống.

Việc tách các giá trị cấu hình khỏi mã nguồn cũng giúp quá trình bảo trì và mở rộng hệ thống trở nên thuận tiện hơn.

## Thực hiện

Truy cập Lambda vừa tạo, chọn **Configuration** → **Environment variables**.

Khai báo các biến môi trường theo yêu cầu của hệ thống, bao gồm tên bảng DynamoDB, tên các Amazon S3 Bucket và các tham số cấu hình khác phục vụ Backend.

Sau khi hoàn tất, lưu lại cấu hình để Lambda có thể sử dụng các giá trị này trong quá trình thực thi.

![Lambda Environment Variables](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/06-lambda-environment.png)

*Hình 5.6. Cấu hình Environment Variables cho AWS Lambda.*

## Kết quả

AWS Lambda đã được cấu hình đầy đủ các biến môi trường và sẵn sàng cho việc triển khai mã nguồn ở bước tiếp theo.

---

# Cấp IAM Policy cho AWS Lambda

## Mục đích

Sau khi tạo IAM Role, cần cấp các quyền truy cập phù hợp để AWS Lambda có thể thực hiện các nghiệp vụ của hệ thống. Các quyền này cho phép Lambda đọc và ghi dữ liệu trên Amazon S3, thao tác với bảng **Videos** trên Amazon DynamoDB và ghi log lên Amazon CloudWatch.

Việc cấu hình đúng IAM Policy giúp Lambda hoạt động ổn định, đồng thời đảm bảo chỉ được phép truy cập những tài nguyên cần thiết.

## Thực hiện

Truy cập IAM Role đã tạo ở bước trước và chọn **Add permissions** để gắn các Policy cần thiết.

Trong quá trình triển khai, Role được cấp quyền truy cập đến Amazon S3, Amazon DynamoDB và Amazon CloudWatch. Sau khi hoàn tất, kiểm tra lại danh sách Policy đã được gắn vào Role.

![Attach IAM Policy](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/07-lambda-iam-policy.png)

*Hình 5.7. Gán IAM Policy cho AWS Lambda.*

## Kết quả

IAM Role đã được cấp đầy đủ quyền truy cập và AWS Lambda có thể tương tác với các dịch vụ AWS cần thiết trong hệ thống.

---

# Triển khai mã nguồn Lambda

## Mục đích

Sau khi hoàn thành các bước cấu hình, mã nguồn Backend sẽ được triển khai lên AWS Lambda để xử lý các yêu cầu từ Amazon API Gateway.

Mã nguồn thực hiện các chức năng chính như tạo Presigned URL, lưu metadata video vào DynamoDB và trả kết quả về cho Frontend.

## Thực hiện

Mở AWS Lambda và chuyển sang thẻ **Code**.

Tải mã nguồn Backend lên Lambda hoặc cập nhật trực tiếp trong trình soạn thảo của AWS Management Console. Sau khi cập nhật, chọn **Deploy** để áp dụng phiên bản mới của hàm Lambda.

Khi quá trình triển khai hoàn tất, kiểm tra để đảm bảo không xuất hiện lỗi trong quá trình Deploy.

![Deploy Lambda Source Code](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/08-lambda-source-code.png)

*Hình 5.8. Triển khai mã nguồn cho AWS Lambda.*

## Kết quả

Mã nguồn Backend đã được triển khai thành công và Lambda đã sẵn sàng tiếp nhận yêu cầu từ Amazon API Gateway.

---

# Kiểm tra dữ liệu Metadata

## Mục đích

Sau khi Lambda được triển khai, cần kiểm tra khả năng ghi dữ liệu vào bảng **Videos** nhằm xác nhận Backend có thể tạo metadata cho video một cách chính xác.

Đây là bước kiểm tra quan trọng trước khi cấu hình REST API.

## Thực hiện

Thực hiện một yêu cầu thử nghiệm đến Lambda để tạo metadata.

Sau khi Lambda thực thi thành công, mở bảng **Videos** trong Amazon DynamoDB và kiểm tra dữ liệu vừa được tạo.

Nếu xuất hiện một Item mới với các thông tin của video, chứng tỏ Lambda đã kết nối thành công với DynamoDB.

![Metadata Created](/my-hugo-site/images/5-Workshop/5.4-S3-onprem/10-lambda-created-dynamodb-item.png)

*Hình 5.9. Metadata được tạo thành công trong bảng Videos.*

## Kết quả

Dữ liệu metadata đã được ghi thành công vào Amazon DynamoDB. Điều này xác nhận Backend có thể lưu trữ thông tin video và sẵn sàng tích hợp với Amazon API Gateway ở bước tiếp theo.
