---
title: "Worklog Tuần 9"
date: 2026-06-12
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

# Worklog Tuần 9

## I. Tóm tắt tổng quan

Sau khi hoàn thiện phần thiết kế kỹ thuật ở tuần trước, trong tuần 9 tôi bắt đầu chuyển sang giai đoạn chuẩn bị hạ tầng AWS cho hệ thống **Mini Video-on-Demand Platform using AWS Serverless**. Mục tiêu chính của tuần này là xác định các tài nguyên AWS cần thiết cho giai đoạn triển khai đầu tiên và chuẩn bị cấu trúc kỹ thuật trước khi bắt đầu phát triển các chức năng chính.

Trong tuần này, tôi tập trung rà soát lại các dịch vụ sẽ được sử dụng trong hệ thống, bao gồm **Amazon S3**, **Amazon DynamoDB**, **Amazon API Gateway**, **AWS Lambda** và **Amazon Cognito**. Đây là những thành phần nền tảng giúp hệ thống có thể xác thực người dùng, tiếp nhận yêu cầu upload video, lưu trữ metadata và chuẩn bị cho quy trình xử lý video tự động.

Bên cạnh đó, tôi cũng tiếp tục hoàn thiện thiết kế cấu trúc S3 Bucket, mô hình bảng DynamoDB, danh sách API endpoint và trách nhiệm của từng Lambda function. Việc chuẩn bị kỹ các thành phần này giúp quá trình triển khai ở các tuần tiếp theo rõ ràng hơn và hạn chế lỗi phát sinh khi tích hợp các dịch vụ AWS với nhau.

Kết thúc tuần 9, tôi đã hoàn thành danh sách tài nguyên AWS cần sử dụng, xác định cấu trúc lưu trữ video, cập nhật thiết kế metadata, xây dựng bản nháp API và hoàn thiện luồng xác thực bằng Amazon Cognito.

---

## II. Mục tiêu chiến lược trong tuần

Mục tiêu trọng tâm của tuần 9 là chuẩn bị hạ tầng AWS ban đầu và cụ thể hóa các tài nguyên cần thiết để phục vụ giai đoạn triển khai hệ thống.

Các mục tiêu chính trong tuần bao gồm:

- Rà soát lại toàn bộ dịch vụ AWS cần sử dụng trong giai đoạn triển khai đầu tiên.
- Xác định cấu trúc Amazon S3 Bucket cho video gốc, video đã xử lý và tài nguyên frontend.
- Hoàn thiện mô hình dữ liệu Amazon DynamoDB dùng để lưu metadata video.
- Xây dựng danh sách API endpoint đầu tiên cho chức năng quản lý video.
- Xác định trách nhiệm của từng AWS Lambda function trong backend.
- Rà soát luồng xác thực người dùng bằng Amazon Cognito.
- Cập nhật tài liệu kỹ thuật và chuẩn bị minh chứng cho worklog.

Thông qua các mục tiêu trên, tôi hướng đến việc tạo ra một nền tảng triển khai rõ ràng trước khi bắt đầu cấu hình và phát triển các thành phần thực tế của hệ thống.

---

## III. Nhật ký hoạt động & Lộ trình phân bổ chi tiết (Từ 16/06/2026 đến 22/06/2026)

| Thời gian | Danh mục hoạt động | Chi tiết các tác vụ thực hiện | Kết quả/Minh chứng đạt được |
| --- | --- | --- | --- |
| Ngày 1 (16/06) | Chuẩn bị hạ tầng AWS | Rà soát các dịch vụ AWS cần thiết cho hệ thống và lên kế hoạch thiết lập hạ tầng ban đầu, bao gồm S3, DynamoDB, API Gateway, Lambda và Cognito. | Hoàn thành danh sách tài nguyên AWS cần sử dụng cho giai đoạn triển khai đầu tiên. |
| Ngày 2 (17/06) | Cấu hình Amazon S3 | Lên kế hoạch cấu trúc S3 Bucket để lưu trữ video gốc, video đã xử lý và tài nguyên frontend tĩnh. | Hoàn thành thiết kế cấu trúc lưu trữ S3 phục vụ upload và phân phối video. |
| Ngày 3 (18/06) | Hoàn thiện thiết kế DynamoDB | Rà soát thiết kế bảng DynamoDB, kiểm tra access pattern và điều chỉnh các trường như Video ID, trạng thái, danh mục, người sở hữu và thời gian upload. | Hoàn thành phiên bản cập nhật của thiết kế metadata trên DynamoDB. |
| Ngày 4 (19/06) | Lập kế hoạch API Gateway | Xác định nhóm API endpoint đầu tiên cho quản lý video, bao gồm yêu cầu upload, danh sách video, chi tiết video và kiểm tra trạng thái xử lý. | Hoàn thành phiên bản đầu tiên của danh sách API endpoint. |
| Ngày 5 (20/06) | Lập kế hoạch Lambda Function | Xác định các Lambda function phục vụ tạo Presigned URL, lưu metadata, cập nhật trạng thái xử lý và truy xuất thông tin video. | Hoàn thành bản phân chia trách nhiệm cho các Lambda function. |
| Ngày 6 (21/06) | Rà soát luồng xác thực | Rà soát luồng xác thực bằng Amazon Cognito và xác định cách sử dụng JWT Token để bảo vệ các API endpoint trên API Gateway. | Hoàn thành bản nháp luồng authentication và authorization. |
| Ngày 7 (22/06) | Tổng kết tuần | Tổng hợp nội dung chuẩn bị hạ tầng, kiểm tra các phần còn thiếu và cập nhật worklog với ghi chú kỹ thuật và hình ảnh minh chứng. | Hoàn thành tài liệu Worklog Tuần 9 và chuẩn bị cho giai đoạn triển khai tiếp theo. |

---

## IV. Thực thi kỹ thuật chuyên sâu & Phân tích chi tiết

Trong tuần 9, tôi bắt đầu chuyển từ giai đoạn thiết kế kỹ thuật sang giai đoạn chuẩn bị hạ tầng triển khai. Mặc dù chưa tập trung vào lập trình chi tiết, tuần này đóng vai trò quan trọng vì các tài nguyên AWS được xác định trong giai đoạn này sẽ trở thành nền tảng cho toàn bộ hệ thống backend và pipeline xử lý video.

Trước tiên, tôi rà soát lại các dịch vụ AWS chính cần sử dụng trong đề tài. Các dịch vụ này bao gồm Amazon S3 để lưu trữ video, Amazon DynamoDB để lưu metadata, Amazon API Gateway để cung cấp API, AWS Lambda để xử lý logic nghiệp vụ và Amazon Cognito để xác thực người dùng. Việc xác định rõ vai trò của từng dịch vụ giúp tôi tránh triển khai dư thừa và đảm bảo hệ thống vẫn giữ đúng định hướng Serverless.

### 1. Chuẩn bị Amazon S3 Storage Structure

Đối với Amazon S3, tôi dự kiến tách dữ liệu thành nhiều nhóm lưu trữ khác nhau để dễ quản lý và bảo mật hơn.

Cấu trúc lưu trữ dự kiến gồm:

- Raw Upload Bucket dùng để lưu video gốc do người dùng upload.
- Processed Media Bucket dùng để lưu video đã xử lý sau khi MediaConvert hoàn tất.
- Static Frontend Bucket dùng để lưu các file tĩnh của web application.

Việc tách riêng video gốc và video đã xử lý giúp pipeline hoạt động rõ ràng hơn. Đồng thời, quyền truy cập của từng bucket cũng có thể được kiểm soát riêng biệt. Raw Upload Bucket chỉ cần nhận file upload từ người dùng thông qua Presigned URL, trong khi Processed Media Bucket sẽ được dùng để phục vụ phát video thông qua CloudFront.

### 2. Hoàn thiện thiết kế DynamoDB

Ở tuần trước, tôi đã xây dựng mô hình dữ liệu ban đầu cho DynamoDB. Trong tuần này, tôi tiếp tục rà soát lại access pattern để bảo đảm bảng dữ liệu có thể phục vụ tốt cho các API chính.

Bảng metadata video dự kiến lưu các thông tin như:

- Video ID.
- Tên video.
- Trạng thái xử lý.
- Người upload.
- Thời gian upload.
- Danh mục video.
- Đường dẫn file gốc.
- Đường dẫn file đã xử lý.
- Thông tin lỗi nếu quá trình xử lý thất bại.

Tôi cũng cân nhắc việc sử dụng trạng thái xử lý như `UPLOADED`, `PROCESSING`, `COMPLETED` và `FAILED` để frontend có thể hiển thị tiến độ xử lý video một cách rõ ràng hơn.

### 3. Xác định API Gateway Endpoint

Đối với API Gateway, tôi bắt đầu xây dựng danh sách các endpoint cần thiết cho giai đoạn đầu.

Các endpoint dự kiến gồm:

- API tạo Presigned URL để upload video.
- API lưu metadata video.
- API lấy danh sách video.
- API lấy chi tiết một video.
- API kiểm tra trạng thái xử lý video.
- API cập nhật trạng thái video sau khi pipeline xử lý hoàn tất.

Việc xác định endpoint trước khi lập trình giúp quá trình phát triển backend rõ ràng hơn. Đồng thời, frontend cũng có thể dựa vào danh sách API này để thiết kế giao diện và luồng tương tác phù hợp.

### 4. Phân chia trách nhiệm Lambda Function

Trong kiến trúc Serverless, AWS Lambda là thành phần xử lý logic chính. Vì vậy, tôi tiến hành chia trách nhiệm cho từng Lambda function thay vì gom toàn bộ xử lý vào một hàm duy nhất.

Các Lambda function dự kiến gồm:

- Lambda tạo Presigned URL.
- Lambda ghi metadata vào DynamoDB.
- Lambda lấy danh sách video.
- Lambda lấy chi tiết video.
- Lambda cập nhật trạng thái xử lý.
- Lambda kích hoạt hoặc hỗ trợ pipeline xử lý video.

Cách phân chia này giúp từng Lambda có trách nhiệm rõ ràng, dễ kiểm thử và dễ áp dụng nguyên tắc Least Privilege khi cấp quyền IAM.

### 5. Rà soát luồng xác thực bằng Amazon Cognito

Đối với phần xác thực, tôi tiếp tục rà soát cách Amazon Cognito hoạt động trong hệ thống. Người dùng sau khi đăng nhập sẽ nhận JWT Token. Token này được gửi kèm trong request đến API Gateway để xác thực quyền truy cập.

Luồng xác thực dự kiến gồm:

- Người dùng đăng nhập thông qua Cognito.
- Cognito cấp JWT Token.
- Frontend gửi token trong header của request.
- API Gateway kiểm tra token thông qua Cognito Authorizer.
- Request hợp lệ mới được chuyển đến Lambda xử lý.

Cách làm này giúp backend không cần tự xây dựng toàn bộ hệ thống authentication, đồng thời tăng tính bảo mật và giảm độ phức tạp của code.

---

## V. Thách thức hạ tầng, Nhật ký xử lý lỗi & Góc nhìn chuyên môn

Trong tuần 9, thách thức lớn nhất của tôi là phải chuyển từ ý tưởng thiết kế sang các tài nguyên cụ thể trên AWS. Khi thiết kế tổng quan, việc lựa chọn dịch vụ có thể khá rõ ràng. Tuy nhiên, khi bắt đầu xác định tên bucket, tên bảng, cấu trúc dữ liệu, endpoint và quyền truy cập, tôi nhận thấy hệ thống cần có quy ước thống nhất ngay từ đầu.

Một vấn đề quan trọng là cách đặt tên tài nguyên. Nếu tên bucket, table, Lambda function và API endpoint không thống nhất, quá trình triển khai và đọc báo cáo sẽ trở nên khó theo dõi. Vì vậy, tôi định hướng sử dụng quy ước đặt tên rõ ràng theo chức năng, ví dụ raw-upload, processed-media, videos-table hoặc generate-upload-url.

Bên cạnh đó, thiết kế quyền IAM cũng là một thách thức cần lưu ý. Mỗi Lambda chỉ nên có quyền cần thiết để thực hiện nhiệm vụ của nó. Ví dụ Lambda tạo Presigned URL chỉ cần quyền ghi vào Raw Upload Bucket, trong khi Lambda đọc danh sách video chỉ cần quyền đọc từ DynamoDB. Việc phân quyền quá rộng có thể gây rủi ro bảo mật, còn phân quyền quá hẹp có thể làm hệ thống lỗi khi chạy.

Một khó khăn khác là xác định trạng thái xử lý video sao cho đủ rõ ràng nhưng không quá phức tạp. Nếu chỉ lưu trạng thái đơn giản như uploaded hoặc completed thì frontend khó hiển thị tiến độ chi tiết. Ngược lại, nếu chia quá nhiều trạng thái thì backend phải xử lý nhiều trường hợp hơn. Vì vậy, tôi chọn các trạng thái cơ bản như uploaded, processing, completed và failed để cân bằng giữa tính rõ ràng và độ phức tạp.

Ngoài ra, việc chuẩn bị API endpoint cũng cần tính đến khả năng mở rộng sau này. Các endpoint cần được đặt tên dễ hiểu, đúng mục đích và có thể mở rộng thêm các chức năng như tìm kiếm video, lọc theo danh mục hoặc hiển thị video theo người upload.

Thông qua quá trình chuẩn bị hạ tầng, tôi nhận thấy rằng một hệ thống Cloud không chỉ phụ thuộc vào việc chọn đúng dịch vụ, mà còn phụ thuộc rất nhiều vào cách tổ chức tài nguyên, cách đặt tên, cách phân quyền và cách định nghĩa luồng dữ liệu giữa các thành phần.

---

## VI. Đánh giá và Chiêm nghiệm chuyên môn

Tuần 9 giúp tôi hiểu rõ hơn tầm quan trọng của giai đoạn chuẩn bị hạ tầng trong một hệ thống Serverless. Trước đây, tôi thường nghĩ rằng chỉ cần biết dịch vụ nào cần dùng là có thể bắt đầu triển khai. Tuy nhiên, khi bước vào giai đoạn chuẩn bị thực tế, tôi nhận ra rằng cần xác định rất nhiều chi tiết như cấu trúc S3 Bucket, mô hình bảng DynamoDB, API endpoint, Lambda function và quyền IAM.

Thông qua việc phân tích từng thành phần, tôi hiểu rõ hơn cách các dịch vụ AWS phối hợp với nhau trong hệ thống. Amazon S3 không chỉ là nơi lưu file video, mà còn là điểm bắt đầu của pipeline xử lý. DynamoDB không chỉ lưu dữ liệu, mà còn phản ánh trạng thái hoạt động của toàn bộ hệ thống. API Gateway và Lambda không chỉ xử lý request, mà còn đóng vai trò kết nối giữa frontend, storage, database và pipeline.

Ngoài ra, tôi cũng nhận thấy rằng việc chuẩn bị tài liệu kỹ thuật trước khi triển khai giúp giảm rủi ro và tránh thay đổi lớn trong quá trình làm việc. Khi các endpoint, function và bảng dữ liệu đã được xác định rõ, quá trình lập trình và kiểm thử sẽ có định hướng cụ thể hơn.

Tuần này cũng giúp tôi cải thiện kỹ năng tổ chức hệ thống, tư duy phân quyền và xây dựng kiến trúc theo hướng dễ mở rộng. Đây là những kỹ năng cần thiết khi làm việc với các hệ thống Cloud thực tế.

---

## VII. Kế hoạch chiến lược & Lộ trình tối ưu cho tuần tiếp theo

Trong tuần tiếp theo, tôi sẽ bắt đầu xây dựng các module backend nền tảng dựa trên tài liệu hạ tầng đã chuẩn bị trong tuần 9.

Các công việc trọng tâm dự kiến bao gồm cấu hình Amazon Cognito cho xác thực người dùng, thiết lập Amazon S3 Bucket, tạo bảng Amazon DynamoDB, xây dựng các Lambda function đầu tiên và kết nối chúng với Amazon API Gateway.

Bên cạnh đó, tôi sẽ tập trung vào chức năng upload video thông qua Presigned URL. Đây là một chức năng quan trọng vì nó giúp người dùng upload file trực tiếp lên S3 mà không cần truyền toàn bộ video qua backend. Sau khi upload thành công, hệ thống sẽ lưu metadata vào DynamoDB để chuẩn bị cho pipeline xử lý video ở các bước sau.

Tôi cũng dự kiến kiểm thử từng thành phần sau khi cấu hình để bảo đảm hệ thống hoạt động ổn định trước khi tích hợp đầy đủ. Việc triển khai theo từng bước nhỏ sẽ giúp dễ phát hiện lỗi và điều chỉnh thiết kế khi cần thiết.

Mục tiêu của tuần tiếp theo là hoàn thành phần backend nền tảng, bao gồm authentication, upload API, metadata handling và các API cơ bản phục vụ quản lý video.