---
title: "Worklog Tuần 7"
date: 2026-05-29
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---


## I. Tóm tắt tổng quan

Tuần 7 đánh dấu một giai đoạn chuyển tiếp quan trọng trong quá trình thực tập của tôi. Sau khi hoàn thành các hoạt động học tập và thực hành liên quan đến những dịch vụ AWS nền tảng ở các tuần trước, tôi bắt đầu tập trung vào việc tổng hợp kiến thức, phân tích yêu cầu và xây dựng định hướng ban đầu cho đề tài thực tập.

Thay vì tiếp tục thực hiện các bài lab riêng lẻ, trong tuần này tôi dành thời gian hệ thống hóa lại kiến thức AWS đã học để lựa chọn giải pháp phù hợp cho đề tài **Mini Video-on-Demand Platform using AWS Serverless**.

Trong tuần này, tôi phân tích yêu cầu nghiệp vụ của hệ thống, nghiên cứu các mô hình kiến trúc có thể áp dụng trên AWS và đánh giá mức độ phù hợp của từng dịch vụ AWS đối với các thành phần khác nhau của hệ thống. Dựa trên kết quả nghiên cứu, tôi lựa chọn mô hình **Serverless Architecture** kết hợp với **Event-Driven Architecture** nhằm tận dụng khả năng mở rộng linh hoạt, giảm chi phí vận hành và hạn chế việc quản lý hạ tầng thủ công.

Đồng thời, tôi cũng xây dựng phiên bản đầu tiên của kiến trúc tổng thể và xác định vai trò của các dịch vụ như **Amazon S3**, **Amazon API Gateway**, **AWS Lambda**, **Amazon Cognito**, **Amazon DynamoDB**, **Amazon CloudFront**, **Amazon SQS** và **AWS Elemental MediaConvert** trong từng giai đoạn hoạt động của hệ thống.

Kết thúc tuần này, tôi đã hoàn thành bản thiết kế kiến trúc ban đầu, xác định lộ trình cho các giai đoạn tiếp theo và chuẩn bị các tài liệu cần thiết để hỗ trợ việc hoàn thiện báo cáo thực tập.

---

## II. Mục tiêu chiến lược trong tuần

Sau khi hoàn thành các hoạt động học tập nền tảng, mục tiêu chính của tuần 7 là chuyển từ việc học từng dịch vụ AWS riêng lẻ sang nghiên cứu và thiết kế giải pháp cho đề tài thực tập.

Các mục tiêu chính trong tuần bao gồm:

- Tổng hợp và hệ thống hóa toàn bộ kiến thức AWS đã học ở các tuần trước để phục vụ việc thiết kế hệ thống.
- Phân tích yêu cầu nghiệp vụ của nền tảng Video-on-Demand và xác định các chức năng cốt lõi cần triển khai.
- Nghiên cứu các mô hình kiến trúc trên AWS, đánh giá ưu và nhược điểm của từng hướng tiếp cận trước khi lựa chọn mô hình Serverless.
- Xác định các dịch vụ AWS phù hợp cho từng thành phần của hệ thống như xác thực người dùng, xử lý nghiệp vụ, lưu trữ dữ liệu, xử lý video và phân phối nội dung.
- Xây dựng phiên bản đầu tiên của thiết kế kiến trúc tổng thể (**Architecture Draft V1**) làm nền tảng cho các tuần tiếp theo.
- Lập kế hoạch phát triển nội dung theo từng giai đoạn nhằm đảm bảo tiến độ và tính thống nhất của báo cáo thực tập.

Thông qua các mục tiêu trên, tôi hướng đến việc hình thành một kiến trúc ban đầu có tính khả thi, đồng thời tạo nền tảng vững chắc để tiếp tục hoàn thiện nội dung kỹ thuật của báo cáo.

---

## III. Nhật ký hoạt động & Lộ trình phân bổ chi tiết (Từ 02/06/2026 đến 08/06/2026)

| Thời gian | Danh mục hoạt động | Chi tiết các tác vụ thực hiện | Kết quả/Minh chứng đạt được |
| --- | --- | --- | --- |
| Ngày 1 (02/06) | Tổng hợp kiến thức AWS | Rà soát và hệ thống hóa kiến thức đã học về Compute, Storage, Database, Networking, Security và Serverless trong chương trình thực tập. | Hoàn thành ghi chú tổng hợp về các dịch vụ AWS và định hướng lựa chọn công nghệ cho hệ thống. |
| Ngày 2 (03/06) | Phân tích yêu cầu nghiệp vụ | Phân tích yêu cầu của hệ thống Video-on-Demand và xác định các chức năng chính như quản lý người dùng, upload video, xử lý video và phát video trực tuyến. | Xây dựng mô tả phạm vi chức năng và luồng nghiệp vụ của hệ thống. |
| Ngày 3 (04/06) | Nghiên cứu giải pháp triển khai | Tìm hiểu các mô hình kiến trúc trên AWS và đánh giá khả năng áp dụng Serverless Architecture và Event-Driven Architecture cho bài toán xử lý video. | Lựa chọn Serverless Architecture làm hướng triển khai chính cho đề tài. |
| Ngày 4 (05/06) | Thiết kế kiến trúc tổng thể | Xác định các thành phần chính của hệ thống và mối quan hệ giữa Frontend, Backend, Data Layer và Video Processing Pipeline. | Hoàn thành phiên bản đầu tiên của kiến trúc tổng thể hệ thống. |
| Ngày 5 (06/06) | Lựa chọn dịch vụ AWS | Đánh giá và lựa chọn các dịch vụ AWS như Amazon S3, API Gateway, Lambda, Cognito, DynamoDB, CloudFront, SQS và MediaConvert cho từng thành phần. | Hoàn thành danh sách dịch vụ AWS dự kiến sử dụng trong đề tài. |
| Ngày 6 (07/06) | Lập kế hoạch triển khai | Chia hệ thống thành các module chức năng và xây dựng kế hoạch triển khai theo từng giai đoạn để hỗ trợ hoàn thiện nội dung báo cáo. | Hoàn thành roadmap triển khai và định hướng phát triển nội dung. |
| Ngày 7 (08/06) | Tổng kết và chuẩn bị nội dung | Rà soát kiến trúc tổng thể, tổng hợp tài liệu nghiên cứu và chuẩn bị nội dung cho các phần tiếp theo của worklog. | Hoàn thành nội dung Worklog Tuần 7 và chuẩn bị cho tuần tiếp theo. |

---

## IV. Thực thi kỹ thuật chuyên sâu & Phân tích chi tiết

Sau khi hoàn thành các hoạt động học tập liên quan đến dịch vụ AWS, tôi chuyển sang giai đoạn từ học từng dịch vụ riêng lẻ sang phân tích một hệ thống hoàn chỉnh phục vụ cho báo cáo thực tập. Thay vì chỉ ghi lại các bước lab, tôi tập trung vào việc phân tích bài toán, nghiên cứu kiến trúc và lựa chọn giải pháp phù hợp cho đề tài.

Thông qua quá trình nghiên cứu, tôi lựa chọn đề tài **Mini Video-on-Demand Platform using AWS Serverless**. Mục tiêu của hệ thống là xây dựng một nền tảng cho phép quản trị viên upload video, sau đó hệ thống tự động xử lý video, chuyển đổi video sang nhiều mức chất lượng khác nhau và phân phối nội dung đến người dùng thông qua CDN.

Trong tuần này, tôi chưa tập trung vào việc triển khai chi tiết từng chức năng. Thay vào đó, tôi ưu tiên xây dựng phiên bản đầu tiên của thiết kế kiến trúc tổng thể (**Architecture Draft V1**). Kiến trúc này đóng vai trò định hướng cho quá trình hoàn thiện nội dung và triển khai trong các tuần tiếp theo.

### Initial Serverless Video-on-Demand Architecture

![Week 7 Screenshot](/my-hugo-site/images/1-Workblog/Week-7/image.png)

Dựa trên kiến trúc ban đầu, tôi chia hệ thống thành nhiều lớp chức năng nhằm giảm sự phụ thuộc giữa các thành phần và tận dụng tối đa các dịch vụ managed/serverless của AWS.

### 1. Frontend Layer

Đối với tầng giao diện người dùng, tôi dự kiến sử dụng **React** hoặc **Next.js** để xây dựng web application vì các framework này hỗ trợ tốt cho việc phát triển giao diện hiện đại và có khả năng mở rộng trong tương lai.

Frontend dự kiến được triển khai dưới dạng **Static Website** trên Amazon S3 và phân phối thông qua Amazon CloudFront. Việc sử dụng CloudFront giúp giảm độ trễ truy cập, cải thiện tốc độ tải trang và hỗ trợ khả năng mở rộng tốt hơn khi số lượng người dùng tăng lên.

Đây cũng là mô hình triển khai phổ biến đối với các ứng dụng Serverless, trong đó frontend được tách biệt khỏi backend.

### 2. API and Authentication Layer

Đối với lớp API, tôi lựa chọn **Amazon API Gateway** làm cổng giao tiếp giữa frontend và backend.

Các API dự kiến được xây dựng theo kiến trúc RESTful. Chúng sẽ tiếp nhận request từ người dùng và chuyển tiếp đến các hàm xử lý nghiệp vụ chạy trên AWS Lambda.

Đồng thời, **Amazon Cognito** được lựa chọn để xử lý xác thực và quản lý người dùng. Thay vì tự xây dựng hệ thống đăng nhập thủ công, Cognito cung cấp sẵn cơ chế quản lý User Pool, hỗ trợ JWT Token và các cơ chế phân quyền. Điều này giúp giảm thời gian phát triển đồng thời cải thiện tính bảo mật.

Ở giai đoạn này, tôi mới xác định vai trò của Cognito trong kiến trúc tổng thể và chưa cấu hình chi tiết User Pool hoặc Authorization Flow.

### 3. Business Logic Layer

Logic nghiệp vụ của hệ thống được dự kiến triển khai bằng **AWS Lambda**.

Việc sử dụng Lambda giúp loại bỏ nhu cầu quản lý máy chủ và chỉ phát sinh chi phí khi có request cần xử lý. Điều này phù hợp với hệ thống Video-on-Demand vì lưu lượng người dùng có thể thay đổi theo thời gian.

Trong thiết kế ban đầu, tôi xác định một số nhóm Lambda chính, bao gồm:

- Xử lý API nghiệp vụ.
- Tạo Presigned URL để upload video.
- Cập nhật trạng thái xử lý video.
- Xử lý các event phát sinh từ processing pipeline.

Các Lambda function dự kiến hoạt động độc lập và giao tiếp thông qua API Gateway, Amazon SQS hoặc EventBridge nhằm giảm sự phụ thuộc giữa các thành phần trong hệ thống.

### 4. Data Layer

Đối với tầng lưu trữ dữ liệu, tôi lựa chọn **Amazon DynamoDB** để lưu metadata của video.

Các thông tin dự kiến được lưu trữ bao gồm:

- Video ID.
- Tên video.
- Trạng thái xử lý.
- Đường dẫn lưu trữ.
- Thời gian tạo.
- Danh mục.
- Thông tin người upload.

Lý do lựa chọn DynamoDB thay vì cơ sở dữ liệu quan hệ là vì dịch vụ này có khả năng mở rộng linh hoạt, hiệu năng cao và phù hợp với kiến trúc Serverless.

Trong giai đoạn nghiên cứu, tôi cũng đề xuất sử dụng **Global Secondary Indexes (GSI)** để hỗ trợ truy vấn theo trạng thái video hoặc danh mục. Tuy nhiên, cấu trúc bảng chi tiết sẽ tiếp tục được hoàn thiện trong tuần tiếp theo.

### 5. Video Processing Pipeline

Đây được xem là thành phần quan trọng nhất của toàn bộ hệ thống.

Tôi dự kiến xây dựng pipeline xử lý video theo mô hình **Event-Driven Architecture** nhằm tách biệt quá trình upload video khỏi quá trình xử lý video.

Luồng xử lý dự kiến như sau:

- Quản trị viên yêu cầu upload video.
- Backend tạo Presigned URL.
- Video được upload trực tiếp lên Amazon S3 Raw Bucket.
- Sau khi upload hoàn tất, hệ thống phát sinh event.
- Amazon SQS nhận yêu cầu xử lý.
- AWS Lambda khởi tạo job trên AWS Elemental MediaConvert.
- MediaConvert chuyển đổi video sang nhiều độ phân giải khác nhau.
- Kết quả được lưu trong Processed Bucket.
- EventBridge gửi event hoàn tất.
- Lambda cập nhật trạng thái video trong DynamoDB.

Mô hình bất đồng bộ này giúp backend không phải chờ quá trình chuyển đổi video hoàn tất, từ đó cải thiện khả năng mở rộng và trải nghiệm người dùng.

### 6. Content Delivery

Sau khi quá trình xử lý hoàn tất, video sẽ được phân phối thông qua **Amazon CloudFront**.

CloudFront đóng vai trò như một CDN và hỗ trợ:

- Giảm độ trễ truy cập.
- Cải thiện tốc độ phát video.
- Giảm tải trực tiếp cho S3.
- Sử dụng cache tại các Edge Location.

Kiến trúc này cũng dự kiến hỗ trợ **HLS Streaming**, cho phép người dùng xem video ở nhiều mức chất lượng khác nhau tùy theo tốc độ mạng.

### 7. Định hướng bảo mật và giám sát

Bên cạnh các thành phần chức năng chính, tôi cũng bước đầu xác định một số dịch vụ hỗ trợ cần thiết để đáp ứng yêu cầu vận hành.

Các dịch vụ dự kiến sử dụng gồm:

- **AWS IAM** để quản lý quyền truy cập theo nguyên tắc Least Privilege.
- **AWS KMS** để mã hóa dữ liệu lưu trữ.
- **Amazon CloudWatch** để theo dõi log và hoạt động hệ thống.
- **AWS CloudTrail** để ghi lại lịch sử hoạt động.
- **AWS WAF** để tăng cường bảo vệ các API public.

Vì đây mới là giai đoạn nghiên cứu ban đầu, tôi chưa triển khai cấu hình chi tiết cho các dịch vụ này. Tôi chỉ xác định vai trò của từng thành phần trong kiến trúc tổng thể.

---

## V. Thách thức hạ tầng, Nhật ký xử lý lỗi & Góc nhìn chuyên môn

Vì tuần 7 chủ yếu tập trung vào nghiên cứu và thiết kế kiến trúc ban đầu, tôi chưa gặp nhiều lỗi triển khai thực tế như các tuần lab trước. Tuy nhiên, trong quá trình phân tích và thiết kế hệ thống, tôi xác định một số thách thức quan trọng cần xem xét trước khi bước vào giai đoạn triển khai.

Thách thức đầu tiên liên quan đến việc lựa chọn kiến trúc phù hợp cho workflow xử lý video. File video thường có dung lượng lớn và cần nhiều thời gian để xử lý. Nếu hệ thống sử dụng mô hình xử lý đồng bộ thông qua API Gateway và Lambda, hệ thống có thể dễ gặp lỗi timeout, tăng thời gian chờ và ảnh hưởng tiêu cực đến trải nghiệm người dùng. Để giải quyết vấn đề này, tôi dự kiến sử dụng xử lý bất đồng bộ thông qua Amazon S3, Amazon SQS, AWS Lambda và AWS Elemental MediaConvert.

Thách thức tiếp theo là việc upload các file video lớn từ frontend lên hệ thống. Nếu toàn bộ dữ liệu video phải đi qua backend hoặc API Gateway, hệ thống có thể tăng tải xử lý, tăng chi phí truyền dữ liệu và có nguy cơ vượt giới hạn kích thước request. Vì vậy, tôi đề xuất sử dụng Presigned URL để frontend có thể upload video trực tiếp lên Amazon S3 Raw Bucket. Backend chỉ chịu trách nhiệm xác thực người dùng, tạo URL tạm thời và lưu metadata cần thiết.

Ngoài ra, việc tổ chức dữ liệu giữa Amazon S3 và Amazon DynamoDB cần được thiết kế cẩn thận. Amazon S3 chịu trách nhiệm lưu trữ video gốc và video đã xử lý, trong khi DynamoDB lưu metadata, trạng thái xử lý và thông tin phục vụ truy vấn. Nếu Video ID, Object Key và trạng thái xử lý không được chuẩn hóa ngay từ đầu, dữ liệu giữa hai dịch vụ có thể không nhất quán. Vì vậy, tôi dự kiến sử dụng một Video ID duy nhất làm khóa liên kết giữa DynamoDB, cấu trúc lưu trữ trên S3 và các MediaConvert job.

Một vấn đề khác được xem xét là xử lý event lặp lại. Trong kiến trúc event-driven, một message từ Amazon SQS hoặc EventBridge có thể được gửi lại nếu quá trình xử lý thất bại. Nếu Lambda không được thiết kế theo hướng idempotent, cùng một video có thể tạo ra nhiều MediaConvert job hoặc cập nhật trạng thái nhiều lần. Vì vậy, tôi xác định cần kiểm tra trạng thái video hiện tại trong DynamoDB trước khi thực hiện các thao tác quan trọng và bổ sung Dead-Letter Queue trong giai đoạn triển khai sau.

Về góc độ bảo mật, tôi nhận thấy kiến trúc ban đầu có nhiều thành phần giao tiếp với nhau và mỗi thành phần cần được cấp quyền phù hợp. Quyền quá rộng có thể tạo rủi ro truy cập trái phép, trong khi quyền quá hạn chế có thể khiến hệ thống hoạt động không đúng. Giải pháp dự kiến là tạo IAM Role riêng cho từng Lambda function, áp dụng nguyên tắc Least Privilege và giới hạn quyền theo từng Bucket, Table, Queue hoặc MediaConvert Job cụ thể.

Ngoài ra, việc phân phối nội dung qua CloudFront cần đảm bảo rằng S3 Processed Bucket không được public trực tiếp. Tôi dự kiến giữ tất cả bucket ở trạng thái private và cho phép CloudFront truy cập nội dung thông qua Origin Access Control. Điều này giúp ngăn người dùng truy cập trực tiếp vào S3 Object URL và tăng khả năng kiểm soát luồng phân phối video.

Từ góc nhìn chuyên môn, tôi nhận ra rằng một kiến trúc Cloud tốt không chỉ cần đáp ứng yêu cầu chức năng, mà còn phải cân bằng giữa hiệu năng, khả năng mở rộng, bảo mật, độ tin cậy và chi phí. Kiến trúc hiện tại mới là phiên bản đầu tiên, vì vậy một số thành phần như AWS WAF, KMS, SNS, SES, X-Ray và AWS Config vẫn đang được cân nhắc dựa trên mức độ cần thiết.

---

## VI. Đánh giá và Chiêm nghiệm chuyên môn

Tuần 7 không tập trung vào triển khai kỹ thuật chi tiết mà đóng vai trò là giai đoạn chuyển tiếp từ học tập sang thiết kế một hệ thống thực tế. Mặc dù trong tuần này chưa có nhiều công việc lập trình, đây vẫn là một tuần quan trọng vì các quyết định kiến trúc sẽ ảnh hưởng trực tiếp đến quá trình hoàn thiện đề tài trong các tuần tiếp theo.

Thông qua quá trình tổng hợp kiến thức và phân tích yêu cầu nghiệp vụ, tôi nhận ra rằng lựa chọn kiến trúc phù hợp cũng quan trọng không kém việc triển khai chức năng. Một kiến trúc được thiết kế rõ ràng sẽ giúp quá trình phát triển có định hướng hơn và giảm nhu cầu thay đổi lớn khi bước vào giai đoạn triển khai.

Bên cạnh đó, tôi cũng có cơ hội nhìn lại mối quan hệ giữa các dịch vụ AWS đã học trong suốt chương trình. Trước đây, mỗi dịch vụ được tiếp cận thông qua các bài lab riêng lẻ. Tuy nhiên, khi đặt vào một hệ thống hoàn chỉnh, tôi hiểu rõ hơn cách các dịch vụ như Amazon API Gateway, AWS Lambda, Amazon Cognito, Amazon S3, Amazon DynamoDB, Amazon SQS, Amazon CloudFront và AWS Elemental MediaConvert có thể phối hợp với nhau để tạo thành một workflow xử lý thống nhất.

Ngoài kiến thức kỹ thuật, tuần này cũng giúp tôi cải thiện khả năng phân tích yêu cầu, trình bày ý tưởng và đưa ra quyết định thiết kế dựa trên các tiêu chí như khả năng mở rộng, hiệu năng, bảo mật và chi phí vận hành.

Sau tuần này, tôi nhận thấy việc đầu tư thời gian cho giai đoạn nghiên cứu và thiết kế kiến trúc sẽ tạo nền tảng vững chắc cho quá trình triển khai sau này, đồng thời giúp giảm rủi ro khi hệ thống trở nên phức tạp hơn.

---

## VII. Kế hoạch chiến lược & Lộ trình tối ưu cho tuần tiếp theo

Sau khi hoàn thành phiên bản đầu tiên của thiết kế kiến trúc tổng thể, tôi sẽ chuyển sang giai đoạn hoàn thiện thiết kế kỹ thuật và chuẩn bị triển khai các thành phần đầu tiên của hệ thống.

Trong tuần tiếp theo, tôi dự kiến rà soát và điều chỉnh kiến trúc dựa trên góp ý từ giảng viên hướng dẫn và kết quả tự đánh giá để đảm bảo tính khả thi trước khi bắt đầu phát triển. Đồng thời, tôi sẽ thiết kế chi tiết hơn các thành phần quan trọng như mô hình dữ liệu trong Amazon DynamoDB, cấu trúc Amazon S3 Bucket, quy trình xác thực người dùng bằng Amazon Cognito và các API nghiệp vụ được triển khai thông qua Amazon API Gateway và AWS Lambda.

Ngoài ra, tôi sẽ chuẩn bị môi trường phát triển, xây dựng cấu trúc tài liệu, thiết lập source code repository nếu cần, phân chia các công việc cần hoàn thành và xây dựng roadmap triển khai cho từng module chức năng. Đây sẽ là bước chuẩn bị quan trọng trước khi bước vào giai đoạn triển khai trong các tuần tiếp theo.

Mục tiêu của tuần tiếp theo không chỉ là hoàn thiện thiết kế kỹ thuật mà còn tạo ra một nền tảng phát triển thống nhất. Điều này sẽ giúp việc triển khai các chức năng như Authentication, Video Upload, Video Processing Pipeline và Video Streaming trở nên hiệu quả hơn, đồng thời giảm các thay đổi lớn trong suốt vòng đời của đề tài.