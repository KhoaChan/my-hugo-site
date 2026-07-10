---
title: "Blog 3"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---

# TỐI ƯU HÓA HIỆU NĂNG VÀ ĐIỀU PHỐI DỊCH VỤ TRONG KIẾN TRÚC SERVERLESS QUY MÔ LỚN

Khi mới bắt đầu áp dụng mô hình **Serverless**, hầu hết chúng ta đều bị thu hút bởi lời hứa quen thuộc của các nhà cung cấp dịch vụ đám mây: chỉ cần tập trung vào việc viết mã, triển khai ứng dụng và hệ thống sẽ tự động mở rộng theo nhu cầu.

Tuy nhiên, giữa lý thuyết và thực tế vận hành ở quy mô doanh nghiệp luôn tồn tại một khoảng cách rất lớn.

Một câu hỏi quan trọng được đặt ra là: điều gì sẽ xảy ra khi hệ thống không chỉ xử lý vài nghìn request, mà phải mở rộng đến hàng trăm nghìn hoặc thậm chí **1 triệu AWS Lambda function chạy đồng thời**?

Ở quy mô này, vấn đề không còn chỉ nằm ở việc code chạy nhanh hay chậm. Nút thắt thật sự thường đến từ **service quota**, giới hạn kết nối, cơ chế retry, cold start, chi phí chờ đợi và cách các dịch vụ phụ thuộc vào nhau.

Bài viết này tập trung phân tích cách tối ưu hiệu năng và điều phối dịch vụ trong kiến trúc serverless quy mô lớn, đặc biệt thông qua sự kết hợp giữa **AWS Lambda**, **Amazon SQS** và **AWS Step Functions**.

---

## 1. Vấn đề khi hệ thống Serverless mở rộng quá lớn

Ở quy mô nhỏ, việc để các service gọi trực tiếp lẫn nhau có thể hoạt động ổn. Ví dụ, một Lambda function gọi API, sau đó gọi tiếp một Lambda function khác để xử lý dữ liệu.

Tuy nhiên, khi traffic tăng mạnh, kiến trúc đồng bộ như vậy dễ gặp nhiều vấn đề:

* Lambda function phải chờ kết quả từ service phía sau.
* Thời gian chờ vẫn bị tính phí.
* Nếu service phía sau chậm hoặc lỗi, toàn bộ chuỗi xử lý bị ảnh hưởng.
* Khi request tăng đột biến, các function có thể bị throttling.
* Một lỗi nhỏ có thể tạo ra hiệu ứng domino trên toàn hệ thống.

Vì vậy, ở quy mô lớn, kiến trúc cần chuyển từ mô hình gọi đồng bộ trực tiếp sang **event-driven architecture** (kiến trúc hướng sự kiện) và **asynchronous processing** (xử lý bất đồng bộ).

---

## 2. Kiến trúc đề xuất: Lambda + SQS + Step Functions

Một kiến trúc serverless có khả năng mở rộng tốt thường không để các thành phần phụ thuộc chặt vào nhau. Thay vào đó, hệ thống sử dụng hàng đợi, event và state machine để điều phối luồng xử lý.

Ba dịch vụ đóng vai trò quan trọng gồm:

* **AWS Lambda:** xử lý logic nghiệp vụ theo từng tác vụ nhỏ.
* **Amazon SQS:** đóng vai trò hàng đợi để hấp thụ traffic tăng đột biến.
* **AWS Step Functions:** điều phối workflow, quản lý trạng thái, retry và xử lý lỗi.

Luồng xử lý có thể hình dung như sau:



Kiến trúc này giúp hệ thống tránh tình trạng một service bị quá tải làm ảnh hưởng đến toàn bộ luồng xử lý.

---

## 3. Vai trò của từng dịch vụ trong kiến trúc

### 3.1. Amazon SQS - Bộ giảm tải cho hệ thống

Amazon SQS hoạt động như một lớp đệm giữa nguồn phát sinh request và các Lambda function xử lý phía sau.

Khi traffic tăng đột biến, SQS sẽ giữ message trong hàng đợi thay vì đẩy toàn bộ request trực tiếp vào Lambda cùng lúc. Điều này giúp:

* Giảm nguy cơ Lambda bị throttling.
* Tránh làm quá tải database hoặc downstream service.
* Cho phép hệ thống xử lý request theo tốc độ ổn định hơn.
* Hạn chế mất dữ liệu khi có lỗi tạm thời.

Nói cách khác, SQS giúp hệ thống hấp thụ tải đỉnh và xử lý dần theo khả năng thực tế của backend.

### 3.2. AWS Lambda - Xử lý logic theo từng tác vụ nhỏ

AWS Lambda phù hợp với các tác vụ xử lý ngắn, độc lập và có thể scale nhanh theo số lượng event.

Tuy nhiên, ở quy mô lớn, cần cấu hình Lambda cẩn thận để tránh các vấn đề như:

* Cold start.
* Vượt giới hạn concurrency.
* Timeout.
* Package quá lớn.
* Retry không kiểm soát.
* Làm quá tải service phía sau.

Lambda nên được thiết kế theo hướng nhỏ gọn, rõ trách nhiệm và không nên giữ logic điều phối phức tạp bên trong code.

### 3.3. AWS Step Functions - Điều phối workflow có trạng thái

AWS Step Functions giúp điều phối nhiều bước xử lý khác nhau trong một workflow rõ ràng.

Thay vì để Lambda A gọi Lambda B rồi chờ kết quả, Step Functions có thể quản lý luồng xử lý theo từng state. Dịch vụ này hỗ trợ:

* Quản lý trạng thái workflow.
* Retry tự động.
* Catch lỗi.
* Timeout theo từng bước.
* Tách biệt logic nghiệp vụ và logic điều phối.
* Quan sát trực quan luồng chạy của hệ thống.

Cách làm này giúp giảm việc viết code xử lý lỗi thủ công và làm workflow dễ theo dõi hơn.

---

## 4. So sánh kiến trúc đồng bộ và kiến trúc hướng sự kiện

| Tiêu chí | Kiến trúc đồng bộ truyền thống | Kiến trúc hướng sự kiện |
| :--- | :--- | :--- |
| **Xử lý tải đỉnh** | Dễ bị ảnh hưởng khi traffic tăng đột ngột, có thể gây lỗi 503 hoặc throttling | SQS hấp thụ traffic và điều tiết luồng xử lý |
| **Tối ưu chi phí** | Chi phí cao hơn do Lambda phải chờ service phía sau phản hồi | Chỉ trả tiền cho thời gian xử lý thực tế |
| **Khả năng mở rộng** | Dễ gặp bottleneck khi nhiều service gọi trực tiếp nhau | Mở rộng linh hoạt hơn nhờ queue và event |
| **Xử lý lỗi** | Cần tự viết retry, timeout và error handling | Step Functions và DLQ hỗ trợ retry, catch và khôi phục |
| **Độ phụ thuộc giữa service** | Coupling cao, lỗi có thể lan truyền | Coupling thấp, lỗi được cô lập tốt hơn |
| **Khả năng quan sát** | Khó theo dõi toàn bộ luồng nếu không có tracing tốt | Workflow rõ ràng, dễ quan sát bằng Step Functions |

Sự kết hợp giữa Lambda, SQS và Step Functions giúp loại bỏ mô hình phản tác dụng, nơi một Lambda function phải ngồi chờ kết quả từ một Lambda function khác. Điều này giúp hệ thống tối ưu tốt hơn cả về hiệu năng lẫn chi phí.

---

## 5. Ba bài học kỹ thuật quan trọng cho Cloud Engineer

### 5.1. Làm chủ concurrency và tránh nghẽn tài nguyên

Ở quy mô rất lớn, giới hạn concurrency là một trong những vấn đề quan trọng nhất cần quan tâm.

Mặc định, mỗi AWS account có giới hạn số lượng Lambda concurrent executions theo từng Region. Nếu toàn bộ workload cùng tăng đột biến, các function quan trọng có thể bị thiếu tài nguyên.

**Giải pháp:**
Cần chủ động cấu hình Reserved Concurrency cho các Lambda function quan trọng. Điều này giúp đảm bảo các quy trình nghiệp vụ cốt lõi luôn có tài nguyên riêng để hoạt động.

Ví dụ:
* Function xử lý thanh toán nên có reserved concurrency riêng.
* Function gửi email hoặc xử lý log có thể dùng concurrency thấp hơn.
* Các workload nền không nên chiếm hết tài nguyên của workload quan trọng.

**Chiến lược retry:**
Khi gọi các service phía sau, cần áp dụng chiến lược:
* Exponential Backoff.
* Jitter.
* Timeout hợp lý.
* Circuit breaker nếu cần.

Điều này giúp tránh tình trạng hệ thống tự tạo ra một dạng self-inflicted DDoS, tức là tự retry quá nhiều và làm quá tải chính các service của mình.

### 5.2. Tối ưu deployment package và kiểm soát cold start

Cold start là một trong những yếu tố ảnh hưởng lớn đến độ trễ của Lambda, đặc biệt với các ứng dụng yêu cầu phản hồi nhanh.

Cold start thường bị ảnh hưởng bởi:
* Runtime sử dụng.
* Kích thước deployment package.
* Số lượng dependency.
* VPC configuration.
* Thời gian khởi tạo connection.
* Cấu hình memory.

**Cách tối ưu:**
Một số cách có thể áp dụng:
* Loại bỏ dependency không cần thiết.
* Dùng công cụ đóng gói như Webpack, esbuild hoặc ProGuard.
* Tối ưu code khởi tạo ngoài handler.
* Tái sử dụng database connection nếu phù hợp.
* Tăng memory để cải thiện CPU tương ứng.
* Dùng Lambda Layers cho thư viện dùng chung.
* Sử dụng Provisioned Concurrency cho endpoint yêu cầu latency thấp.

Với các hệ thống cần độ trễ ổn định, Provisioned Concurrency là một giải pháp quan trọng vì nó giúp giữ môi trường thực thi luôn sẵn sàng.

### 5.3. Thiết kế cơ chế an toàn và khôi phục lỗi

Ở quy mô hàng triệu request, không nên giả định rằng mọi request đều thành công. Ngay cả tỷ lệ lỗi rất nhỏ cũng có thể tạo ra số lượng lỗi lớn.

Ví dụ, nếu hệ thống xử lý 1.000.000 request và tỷ lệ lỗi chỉ là 0,01%, vẫn có khoảng 100 request thất bại. Vì vậy, hệ thống cần có cơ chế xử lý lỗi rõ ràng.

**Dead Letter Queue (DLQ):**
Dead Letter Queue giúp lưu lại các message không thể xử lý thành công sau nhiều lần retry. DLQ rất hữu ích để:
* Không làm mất dữ liệu.
* Phân tích nguyên nhân lỗi.
* Reprocess message sau khi sửa lỗi.
* Cô lập các message gây lỗi khỏi luồng chính.

**Lambda Destinations:**
Lambda Destinations cho phép định tuyến kết quả thực thi của Lambda đến các đích khác nhau, ví dụ:
* Gửi kết quả thành công đến EventBridge.
* Gửi lỗi đến SQS.
* Gửi thông báo đến SNS.
* Ghi log hoặc trigger workflow khác.

Cách này giúp giảm việc viết quá nhiều logic xử lý lỗi trong application code.

---

## 6. Ứng dụng thực tiễn: Infrastructure as Code với AWS SAM

Để triển khai kiến trúc serverless quy mô lớn, không nên cấu hình thủ công qua AWS Console. Thay vào đó, hạ tầng nên được định nghĩa bằng mã thông qua các công cụ như:

* AWS SAM.
* AWS CDK.
* AWS CloudFormation.
* Terraform.

Dưới đây là ví dụ AWS SAM template triển khai một Lambda function có cấu hình Reserved Concurrency và Dead Letter Queue bằng Amazon SQS.

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Transform: AWS::Serverless-2016-10-31
Description: AWS SAM Template for Massive Scale Serverless Architecture

Resources:
  MyServerlessDeadLetterQueue:
    Type: AWS::SQS::Queue
    Properties:
      QueueName: !Sub "${AWS::StackName}-dlq"
      MessageRetentionPeriod: 1209600 # 14 days of retention for safe debugging

  MyMassiveScaleFunction:
    Type: AWS::Serverless::Function
    Properties:
      FunctionName: !Sub "${AWS::StackName}-core-processor"
      CodeUri: src/
      Handler: app.lambda_handler
      Runtime: python3.11
      MemorySize: 2048
      Timeout: 30
      ReservedConcurrentExecutions: 500
      DeadLetterQueue:
        Type: SQS
        TargetArn: !GetAtt MyServerlessDeadLetterQueue.Arn
      Events:
        SQSTrigger:
          Type: SQS
          Properties:
            Queue: !GetAtt MyServerlessDeadLetterQueue.Arn
            BatchSize: 10


 Giải thích nhanh template:
Trong template trên:

MyServerlessDeadLetterQueue tạo một SQS queue dùng làm DLQ.

MessageRetentionPeriod giữ message lỗi trong 14 ngày để phục vụ debug.

MyMassiveScaleFunction là Lambda function xử lý chính.

ReservedConcurrentExecutions: 500 giúp giới hạn và cô lập tài nguyên concurrency cho function.

DeadLetterQueue giúp lưu lại message lỗi sau khi xử lý thất bại.

SQSTrigger cấu hình Lambda được kích hoạt bởi SQS.

7. Ứng dụng thực tiễn và thảo luận cộng đồng
Kiến trúc serverless có khả năng mở rộng rất mạnh, nhưng sức mạnh đó chỉ thực sự được khai thác khi kỹ sư hiểu rõ giới hạn và cơ chế vận hành của từng dịch vụ.

Một hệ thống serverless tốt cần đảm bảo:

Có queue để hấp thụ tải đỉnh.

Có workflow để điều phối trạng thái.

Có retry và DLQ để xử lý lỗi.

Có observability để theo dõi hệ thống.

Có giới hạn concurrency để bảo vệ tài nguyên.

Có IaC để triển khai nhất quán và dễ lặp lại.

Bài học quan trọng là serverless không có nghĩa là không cần thiết kế hạ tầng. Ngược lại, khi hệ thống càng lớn, việc thiết kế luồng dữ liệu, giới hạn tài nguyên và cơ chế xử lý lỗi càng trở nên quan trọng.

8. Bài học rút ra
Sau khi tìm hiểu chủ đề này, tôi rút ra một số bài học quan trọng:

Serverless giúp giảm gánh nặng quản lý server, nhưng không loại bỏ trách nhiệm thiết kế kiến trúc.

Ở quy mô lớn, service quota và concurrency limit là yếu tố bắt buộc phải kiểm soát.

Không nên để các Lambda function gọi đồng bộ lẫn nhau nếu workflow có nhiều bước xử lý.

Amazon SQS giúp hệ thống hấp thụ tải đột biến và xử lý bất đồng bộ ổn định hơn.

AWS Step Functions giúp điều phối workflow rõ ràng, dễ quan sát và dễ xử lý lỗi.

Cold start cần được tối ưu nếu hệ thống yêu cầu latency thấp.

DLQ và Lambda Destinations là những thành phần quan trọng để tránh mất dữ liệu khi lỗi xảy ra.

Infrastructure as Code là hướng tiếp cận cần thiết khi triển khai hạ tầng cloud nghiêm túc.

9. Kết luận
Kiến trúc serverless mang lại rất nhiều lợi ích về khả năng mở rộng, chi phí và tốc độ triển khai. Tuy nhiên, để vận hành ổn định ở quy mô lớn, kỹ sư cloud cần hiểu rõ cách các dịch vụ phối hợp với nhau.

Sự kết hợp giữa AWS Lambda, Amazon SQS và AWS Step Functions là một mô hình rất phù hợp để xử lý các workload bất đồng bộ, có lưu lượng lớn và cần khả năng khôi phục lỗi tốt.

Thông qua chủ đề này, tôi nhận ra rằng một hệ thống cloud tốt không chỉ là hệ thống có thể chạy được, mà còn phải có khả năng mở rộng có kiểm soát, chịu lỗi tốt, dễ quan sát, tối ưu chi phí và giảm thiểu tối đa thao tác thủ công.

Tài liệu tham khảo
Bài đăng gốc trên AWS Architecture Blog: Lessons learned from scaling to 1 million AWS Lambda functions

Bài đăng thảo luận cộng đồng trên nhóm AWS FCJ về kiến trúc serverless quy mô lớn