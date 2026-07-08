---
title: "Worklog Tuần 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---


## I. Tổng quan tuần 1

Tuần đầu tiên trong chương trình thực tập **First Cloud AI Journey (FCJ)** là giai đoạn giúp tôi làm quen với môi trường học tập, định hướng cách làm việc và xây dựng nền tảng ban đầu về điện toán đám mây. Mục tiêu chính của tuần này không chỉ là hiểu AWS ở mức khái niệm, mà còn từng bước thiết lập môi trường thực hành, bảo mật tài khoản và làm quen với các công cụ cần thiết để triển khai lab trong những tuần tiếp theo.

Trong tuần này, tôi đã hoàn thành các bước quan trọng như tham gia cộng đồng FCJ, tìm hiểu hạ tầng toàn cầu của AWS, cấu hình AWS CLI, thực hành một số dịch vụ cơ bản và thiết lập các nguyên tắc bảo mật ban đầu cho tài khoản AWS. Đặc biệt, tôi đã tham gia các workshop, hoàn thành các bài lab cần thiết và nhận được **$100 AWS Credit** để tiếp tục thực hành trong quá trình học.

## II. Nhật ký hoạt động trong tuần

| Thời gian  | Nội dung thực hiện                         | Mô tả công việc                                                                                                                                                           | Kết quả đạt được                          |
| ---------- | ------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------- |
| Ngày 1 - 2 | Làm quen chương trình và bảo mật tài khoản | Tham gia không gian làm việc của FCJ, đọc các quy định chung, làm quen với cách tổ chức chương trình. Đồng thời tiến hành bảo mật tài khoản AWS Root bằng MFA.            | Tài khoản AWS Root đã được bảo mật cơ bản |
| Ngày 3     | Tìm hiểu hạ tầng AWS                       | Nghiên cứu tổng quan về hạ tầng toàn cầu AWS, bao gồm Region, Availability Zone và Local Zone. Tìm hiểu vai trò của từng thành phần trong việc xây dựng hệ thống ổn định. | Nắm được mô hình hạ tầng toàn cầu của AWS |
| Ngày 4     | Cài đặt môi trường thực hành               | Cài đặt AWS CLI v2, Session Manager Plugin và cấu hình profile trên máy tính cá nhân. Kiểm tra kết nối bằng lệnh STS để xác nhận tài khoản hoạt động đúng.                | Môi trường CLI đã sẵn sàng để thực hành   |
| Ngày 5 - 6 | Thực hành dịch vụ AWS                      | Tham gia workshop “Explore AWS” và thực hiện các bài lab liên quan đến AI/ML, Serverless, Database và Compute.                                                            | Hoàn thành lab và nhận $100 AWS Credit    |
| Ngày 7     | Tổng hợp và tài liệu hóa                   | Xây dựng cấu trúc ban đầu cho portfolio thực tập bằng Hugo, ghi chú lại quá trình học và kiểm tra các thiết lập chi phí ban đầu trong AWS.                                | Hoàn thiện nền tảng nhật ký thực tập      |

## III. Nội dung kỹ thuật đã tìm hiểu

### 1. Quản lý định danh và bảo mật với IAM

Trong tuần đầu, tôi nhận thấy bảo mật là một phần rất quan trọng khi làm việc với AWS. Vì vậy, tôi đã tập trung tìm hiểu và thực hành các bước cơ bản liên quan đến **IAM - Identity and Access Management**.

Các nội dung đã thực hiện gồm:

* Tạo người dùng IAM riêng để sử dụng thay cho tài khoản Root trong các hoạt động thường ngày.
* Bật MFA cho tài khoản Root nhằm tăng mức độ bảo vệ khi đăng nhập.
* Tìm hiểu nguyên tắc **Least Privilege**, tức là chỉ cấp quyền vừa đủ cho từng người dùng hoặc dịch vụ.
* Đọc và phân tích cấu trúc IAM Policy dạng JSON để hiểu cách AWS kiểm soát quyền truy cập.
* Phân biệt vai trò của User, Group, Role và Policy trong hệ thống phân quyền của AWS.

Qua phần này, tôi hiểu rằng việc sử dụng tài khoản Root trực tiếp là không an toàn. Thay vào đó, cần tạo IAM User hoặc IAM Role phù hợp với từng mục đích sử dụng để giảm thiểu rủi ro về bảo mật.

### 2. Làm quen với AWS CLI và môi trường cục bộ

Sau khi tìm hiểu AWS Management Console, tôi tiếp tục cài đặt và cấu hình **AWS CLI** trên máy tính cá nhân. Việc sử dụng CLI giúp tôi thao tác với AWS nhanh hơn, đồng thời chuẩn bị tốt hơn cho các bài lab tự động hóa sau này.

Các thao tác đã thực hiện:

* Cài đặt AWS CLI v2.
* Cấu hình Access Key, Secret Access Key và Region mặc định.
* Tạo profile để quản lý thông tin đăng nhập.
* Kiểm tra danh tính tài khoản bằng lệnh `aws sts get-caller-identity`.
* Làm quen với một số lệnh cơ bản để kiểm tra dịch vụ và tài nguyên AWS.

Thông qua phần này, tôi hiểu được sự khác nhau giữa thao tác trên giao diện Console và thao tác bằng dòng lệnh. Console trực quan và dễ sử dụng, còn CLI phù hợp hơn khi cần tự động hóa hoặc thao tác nhanh với nhiều tài nguyên.

### 3. Khám phá các dịch vụ AWS cơ bản

Trong quá trình tham gia workshop và thực hành lab, tôi đã có cơ hội tiếp cận một số dịch vụ phổ biến của AWS, bao gồm:

#### Amazon EC2

Tôi đã tìm hiểu cách khởi tạo một máy chủ ảo EC2, lựa chọn instance type phù hợp, cấu hình Security Group và kết nối vào máy chủ bằng SSH. Qua đó, tôi hiểu rõ hơn về cách triển khai một môi trường máy chủ trên cloud.

#### AWS Lambda

Tôi thực hành tạo một hàm serverless cơ bản. Điểm tôi ấn tượng là Lambda cho phép chạy code mà không cần trực tiếp quản lý máy chủ. Điều này giúp tôi hiểu rõ hơn về mô hình serverless và cách tập trung vào logic xử lý thay vì hạ tầng.

#### Amazon RDS

Tôi tìm hiểu cách triển khai cơ sở dữ liệu quan hệ trên AWS. Các khái niệm như Automated Backup, Multi-AZ và cấu hình database instance giúp tôi hiểu thêm về tính sẵn sàng và độ an toàn dữ liệu trong môi trường cloud.

#### Amazon Bedrock

Tôi làm quen với Amazon Bedrock và các Foundation Model. Việc thử nghiệm model qua playground giúp tôi hiểu cơ bản về cách gửi prompt, phản hồi của mô hình, token và độ trễ khi xử lý yêu cầu AI.

#### AWS Budgets

Tôi tạo cảnh báo ngân sách để theo dõi chi phí sử dụng AWS. Đây là bước rất cần thiết khi sử dụng tài khoản Free Tier hoặc credit, giúp tránh phát sinh chi phí ngoài ý muốn.

## IV. Vấn đề gặp phải và cách xử lý

Trong quá trình sử dụng AWS CLI, tôi gặp lỗi **AccessDenied** khi thử liệt kê danh sách S3 bucket. Ban đầu, tôi nghĩ lỗi đến từ cấu hình CLI, nhưng sau khi kiểm tra lại thì nguyên nhân là IAM User chưa có quyền phù hợp.

### Nguyên nhân

IAM User đang sử dụng chưa được cấp quyền `s3:ListAllMyBuckets`, nên AWS từ chối yêu cầu truy cập danh sách bucket.

### Cách xử lý

Tôi đã kiểm tra lại IAM Policy và bổ sung quyền cần thiết cho user. Tuy nhiên, thay vì cấp toàn bộ quyền không cần thiết, tôi chỉ thêm quyền liên quan đến thao tác đang thực hiện để tuân thủ nguyên tắc **quyền hạn tối thiểu**.

### Bài học rút ra

Qua lỗi này, tôi hiểu rõ hơn rằng hầu hết các thao tác trên AWS đều phụ thuộc vào quyền IAM. Khi gặp lỗi AccessDenied, cần kiểm tra lại policy, quyền được gán và phạm vi tài nguyên trước khi kết luận lỗi đến từ dịch vụ hoặc công cụ.

## V. Kết quả đạt được trong tuần 1

Sau tuần đầu tiên, tôi đã đạt được các kết quả sau:

* Làm quen với chương trình First Cloud AI Journey và quy trình học tập.
* Hiểu tổng quan về AWS và các nhóm dịch vụ chính như Compute, Storage, Database, Networking, AI/ML.
* Nắm được khái niệm Region, Availability Zone và cách AWS tổ chức hạ tầng toàn cầu.
* Bảo mật tài khoản AWS Root bằng MFA.
* Tạo IAM User để sử dụng cho các hoạt động thực hành hằng ngày.
* Cài đặt và cấu hình thành công AWS CLI trên máy tính cá nhân.
* Thực hành kiểm tra tài khoản AWS bằng CLI.
* Làm quen với các dịch vụ EC2, Lambda, RDS, Bedrock và AWS Budgets.
* Hoàn thành các bài lab cần thiết trong workshop.
* Nhận được **$100 AWS Credit** để tiếp tục thực hành trong các tuần tiếp theo.
* Xây dựng cấu trúc ban đầu cho portfolio ghi lại quá trình thực tập.

## VI. Suy nghĩ sau tuần đầu tiên

Tuần đầu tiên giúp tôi có cái nhìn rõ ràng hơn về AWS và cách một hệ thống cloud được tổ chức. Trước đây, tôi chỉ hiểu đơn giản cloud là nơi để lưu trữ hoặc chạy website. Sau khi thực hành, tôi nhận ra AWS là một hệ sinh thái rất rộng, bao gồm hạ tầng, bảo mật, cơ sở dữ liệu, trí tuệ nhân tạo, tự động hóa và quản lý chi phí.

Điều quan trọng nhất tôi học được trong tuần này là khi làm việc với cloud, không thể chỉ tập trung vào việc “chạy được dịch vụ”. Người dùng cần quan tâm đến bảo mật, phân quyền, chi phí, khả năng mở rộng và cách quản lý tài nguyên một cách có kiểm soát.

Việc nhận được $100 AWS Credit cũng là một động lực lớn để tôi tiếp tục thực hành nhiều hơn trong các tuần tiếp theo. Đây là cơ hội để tôi thử nghiệm các dịch vụ AWS thực tế mà không quá lo lắng về chi phí ban đầu.

## VII. Định hướng cho tuần 2

Trong tuần tiếp theo, tôi sẽ tập trung đi sâu hơn vào các dịch vụ hạ tầng cốt lõi của AWS. Mục tiêu là hiểu rõ cách thiết kế mạng, triển khai máy chủ, cấu hình database và xây dựng hệ thống có khả năng mở rộng.

Các nội dung dự kiến thực hiện trong tuần 2 gồm:

* Lab 1: Tìm hiểu IAM nâng cao và Organizational Unit.
* Lab 2: Thiết kế mạng với Amazon VPC.
* Lab 3: Thực hành EC2 kết hợp Auto Scaling và Load Balancing.
* Lab 4: Khởi tạo và cấu hình máy chủ ảo Windows/Linux trên EC2.
* Lab 5: Triển khai và quản trị cơ sở dữ liệu với Amazon RDS.
* Lab 6: Deploy ứng dụng FCJ Management Application với Auto Scaling Group.



