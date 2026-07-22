---
title: "Worklog Tuần 3"
date: 2026-05-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---



## I. Tóm tắt tổng quan

Tuần 3 là giai đoạn tôi bắt đầu chuyển từ việc triển khai hạ tầng cơ bản sang thiết kế các mô hình mạng và vận hành nâng cao hơn trên AWS. Nếu ở các tuần trước tôi chủ yếu làm quen với IAM, VPC, EC2, S3 và RDS, thì trong tuần này tôi tập trung nhiều hơn vào các nội dung liên quan đến giám sát, kiểm soát chi phí, DNS nội bộ, quản trị bằng dòng lệnh, thiết kế mạng nhiều tầng và cân bằng tải.

Các bài lab trong tuần bao gồm **AWS Budgets và Amazon CloudWatch**, **Amazon Route 53 Private Hosted Zone**, **Route 53 Resolver**, **AWS CLI**, **Advanced VPC Topology** và **Elastic Load Balancing**. Đây là những thành phần quan trọng khi xây dựng một hệ thống cloud có tính ổn định, dễ giám sát, bảo mật tốt và có khả năng mở rộng.

Thông qua tuần học này, tôi hiểu rõ hơn rằng một hệ thống AWS thực tế không chỉ bao gồm máy chủ EC2 hay database RDS riêng lẻ. Hệ thống còn cần cơ chế giám sát, cảnh báo, phân giải tên miền nội bộ, định tuyến hợp lý, phân tầng mạng và cân bằng tải để đảm bảo tính sẵn sàng cao.

---

## II. Mục tiêu trong tuần

Trong tuần 3, tôi đặt ra các mục tiêu chính sau:

* Thiết lập ngân sách và cảnh báo chi phí bằng AWS Budgets.
* Tạo dashboard giám sát tài nguyên bằng Amazon CloudWatch.
* Cấu hình CloudWatch Alarm để theo dõi mức sử dụng CPU của EC2.
* Tìm hiểu và tạo Route 53 Private Hosted Zone cho tên miền nội bộ.
* Hiểu vai trò của Route 53 Resolver trong mô hình Hybrid DNS.
* Thực hành quản lý tài nguyên AWS bằng AWS CLI.
* Sử dụng bộ lọc JMESPath để trích xuất dữ liệu EC2 cần thiết.
* Thiết kế thêm subnet riêng cho tầng database.
* Cấu hình route table riêng để cô lập vùng database khỏi Internet.
* Triển khai Application Load Balancer để phân phối traffic đến EC2 instance.
* Hiểu cơ chế Target Group và Health Check trong Elastic Load Balancing.
* Rèn luyện tư duy kiểm soát chi phí và dọn dẹp tài nguyên sau khi thực hành.

---

## III. Nhật ký hoạt động trong tuần

| Thời gian  | Danh mục hoạt động            | Nội dung công việc thực hiện                                                                                     | Kết quả/Minh chứng đạt được                                      |
| ---------- | ----------------------------- | ---------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| Ngày 1     | Giám sát và kiểm soát chi phí | Tạo AWS Budget, cấu hình các mức cảnh báo chi phí, tạo CloudWatch Dashboard và CloudWatch Alarm để theo dõi EC2. | Thiết lập được hệ thống theo dõi chi phí và giám sát CPU cho EC2 |
| Ngày 2     | Quản lý DNS nội bộ            | Tạo Route 53 Private Hosted Zone và liên kết với VPC tùy chỉnh.                                                  | Chuẩn bị được môi trường phân giải tên miền nội bộ               |
| Ngày 3 - 4 | Hybrid DNS                    | Tạo Route 53 Resolver Inbound Endpoint và Outbound Endpoint theo mô hình Multi-AZ.                               | Hiểu cách thiết lập phân giải DNS giữa AWS và hệ thống bên ngoài |
| Ngày 4     | Quản trị bằng AWS CLI         | Sử dụng AWS CLI để truy vấn thông tin EC2, kết hợp JMESPath để lọc dữ liệu và xuất kết quả dạng bảng.            | Quản lý và kiểm tra tài nguyên AWS bằng dòng lệnh thành công     |
| Ngày 5     | Advanced VPC Topology         | Tạo subnet riêng cho tầng database và cấu hình route table riêng không có đường ra Internet Gateway.             | Database subnet được cô lập khỏi truy cập trực tiếp từ Internet  |
| Ngày 6     | Elastic Load Balancing        | Tạo Application Load Balancer, Target Group và đăng ký EC2 instance vào Target Group.                            | Hoàn thành mô hình cân bằng tải cơ bản cho ứng dụng web          |
| Ngày 7     | Tổng hợp và tài liệu hóa      | Ghi chú quá trình thực hành, tổng hợp lỗi gặp phải, chuẩn bị hình minh chứng và cập nhật portfolio Hugo.         | Hoàn thiện nội dung Worklog Tuần 3                               |

---

# IV. Thực hành kỹ thuật chi tiết qua các bài Lab

---

# LAB 7: Amazon CloudWatch & AWS Budgets - Thiết lập giám sát và ngân sách

## 1. Tổng quan

Trong quá trình sử dụng AWS, việc kiểm soát chi phí và giám sát tài nguyên là hai yếu tố rất quan trọng. Nếu không có ngân sách và cảnh báo phù hợp, tài khoản có thể phát sinh chi phí ngoài ý muốn. Nếu không có hệ thống giám sát, người dùng khó phát hiện kịp thời khi tài nguyên gặp lỗi hoặc vượt ngưỡng sử dụng.

Ở bài lab này, tôi thực hành sử dụng **AWS Budgets** để tạo giới hạn chi phí hằng tháng và sử dụng **Amazon CloudWatch** để theo dõi hiệu năng của EC2 instance. Thông qua đó, tôi hiểu rõ hơn cách một kỹ sư cloud theo dõi cả chi phí lẫn trạng thái kỹ thuật của hệ thống.

## 2. Kiến thức đã tìm hiểu

### AWS Budgets

AWS Budgets là dịch vụ cho phép người dùng thiết lập ngân sách chi phí và nhận cảnh báo khi mức sử dụng gần chạm hoặc vượt quá giới hạn đã đặt. Đây là công cụ quan trọng trong quản trị chi phí cloud, đặc biệt khi thực hành trong môi trường Free Tier hoặc Sandbox.

### Amazon CloudWatch

Amazon CloudWatch là dịch vụ dùng để thu thập và theo dõi các chỉ số vận hành của tài nguyên AWS. Với EC2, một chỉ số quan trọng thường được theo dõi là `CPUUtilization`, cho biết mức độ sử dụng CPU của instance.

### CloudWatch Alarm

CloudWatch Alarm cho phép tạo cảnh báo khi một metric vượt qua ngưỡng đã cấu hình. Ví dụ, nếu CPUUtilization vượt quá 80% trong một khoảng thời gian nhất định, Alarm có thể chuyển sang trạng thái `ALARM` để người quản trị biết và xử lý kịp thời.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập dịch vụ AWS Budgets.
* Tạo một monthly cost budget.
* Đặt hạn mức ngân sách là `20.00 USD`.
* Cấu hình ngân sách tập trung vào chi phí liên quan đến EC2.
* Thiết lập nhiều mức cảnh báo như 80%, 100% chi phí dự báo và 100% chi phí thực tế.
* Kiểm tra lại trạng thái ngân sách sau khi tạo.
* Truy cập dịch vụ Amazon CloudWatch.
* Tạo dashboard tên `EC2-Monitor-Dashboard`.
* Thêm widget để theo dõi metric `CPUUtilization` của EC2.
* Tạo CloudWatch Alarm tên `EC2-High-CPU-Alarm`.
* Cấu hình điều kiện cảnh báo khi CPU vượt quá 80%.
* Kiểm tra lại cấu hình và lưu Alarm.

## 4. Minh chứng thực hành

![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%202.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%203.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%204.png)

## 5. Kết quả đạt được

Sau khi hoàn thành bài lab, tôi đã thiết lập được hệ thống kiểm soát chi phí bằng AWS Budgets và hệ thống giám sát tài nguyên bằng Amazon CloudWatch. Tôi cũng hiểu rõ hơn vai trò của budget alert và performance alarm trong việc giúp người quản trị phát hiện sớm các vấn đề về chi phí và hiệu năng.

---

# LAB 8: Amazon Route 53 - Tạo Private Hosted Zone

## 1. Tổng quan

Trong một hệ thống mạng nội bộ, các dịch vụ không nên luôn gọi nhau bằng địa chỉ IP vì IP có thể thay đổi theo thời gian. Việc sử dụng tên miền nội bộ giúp hệ thống dễ quản lý hơn, dễ đọc hơn và phù hợp hơn với mô hình vận hành thực tế.

Ở bài lab này, tôi thực hành tạo **Route 53 Private Hosted Zone** để quản lý tên miền nội bộ trong phạm vi VPC. Qua đó, tôi hiểu cách AWS hỗ trợ phân giải DNS riêng tư cho các tài nguyên không cần public ra Internet.

## 2. Kiến thức đã tìm hiểu

### Private Hosted Zone

Private Hosted Zone là vùng DNS chỉ có hiệu lực bên trong các VPC được liên kết. Khác với Public Hosted Zone, các bản ghi trong Private Hosted Zone không thể được truy vấn trực tiếp từ Internet công cộng.

### DNS Resolution

DNS Resolution giúp tài nguyên phân giải tên miền thành địa chỉ IP. Trong AWS, các thuộc tính DNS của VPC cần được bật để EC2 và các tài nguyên khác có thể phân giải tên miền nội bộ.

### VPC Association

Private Hosted Zone cần được liên kết với một hoặc nhiều VPC. Chỉ các tài nguyên nằm trong VPC được liên kết mới có thể phân giải các bản ghi thuộc hosted zone đó.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập dịch vụ Route 53.
* Chọn Hosted zones.
* Tạo một hosted zone mới.
* Nhập tên miền nội bộ là `hutech.local`.
* Chọn loại hosted zone là Private hosted zone.
* Liên kết hosted zone với VPC tùy chỉnh.
* Kiểm tra hosted zone sau khi tạo.
* Rà soát các bản ghi DNS mặc định do AWS tạo.

## 4. Minh chứng thực hành

![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%205.png)

## 5. Kết quả đạt được

Sau khi hoàn thành bài lab, tôi đã tạo được Private Hosted Zone phục vụ cho việc quản lý DNS nội bộ. Tôi hiểu rằng private DNS rất hữu ích khi các dịch vụ cần giao tiếp với nhau trong nội bộ hệ thống mà không cần công khai ra Internet.

---

# LAB 10: Amazon Route 53 Resolver - Kết nối Hybrid DNS

## 1. Tổng quan

Trong mô hình Hybrid Cloud, các tài nguyên trên AWS và hệ thống nội bộ bên ngoài AWS có thể cần phân giải tên miền của nhau. Để làm được điều đó, cần có cơ chế chuyển tiếp truy vấn DNS một cách an toàn giữa VPC và hệ thống bên ngoài.

Ở bài lab này, tôi thực hành cấu hình **Amazon Route 53 Resolver Endpoints**, bao gồm Inbound Endpoint và Outbound Endpoint. Bài lab giúp tôi hiểu cách AWS hỗ trợ phân giải DNS hai chiều trong mô hình kết nối lai.

## 2. Kiến thức đã tìm hiểu

### Inbound Endpoint

Inbound Endpoint cho phép các truy vấn DNS từ hệ thống bên ngoài, ví dụ môi trường on-premises, được chuyển tiếp vào AWS. Endpoint này sử dụng các Elastic Network Interfaces nằm trong VPC để tiếp nhận truy vấn DNS từ bên ngoài.

### Outbound Endpoint

Outbound Endpoint cho phép tài nguyên trong AWS chuyển tiếp truy vấn DNS ra các DNS server bên ngoài. Điều này hữu ích khi các máy chủ trên AWS cần phân giải tên miền nội bộ nằm ngoài AWS.

### Thiết kế Multi-AZ

Để tăng tính sẵn sàng, Resolver Endpoints nên được triển khai trên ít nhất hai Availability Zones. Điều này giúp hạn chế điểm lỗi đơn lẻ nếu một AZ gặp sự cố.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập dịch vụ Route 53 Resolver.
* Tạo Inbound Endpoint tên `Hutech-Inbound-Endpoint`.
* Chọn VPC tùy chỉnh.
* Cấu hình địa chỉ IP endpoint trên nhiều Availability Zones.
* Kiểm tra trạng thái sau khi tạo Inbound Endpoint.
* Tạo Outbound Endpoint tên `Hutech-Outbound-Endpoint`.
* Sử dụng mô hình triển khai Multi-AZ tương tự.
* Kiểm tra trạng thái endpoint chuyển sang `Operational`.
* Xem lại cấu hình endpoint và các network interfaces liên quan.
* Kiểm tra khả năng phát sinh chi phí và lên kế hoạch dọn dẹp sau khi thực hành.

## 4. Minh chứng thực hành

![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%206.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%207.png)

## 5. Kết quả đạt được

Sau khi hoàn thành bài lab, tôi hiểu được cách Route 53 Resolver hỗ trợ phân giải DNS giữa AWS và hệ thống bên ngoài. Tôi cũng nhận thấy các Resolver Endpoints có thể phát sinh chi phí theo thời gian chạy, vì vậy cần theo dõi và xóa tài nguyên sau khi hoàn thành thực hành nếu không còn sử dụng.

---

# LAB 11: Quản trị hạ tầng bằng AWS CLI

## 1. Tổng quan

AWS CLI cho phép quản lý tài nguyên AWS bằng dòng lệnh thay vì chỉ thao tác trên AWS Console. Đây là kỹ năng quan trọng vì các thao tác dòng lệnh có thể được tự động hóa, lặp lại và tích hợp vào script.

Ở bài lab này, tôi thực hành sử dụng AWS CLI để lấy thông tin EC2 instance và định dạng kết quả bằng JMESPath query. Điều này giúp tôi hiểu cách kỹ sư cloud trích xuất thông tin cần thiết từ các phản hồi API lớn của AWS.

## 2. Kiến thức đã tìm hiểu

### AWS CloudShell

AWS CloudShell là terminal chạy trực tiếp trên trình duyệt do AWS cung cấp. Công cụ này đã tích hợp sẵn AWS CLI và tự động sử dụng quyền của IAM user đang đăng nhập.

### AWS CLI

AWS CLI là công cụ dòng lệnh cho phép tương tác với các dịch vụ AWS thông qua lệnh. CLI rất hữu ích khi cần tự động hóa, kiểm tra trạng thái tài nguyên hoặc xử lý sự cố.

### JMESPath Query

JMESPath là ngôn ngữ truy vấn dùng để lọc dữ liệu JSON. Với tùy chọn `--query`, AWS CLI có thể chỉ trả về những trường dữ liệu cần thiết thay vì hiển thị toàn bộ JSON dài và khó đọc.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Mở AWS CloudShell.
* Kiểm tra môi trường CLI đã sẵn sàng.
* Sử dụng AWS CLI để mô tả EC2 instances.
* Thêm JMESPath query để lọc dữ liệu đầu ra.
* Trích xuất các thông tin quan trọng như Instance ID, trạng thái instance và instance type.
* Hiển thị kết quả ở dạng bảng để dễ đọc hơn.

Lệnh đã sử dụng:

```bash
aws ec2 describe-instances --query "Reservations[*].Instances[*].[InstanceId,State.Name,InstanceType]" --output table
```

## 4. Minh chứng thực hành

![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%208.png)

## 5. Kết quả đạt được

Sau khi hoàn thành bài lab, tôi đã truy xuất thành công thông tin EC2 instance bằng AWS CLI và hiển thị kết quả ở dạng bảng rõ ràng. Tôi hiểu rằng CLI và JMESPath giúp quản lý tài nguyên AWS hiệu quả hơn so với việc kiểm tra thủ công từng tài nguyên trên Console.

---

# LAB 13: Advanced VPC Topology - Cô lập mạng nhiều tầng

## 1. Tổng quan

Khi hệ thống cloud trở nên phức tạp hơn, mô hình mạng phẳng không còn đủ an toàn cho các thành phần nhạy cảm như database. Một hệ thống thực tế nên được chia thành nhiều tầng, ví dụ tầng public, tầng application và tầng database, nhằm kiểm soát tốt hơn luồng truy cập.

Trong bài lab này, tôi thực hành xây dựng mô hình VPC nâng cao bằng cách tạo một subnet riêng cho tầng database và liên kết subnet này với một route table riêng. Qua đó, tôi hiểu rõ hơn cách thiết kế mạng nhiều tầng giúp tăng cường bảo mật cho hệ thống cloud.

## 2. Kiến thức đã tìm hiểu

### Kiến trúc 3 tầng

Kiến trúc 3 tầng chia hệ thống thành ba lớp logic:

* Public/Web Tier: tiếp nhận traffic từ Internet.
* Application Tier: xử lý logic nghiệp vụ.
* Database Tier: lưu trữ dữ liệu nhạy cảm và cần được cô lập khỏi truy cập trực tiếp từ Internet.

### Cô lập bằng Route Table

Route Table quyết định cách traffic di chuyển trong VPC. Khi sử dụng một route table riêng cho database subnet và không thêm route ra Internet Gateway, tầng database có thể được cô lập khỏi Internet công cộng.

### Quy hoạch CIDR

Quy hoạch CIDR giúp chia dải địa chỉ IP rõ ràng, tránh xung đột mạng và tạo nền tảng cho việc cấu hình Security Group hoặc Network ACL chính xác hơn.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập dịch vụ VPC.
* Chọn VPC hiện có là `MyLabVPC`.
* Tạo subnet riêng cho tầng database.
* Đặt tên subnet là `Private-DB-Subnet-1A`.
* Gán CIDR block là `10.0.3.0/24`.
* Kiểm tra subnet sau khi tạo.
* Tạo route table mới tên `DB-Route-Table`.
* Liên kết `Private-DB-Subnet-1A` với `DB-Route-Table`.
* Đảm bảo route table không có route `0.0.0.0/0` trỏ đến Internet Gateway.
* Kiểm tra lại subnet association để xác nhận subnet database đã được cô lập.

## 4. Minh chứng thực hành

![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%209.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%2010.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%2011.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%2012.png)

## 5. Kết quả đạt được

Sau khi hoàn thành bài lab, tôi đã tạo được subnet riêng cho tầng database và liên kết nó với route table riêng. Tôi hiểu rằng việc thiết kế subnet và route table hợp lý có thể giúp ngăn các tài nguyên nhạy cảm bị truy cập trực tiếp từ Internet.

---

# LAB 14: Elastic Load Balancing - Triển khai hệ thống cân bằng tải

## 1. Tổng quan

Trong môi trường production, nếu một ứng dụng chỉ chạy trên một máy chủ duy nhất thì hệ thống sẽ có nguy cơ downtime cao. Khi máy chủ đó gặp lỗi hoặc bị quá tải, toàn bộ ứng dụng có thể bị gián đoạn.

Ở bài lab này, tôi thực hành triển khai **Application Load Balancer - ALB** để phân phối traffic đến các EC2 instances. Bài lab giúp tôi hiểu cách Elastic Load Balancing cải thiện tính sẵn sàng cao và giảm rủi ro điểm lỗi đơn lẻ.

## 2. Kiến thức đã tìm hiểu

### Application Load Balancer

Application Load Balancer hoạt động ở tầng 7 trong mô hình OSI. ALB có thể xử lý traffic HTTP/HTTPS và định tuyến request dựa trên các điều kiện như path, host hoặc header.

### Target Group

Target Group là nhóm tài nguyên nhận traffic từ load balancer, thường là các EC2 instances. ALB sẽ chuyển request của người dùng đến các target đang khỏe mạnh trong target group.

### Health Check

Health Check là cơ chế kiểm tra tình trạng hoạt động của các target. Nếu một EC2 instance không phản hồi đúng, load balancer sẽ đánh dấu instance đó là unhealthy và ngừng chuyển traffic đến instance đó.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập dịch vụ EC2.
* Vào mục Load Balancers.
* Tạo một Application Load Balancer mới.
* Đặt tên load balancer là `MyWebALB`.
* Chọn scheme là Internet-facing.
* Chọn IP address type là IPv4.
* Gắn ALB vào VPC `MyLabVPC`.
* Chọn các public subnets thuộc nhiều Availability Zones.
* Tạo Target Group tên `Web-TG`.
* Chọn giao thức HTTP và port `80`.
* Cấu hình đường dẫn Health Check là `/`.
* Đăng ký các EC2 instances hiện có vào Target Group.
* Kiểm tra lại cấu hình và tạo load balancer.
* Lấy DNS name của ALB sau khi tạo.
* Truy cập thử bằng ALB DNS endpoint.

## 4. Minh chứng thực hành

![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%2013.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%2014.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-3/image%20copy%2015.png)

## 5. Kết quả đạt được

Sau khi hoàn thành bài lab, tôi đã cấu hình được Application Load Balancer và Target Group. Tôi hiểu cách ALB phân phối traffic đến EC2 instances và cách Health Check giúp hệ thống duy trì tính sẵn sàng bằng cách phát hiện các target không hoạt động ổn định.

---

## V. Thách thức gặp phải và cách xử lý

Trong tuần 3, tôi gặp một số khó khăn thực tế khi thực hành với các thành phần hạ tầng nâng cao trên AWS.

### 1. Rủi ro chi phí từ NAT Gateway và Resolver Endpoints

Khi thiết kế VPC hoặc cấu hình Hybrid DNS, AWS có thể đề xuất sử dụng NAT Gateway hoặc Route 53 Resolver Endpoints. Đây là những tài nguyên có thể phát sinh chi phí theo giờ nếu để chạy liên tục.

Để xử lý vấn đề này, tôi kiểm tra kỹ thành phần nào thật sự cần thiết trước khi tạo. Sau khi hoàn thành thực hành, tôi lên kế hoạch xóa các tài nguyên không còn sử dụng để tránh phát sinh chi phí ngoài ý muốn.

### 2. Giới hạn khi sử dụng CloudShell

Trong một số trường hợp, CloudShell có thể gặp giới hạn về khu vực hoặc quyền truy cập tạm thời. Điều này giúp tôi hiểu rằng quá trình vận hành cloud không phải lúc nào cũng diễn ra hoàn hảo.

Cách xử lý là cần sẵn sàng chuyển sang môi trường terminal cục bộ, cấu hình AWS CLI trên máy cá nhân hoặc dùng một shell thay thế để tiếp tục quá trình thực hành.

### 3. Độ phức tạp khi cô lập mạng

Khi xây dựng Advanced VPC Topology, cần đảm bảo các private subnet thật sự không có đường truy cập trực tiếp từ Internet. Nếu liên kết sai route table, một tài nguyên nhạy cảm có thể bị expose ngoài ý muốn.

Tôi xử lý bằng cách kiểm tra kỹ route table association và xác nhận database subnet không có route đi ra Internet Gateway.

### 4. Cấu hình Health Check cho Load Balancer

Khi cấu hình ALB và Target Group, đường dẫn Health Check cần trùng với đường dẫn mà ứng dụng có thể phản hồi hợp lệ. Nếu ứng dụng không phản hồi đúng đường dẫn đã cấu hình, target có thể bị đánh dấu là unhealthy.

Để xử lý, tôi kiểm tra lại cấu hình web server và đảm bảo path `/` có thể trả về HTTP response hợp lệ.

---

## VI. Suy ngẫm sau tuần 3

Tuần 3 giúp tôi hiểu rằng cloud engineering không chỉ là tạo từng dịch vụ AWS riêng lẻ, mà là thiết kế toàn bộ đường đi của dữ liệu trong hệ thống.

Một request từ Internet cần đi qua Internet Gateway, được điều hướng bởi Route Table, đi đến Application Load Balancer, sau đó được phân phối đến các EC2 instances đang hoạt động tốt. Đồng thời, hệ thống còn cần DNS nội bộ, giám sát hiệu năng, cảnh báo chi phí và mạng riêng được cô lập để vận hành ổn định.

Bài học quan trọng nhất tôi rút ra là một kỹ sư cloud cần tư duy theo từng lớp: lớp mạng, lớp bảo mật, lớp compute, lớp DNS, lớp giám sát và lớp chi phí. Mỗi lớp đều ảnh hưởng trực tiếp đến tính ổn định và an toàn của toàn bộ hệ thống.

Tuần này cũng giúp tôi nhận thức rõ hơn về các nguyên tắc trong AWS Well-Architected Framework, đặc biệt là vận hành hiệu quả, bảo mật, độ tin cậy và tối ưu chi phí.

---

## VII. Kế hoạch cho tuần tiếp theo

Trong tuần tiếp theo, tôi sẽ tiếp tục học và thực hành các nội dung nâng cao hơn nhằm cải thiện khả năng mở rộng và hiệu năng của hệ thống.

Các nội dung dự kiến gồm:

* Thực hành Auto Scaling Groups.
* Tìm hiểu cách tự động tăng hoặc giảm số lượng EC2 instances theo nhu cầu tải.
* Nghiên cứu Amazon CloudFront để phân phối nội dung và tối ưu CDN.
* Hiểu cách CDN giúp giảm độ trễ cho người dùng ở nhiều khu vực.
* Tiếp tục thực hành giám sát và kiểm soát chi phí.
* Cập nhật portfolio Hugo với ghi chú kỹ thuật và ảnh minh chứng.
* Xem thêm tài liệu và video kiến trúc AWS để củng cố tư duy thiết kế cloud.

