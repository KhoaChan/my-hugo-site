---
title: "Worklog Tuần 6"
date: 2026-05-22
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---


## I. Tóm tắt tổng quan

Tuần 6 là giai đoạn tôi tập trung vào các nội dung liên quan đến **Data Lake, NoSQL Database, phân tích dữ liệu phi máy chủ và xử lý dữ liệu lớn trên AWS**. Nếu các tuần trước chủ yếu tập trung vào hạ tầng mạng, bảo mật, máy chủ, CI/CD và quản trị quyền truy cập, thì tuần này tôi đi sâu hơn vào cách xây dựng nền tảng dữ liệu phục vụ phân tích và trực quan hóa thông tin.

Các bài lab trong tuần bao gồm **Data Lake trên Amazon S3 kết hợp AWS Glue**, **thiết kế dữ liệu NoSQL với Amazon DynamoDB**, **phân tích chi phí và hiệu năng bằng AWS Glue và Amazon Athena**, cùng với **xây dựng pipeline phân tích dữ liệu đầu cuối bằng Amazon Kinesis, Amazon EMR và Amazon QuickSight**.

Thông qua tuần học này, tôi hiểu rõ hơn vai trò của dữ liệu trong hệ thống cloud hiện đại. Một hệ thống AWS không chỉ cần có máy chủ, mạng và database, mà còn cần có khả năng thu thập, lưu trữ, xử lý, truy vấn và trực quan hóa dữ liệu để hỗ trợ việc ra quyết định.

---

## II. Mục tiêu trong tuần

Trong tuần 6, tôi đặt ra các mục tiêu chính sau:

* Hiểu khái niệm Data Lake và cách xây dựng Data Lake trên AWS.
* Sử dụng Amazon S3 làm nơi lưu trữ dữ liệu thô, dữ liệu bán cấu trúc và dữ liệu đã xử lý.
* Tìm hiểu vai trò của AWS Glue Crawler trong việc tự động phát hiện schema dữ liệu.
* Hiểu cách Glue Data Catalog lưu trữ metadata phục vụ cho các dịch vụ phân tích.
* Tìm hiểu tư duy thiết kế dữ liệu NoSQL trên Amazon DynamoDB.
* Phân tích access patterns trước khi thiết kế bảng DynamoDB.
* Hiểu cách sử dụng Partition Key, Sort Key và Global Secondary Index.
* Thực hành truy vấn dữ liệu lưu trên S3 bằng Amazon Athena.
* Tìm hiểu cách tối ưu chi phí truy vấn bằng định dạng dữ liệu dạng cột như Parquet.
* Xây dựng pipeline phân tích dữ liệu với Kinesis, EMR và QuickSight.
* Rèn luyện tư duy thiết kế hệ thống dữ liệu có khả năng mở rộng và tối ưu chi phí.

---

## III. Nhật ký hoạt động trong tuần

| Thời gian  | Danh mục hoạt động       | Nội dung công việc thực hiện                                                                                      | Kết quả/Minh chứng đạt được                                    |
| ---------- | ------------------------ | ----------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| Ngày 1 - 2 | Data Lake Lifecycle      | Thực hành xây dựng Data Lake bằng Amazon S3, tạo bucket lưu dữ liệu và cấu hình AWS Glue Crawler để quét dữ liệu. | Hoàn thành nền tảng lưu trữ dữ liệu tập trung trên S3          |
| Ngày 3     | NoSQL Data Modeling      | Tìm hiểu thiết kế bảng DynamoDB, xác định Partition Key, Sort Key và chỉ mục phụ dựa trên access patterns.        | Hiểu cách tối ưu schema DynamoDB theo nhu cầu truy vấn thực tế |
| Ngày 4     | Cost & Query Analytics   | Thực hành sử dụng AWS Glue kết hợp Amazon Athena để truy vấn log hoặc dữ liệu lưu trên S3 bằng SQL.               | Truy vấn dữ liệu trên S3 mà không cần quản lý máy chủ          |
| Ngày 5 - 6 | End-to-End Analytics     | Tìm hiểu pipeline phân tích dữ liệu gồm Kinesis, EMR và QuickSight để xử lý và trực quan hóa dữ liệu.             | Hiểu quy trình phân tích dữ liệu từ thu thập đến dashboard     |
| Ngày 7     | Tổng hợp và tài liệu hóa | Tổng hợp ghi chú kỹ thuật, chuẩn bị hình ảnh minh chứng và cập nhật nội dung lên Hugo portfolio.                  | Hoàn thiện nội dung Worklog Tuần 6                             |

---

# IV. Thực hành kỹ thuật chi tiết qua các bài Lab

---

# LAB 35: Data Lake on AWS

## 1. Tổng quan

Data Lake là một kiến trúc lưu trữ dữ liệu tập trung, cho phép lưu nhiều loại dữ liệu khác nhau như dữ liệu thô, dữ liệu bán cấu trúc, log hệ thống hoặc dữ liệu đã được xử lý. Trên AWS, Amazon S3 thường được sử dụng làm nền tảng lưu trữ chính cho Data Lake vì có khả năng mở rộng cao, độ bền dữ liệu lớn và chi phí linh hoạt.

Trong bài lab này, tôi thực hành xây dựng một Data Lake cơ bản bằng Amazon S3, sau đó sử dụng AWS Glue Crawler để tự động quét dữ liệu và tạo metadata trong Glue Data Catalog. Metadata này có thể được sử dụng bởi các dịch vụ phân tích như Amazon Athena để truy vấn dữ liệu trực tiếp.

## 2. Kiến thức đã tìm hiểu

### Amazon S3 trong Data Lake

Amazon S3 đóng vai trò là nơi lưu trữ trung tâm cho nhiều loại dữ liệu. Dữ liệu có thể được tổ chức theo các vùng như raw data, processed data hoặc curated data để dễ quản lý và phục vụ các bước xử lý tiếp theo.

### AWS Glue Crawler

AWS Glue Crawler giúp tự động quét dữ liệu trong S3, nhận diện định dạng dữ liệu và suy luận schema. Sau khi crawler chạy thành công, thông tin schema sẽ được cập nhật vào Glue Data Catalog.

### Glue Data Catalog

Glue Data Catalog là nơi lưu trữ metadata của dữ liệu. Các dịch vụ phân tích như Athena có thể dựa vào metadata này để hiểu cấu trúc dữ liệu và thực hiện truy vấn SQL trên dữ liệu lưu trong S3.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập dịch vụ Amazon S3.
* Tạo bucket phục vụ lưu trữ dữ liệu cho Data Lake.
* Tổ chức dữ liệu theo thư mục logic như `raw/`, `processed/` hoặc `analytics/`.
* Upload dữ liệu mẫu vào S3 bucket.
* Truy cập dịch vụ AWS Glue.
* Tạo IAM Role phù hợp để Glue có quyền đọc dữ liệu từ S3.
* Tạo AWS Glue Crawler.
* Cấu hình đường dẫn dữ liệu đầu vào là thư mục trong S3.
* Chạy crawler để quét dữ liệu.
* Kiểm tra schema được tạo trong Glue Data Catalog.
* Xác nhận bảng metadata đã sẵn sàng cho các dịch vụ phân tích.

## 4. Minh chứng thực hành

![Week 6 Screenshot](/my-hugo-site/images/1-Workblog/Week-6/image.png)
![Week 6 Screenshot](/my-hugo-site/images/1-Workblog/Week-6/image%20copy.png)

## 5. Kết quả đạt được

Sau khi hoàn thành bài lab, tôi hiểu được cách xây dựng nền tảng Data Lake cơ bản trên AWS. Tôi cũng nắm được vai trò của Amazon S3 trong lưu trữ dữ liệu và AWS Glue trong việc tự động phát hiện metadata, giúp dữ liệu có thể được truy vấn và phân tích dễ dàng hơn.

---

# LAB 39: Amazon DynamoDB Immersion Day

## 1. Tổng quan

Amazon DynamoDB là dịch vụ cơ sở dữ liệu NoSQL được quản lý hoàn toàn bởi AWS. Dịch vụ này phù hợp với các ứng dụng yêu cầu hiệu năng cao, độ trễ thấp và khả năng mở rộng lớn. Khác với cơ sở dữ liệu quan hệ truyền thống, thiết kế dữ liệu trong DynamoDB cần dựa nhiều vào các mẫu truy cập dữ liệu của ứng dụng.

Trong bài lab này, tôi tập trung tìm hiểu cách thiết kế bảng DynamoDB, lựa chọn Partition Key, Sort Key và sử dụng Global Secondary Index để hỗ trợ các truy vấn ngoài khóa chính.

## 2. Kiến thức đã tìm hiểu

### NoSQL Data Modeling

Thiết kế dữ liệu NoSQL không tập trung vào chuẩn hóa bảng như mô hình quan hệ. Thay vào đó, cần xác định trước ứng dụng sẽ truy vấn dữ liệu như thế nào, từ đó thiết kế khóa và chỉ mục phù hợp.

### Partition Key và Sort Key

Partition Key quyết định cách dữ liệu được phân phối trong DynamoDB. Sort Key giúp sắp xếp và truy vấn dữ liệu trong cùng một partition. Khi kết hợp hai loại khóa này, có thể thiết kế các mẫu truy vấn linh hoạt hơn.

### Global Secondary Index

Global Secondary Index cho phép truy vấn dữ liệu bằng một khóa khác ngoài khóa chính của bảng. Đây là công cụ quan trọng khi ứng dụng cần nhiều kiểu truy vấn khác nhau.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập dịch vụ Amazon DynamoDB.
* Tạo bảng DynamoDB mới.
* Xác định Partition Key và Sort Key phù hợp.
* Cấu hình chế độ capacity phù hợp với môi trường thực hành.
* Thêm một số dữ liệu mẫu vào bảng.
* Tạo Global Secondary Index nếu cần hỗ trợ thêm access patterns.
* Thực hiện thao tác PutItem để thêm dữ liệu.
* Thực hiện Query hoặc Scan để kiểm tra dữ liệu.
* Quan sát cách DynamoDB trả về kết quả truy vấn.
* Ghi chú lại cách thiết kế khóa ảnh hưởng đến hiệu quả truy vấn.

## 4. Minh chứng thực hành

![Week 6 Screenshot](/my-hugo-site/images/1-Workblog/Week-6/image%20copy%202.png)
![Week 6 Screenshot](/my-hugo-site/images/1-Workblog/Week-6/image%20copy%203.png)

## 5. Kết quả đạt được

Sau bài lab này, tôi hiểu rõ hơn sự khác biệt giữa tư duy thiết kế SQL và NoSQL. Với DynamoDB, việc xác định access patterns trước khi thiết kế bảng là rất quan trọng. Tôi cũng hiểu cách Partition Key, Sort Key và GSI ảnh hưởng đến hiệu năng truy vấn và chi phí vận hành.

---

# LAB 40: Cost and Performance Analysis with AWS Glue and Amazon Athena

## 1. Tổng quan

AWS Glue và Amazon Athena là hai dịch vụ thường được kết hợp để xây dựng hệ thống phân tích dữ liệu phi máy chủ. AWS Glue giúp tự động phát hiện schema và quản lý metadata, trong khi Amazon Athena cho phép chạy truy vấn SQL trực tiếp trên dữ liệu lưu trong Amazon S3.

Trong bài lab này, tôi thực hành sử dụng Glue và Athena để phân tích dữ liệu log hoặc dữ liệu chi phí lưu trong S3. Đây là cách tiếp cận hiệu quả vì không cần khởi tạo máy chủ phân tích riêng, đồng thời có thể truy vấn dữ liệu khi cần.

## 2. Kiến thức đã tìm hiểu

### Serverless Analytics

Serverless Analytics cho phép phân tích dữ liệu mà không cần quản lý hạ tầng máy chủ. Người dùng chỉ cần chuẩn bị dữ liệu, metadata và câu truy vấn.

### Amazon Athena

Amazon Athena cho phép thực hiện truy vấn SQL trực tiếp trên dữ liệu trong S3. Athena tính chi phí dựa trên lượng dữ liệu được quét, vì vậy định dạng dữ liệu và cách tổ chức file ảnh hưởng trực tiếp đến chi phí.

### Định dạng Parquet

Parquet là định dạng dữ liệu dạng cột, giúp giảm dung lượng quét khi truy vấn. So với CSV hoặc JSON, Parquet thường hiệu quả hơn cho các bài toán phân tích dữ liệu lớn.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Chuẩn bị dữ liệu log hoặc dữ liệu mẫu trên Amazon S3.
* Cấu hình AWS Glue Crawler để quét dữ liệu.
* Tạo hoặc cập nhật bảng metadata trong Glue Data Catalog.
* Truy cập Amazon Athena Query Editor.
* Chọn database và table đã được tạo bởi Glue.
* Viết câu lệnh SQL để truy vấn dữ liệu.
* Chạy truy vấn và kiểm tra kết quả.
* Quan sát dung lượng dữ liệu được quét.
* Ghi chú cách tối ưu truy vấn bằng cách tổ chức dữ liệu và định dạng file phù hợp.

## 4. Minh chứng thực hành

![Week 6 Screenshot](/my-hugo-site/images/1-Workblog/Week-6/image%20copy%204.png)
![Week 6 Screenshot](/my-hugo-site/images/1-Workblog/Week-6/image%20copy%205.png)

## 5. Kết quả đạt được

Sau khi hoàn thành bài lab, tôi hiểu cách sử dụng AWS Glue và Amazon Athena để phân tích dữ liệu lưu trữ trong S3. Tôi cũng nhận ra rằng việc tối ưu định dạng dữ liệu, đặc biệt là chuyển sang Parquet, có thể giúp giảm lượng dữ liệu quét và tối ưu chi phí truy vấn.

---

# LAB 72: Analytics on AWS Workshop

## 1. Tổng quan

Trong các hệ thống hiện đại, dữ liệu có thể được tạo ra liên tục từ người dùng, ứng dụng, thiết bị hoặc log vận hành. Vì vậy, việc xây dựng pipeline xử lý dữ liệu đầu cuối là rất quan trọng để thu thập, xử lý và trực quan hóa dữ liệu phục vụ phân tích.

Trong bài lab này, tôi tìm hiểu mô hình phân tích dữ liệu trên AWS với các dịch vụ như Amazon Kinesis, Amazon EMR và Amazon QuickSight. Mỗi dịch vụ đảm nhận một vai trò khác nhau trong pipeline dữ liệu: Kinesis thu thập luồng dữ liệu, EMR xử lý dữ liệu lớn và QuickSight trực quan hóa kết quả.

## 2. Kiến thức đã tìm hiểu

### Amazon Kinesis

Amazon Kinesis được sử dụng để thu thập và xử lý dữ liệu streaming theo thời gian thực. Dịch vụ này phù hợp với các hệ thống cần xử lý log, sự kiện người dùng hoặc dữ liệu liên tục.

### Amazon EMR

Amazon EMR là dịch vụ xử lý dữ liệu lớn dựa trên các framework như Apache Spark hoặc Hadoop. EMR phù hợp với các tác vụ xử lý phân tán, làm sạch dữ liệu hoặc phân tích dữ liệu quy mô lớn.

### Amazon QuickSight

Amazon QuickSight là dịch vụ trực quan hóa dữ liệu và xây dựng dashboard phân tích. QuickSight giúp chuyển dữ liệu đã xử lý thành biểu đồ, báo cáo và dashboard phục vụ người dùng cuối hoặc doanh nghiệp.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Tìm hiểu kiến trúc tổng thể của pipeline phân tích dữ liệu.
* Tạo hoặc cấu hình Amazon Kinesis Data Stream.
* Xác định số lượng shard hoặc chế độ capacity phù hợp.
* Theo dõi dữ liệu streaming đi vào Kinesis.
* Tìm hiểu cách Amazon EMR xử lý dữ liệu lớn.
* Cấu hình EMR cluster hoặc EMR steps theo mục tiêu lab.
* Chuẩn bị dữ liệu đã xử lý để phục vụ trực quan hóa.
* Kết nối dữ liệu với Amazon QuickSight.
* Tạo biểu đồ hoặc dashboard phân tích.
* Kiểm tra kết quả trực quan hóa và ghi chú lại quy trình.

## 4. Minh chứng thực hành

![Week 6 Screenshot](/my-hugo-site/images/1-Workblog/Week-6/image%20copy%206.png)
![Week 6 Screenshot](/my-hugo-site/images/1-Workblog/Week-6/image%20copy%207.png)

## 5. Kết quả đạt được

Sau bài lab này, tôi hiểu được quy trình xây dựng một hệ thống phân tích dữ liệu đầu cuối trên AWS. Tôi nắm được vai trò của Kinesis trong thu thập dữ liệu streaming, EMR trong xử lý dữ liệu lớn và QuickSight trong trực quan hóa kết quả phân tích.

---

## V. Thách thức gặp phải và cách xử lý

Trong tuần 6, tôi gặp một số vấn đề liên quan đến quyền hạn, chi phí và cách tối ưu dữ liệu khi thực hành các dịch vụ phân tích trên AWS.

### 1. Lỗi quyền khi tạo AWS Glue Crawler

Trong quá trình tạo Glue Crawler, có trường hợp tài khoản thực hành bị giới hạn quyền IAM, dẫn đến việc không thể tạo role tự động hoặc không đủ quyền truy cập vào một số tài nguyên.

Cách xử lý là kiểm tra thông báo lỗi, sử dụng IAM Role có sẵn nếu được cung cấp, hoặc thực hiện các bước mô phỏng metadata và truy vấn dữ liệu bằng Athena trong phạm vi quyền được phép.

### 2. Tối ưu chi phí khi dùng Athena

Athena tính chi phí theo lượng dữ liệu được quét. Nếu dữ liệu ở dạng CSV hoặc JSON lớn, truy vấn có thể tốn nhiều chi phí hơn.

Tôi rút ra rằng cần tổ chức dữ liệu hợp lý, phân vùng dữ liệu theo thời gian hoặc loại dữ liệu, đồng thời ưu tiên sử dụng định dạng cột như Parquet để giảm dung lượng quét.

### 3. Tư duy thiết kế DynamoDB

Ban đầu, tôi có xu hướng thiết kế bảng DynamoDB giống cơ sở dữ liệu quan hệ. Tuy nhiên, sau khi tìm hiểu, tôi nhận ra DynamoDB cần được thiết kế dựa trên access patterns thay vì chỉ dựa vào quan hệ giữa các thực thể.

Cách xử lý là xác định trước các truy vấn chính của ứng dụng, sau đó mới thiết kế Partition Key, Sort Key và GSI phù hợp.

### 4. Quản lý chi phí khi dùng dịch vụ phân tích lớn

Các dịch vụ như EMR, Kinesis hoặc QuickSight có thể phát sinh chi phí nếu để chạy lâu. Vì vậy, cần theo dõi tài nguyên, chụp minh chứng ngay sau khi thực hành và xóa hoặc dừng tài nguyên không còn sử dụng.

---

## VI. Suy ngẫm sau tuần 6

Tuần 6 giúp tôi có cái nhìn rõ hơn về vai trò của dữ liệu trong kiến trúc cloud hiện đại. Trước đây, tôi thường tập trung vào việc triển khai ứng dụng, máy chủ hoặc database. Tuy nhiên, sau tuần này, tôi nhận ra rằng dữ liệu là một phần rất quan trọng để đánh giá hiệu năng hệ thống, tối ưu chi phí và hỗ trợ ra quyết định.

Tôi đặc biệt ấn tượng với cách AWS kết hợp nhiều dịch vụ để tạo thành một pipeline phân tích dữ liệu hoàn chỉnh. Amazon S3 có thể lưu trữ dữ liệu ở quy mô lớn, AWS Glue giúp quản lý metadata, Athena cho phép truy vấn SQL trực tiếp, DynamoDB phục vụ các ứng dụng cần độ trễ thấp, còn Kinesis, EMR và QuickSight hỗ trợ xử lý và trực quan hóa dữ liệu lớn.

Bài học quan trọng nhất trong tuần này là khi thiết kế hệ thống dữ liệu trên cloud, cần cân bằng giữa hiệu năng, chi phí và khả năng mở rộng. Một thiết kế tốt không chỉ giúp truy vấn nhanh hơn mà còn giúp giảm chi phí vận hành lâu dài.

---

## VII. Kế hoạch cho tuần tiếp theo

Trong tuần tiếp theo, tôi sẽ tiếp tục củng cố kiến thức AWS và bắt đầu định hướng rõ hơn cho đồ án thực tế.

Các nội dung dự kiến gồm:

* Tiếp tục hoàn thành các bài lab nâng cao trong chương trình.
* Trao đổi thêm với mentor về các lỗi gặp phải trong quá trình thực hành.
* Nghiên cứu kiến trúc hệ thống đặt tour du lịch Việt Nam tích hợp Chatbot AI.
* Phân tích luồng dữ liệu, dịch vụ chính và kiến trúc tổng thể của đồ án.
* Tìm hiểu cách kết hợp AI service với backend và database.
* Chuẩn bị sơ đồ kiến trúc để gửi mentor góp ý.
* Cập nhật portfolio Hugo với ghi chú kỹ thuật và ảnh minh chứng thực hành.

