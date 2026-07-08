---
title: "Worklog Tuần 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.2. </b> "
---


# Worklog Tuần 2

## I. Tóm tắt tổng quan

Tuần 2 là giai đoạn tôi bắt đầu đi sâu hơn vào các thành phần hạ tầng cốt lõi trên AWS. Nếu tuần đầu tiên chủ yếu tập trung vào việc làm quen với môi trường thực tập, tìm hiểu tổng quan về AWS và thiết lập các công cụ ban đầu, thì trong tuần này tôi đã trực tiếp thực hành thiết kế, cấu hình và kiểm tra nhiều dịch vụ quan trọng trong hệ sinh thái điện toán đám mây.

Nội dung thực hành trong tuần trải dài qua nhiều nhóm dịch vụ khác nhau, bao gồm: hạ tầng toàn cầu AWS, quản lý định danh và phân quyền với IAM, thiết kế mạng riêng ảo bằng VPC, triển khai máy chủ ảo EC2, lưu trữ website tĩnh bằng S3 và khởi tạo cơ sở dữ liệu quan hệ với Amazon RDS.

Thông qua các bài lab, tôi hiểu rõ hơn cách các dịch vụ AWS kết nối với nhau để tạo thành một hệ thống hoàn chỉnh. Bên cạnh việc tạo tài nguyên, tôi cũng chú ý đến các yếu tố quan trọng như bảo mật tài khoản, kiểm soát quyền truy cập, cấu hình mạng, quản lý chi phí và dọn dẹp tài nguyên sau khi thực hành.

## II. Mục tiêu trong tuần

Trong tuần 2, tôi đặt ra các mục tiêu chính sau:

* Tìm hiểu cách AWS tổ chức hạ tầng toàn cầu thông qua Region, Availability Zone và Edge Location.
* Củng cố kiến thức về bảo mật tài khoản và quản lý quyền truy cập bằng IAM.
* Tạo IAM Admin User riêng để hạn chế sử dụng tài khoản Root trong các thao tác hằng ngày.
* Thiết kế một mạng ảo riêng biệt bằng Amazon VPC.
* Cấu hình Public Subnet, Internet Gateway và Route Table để kiểm soát luồng truy cập Internet.
* Triển khai máy chủ ảo Linux bằng Amazon EC2.
* Sử dụng User Data để tự động cài đặt Apache Web Server khi khởi tạo EC2.
* Tạo S3 Bucket và cấu hình Static Website Hosting.
* Triển khai cơ sở dữ liệu MySQL bằng Amazon RDS.
* Nhận biết các lỗi thường gặp trong quá trình cấu hình và biết cách xử lý.
* Kiểm soát chi phí bằng cách lựa chọn cấu hình phù hợp với Free Tier/Sandbox và xóa tài nguyên không cần thiết sau khi hoàn thành lab.

## III. Nhật ký hoạt động trong tuần

| Thời gian  | Danh mục hoạt động            | Nội dung công việc thực hiện                                                                                                                                                                            | Kết quả/Minh chứng đạt được                                           |
| ---------- | ----------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| Ngày 1 - 2 | Hạ tầng toàn cầu và bảo mật   | Tìm hiểu cấu trúc hạ tầng toàn cầu của AWS, bao gồm Region, Availability Zone và Edge Location. Kiểm tra lại bảo mật tài khoản, bật MFA và thiết lập IAM Admin User để sử dụng thay cho tài khoản Root. | Tài khoản Root được bảo vệ bằng MFA, IAM Admin User sẵn sàng sử dụng  |
| Ngày 3     | Quản lý định danh với IAM     | Tạo IAM User, IAM Group và gán quyền thông qua Policy. Tìm hiểu cách IAM Policy hoạt động và áp dụng nguyên tắc Least Privilege.                                                                        | Hiểu cách quản lý quyền truy cập và phân quyền người dùng trên AWS    |
| Ngày 4     | Thiết kế mạng với Amazon VPC  | Tạo VPC riêng, chia Public Subnet, cấu hình Internet Gateway và chỉnh sửa Route Table để subnet có thể kết nối Internet.                                                                                | Hoàn thành mô hình mạng ảo cơ bản phục vụ triển khai EC2              |
| Ngày 5     | Triển khai máy chủ EC2        | Khởi tạo EC2 instance chạy Amazon Linux, cấu hình Key Pair, Security Group và User Data để tự động cài đặt Apache Web Server.                                                                           | Máy chủ EC2 hoạt động và truy cập được qua Public IP                  |
| Ngày 6     | Lưu trữ website tĩnh với S3   | Tạo S3 Bucket, tắt Block Public Access cho mục đích hosting, cấu hình Static Website Hosting và Bucket Policy, sau đó upload file `index.html`.                                                         | Website tĩnh hoạt động thông qua S3 Website Endpoint                  |
| Ngày 7     | Cơ sở dữ liệu RDS và tổng hợp | Tạo RDS MySQL instance, chọn cấu hình phù hợp với Free Tier, kiểm tra trạng thái database và ghi chú lại lỗi trong quá trình thực hành.                                                                 | Database chuyển sang trạng thái Available, hoàn thiện tài liệu tuần 2 |

## IV. Thực hành kỹ thuật chi tiết qua các bài Lab

---

# LAB 1: Giới thiệu hạ tầng toàn cầu AWS

## 1. Tổng quan

Trước khi triển khai các dịch vụ cụ thể trên AWS, tôi cần hiểu cách AWS xây dựng và phân bổ hạ tầng vật lý trên phạm vi toàn cầu. Đây là kiến thức nền tảng quan trọng vì mọi quyết định triển khai dịch vụ đều liên quan đến vị trí đặt tài nguyên, độ trễ, khả năng sẵn sàng và khả năng chịu lỗi của hệ thống.

Bài lab này giúp tôi hình dung rõ hơn cách AWS tổ chức hạ tầng theo nhiều lớp, từ cấp độ toàn cầu đến từng Region, Availability Zone và Edge Location. Nhờ đó, tôi có thể hiểu vì sao khi thiết kế một hệ thống cloud, không nên chỉ quan tâm đến việc “tạo được dịch vụ”, mà còn cần quan tâm đến vị trí triển khai và khả năng dự phòng.

## 2. Kiến thức đã tìm hiểu

### AWS Region

AWS Region là một khu vực địa lý độc lập, nơi AWS đặt các cụm trung tâm dữ liệu. Mỗi Region có thể phục vụ một nhóm người dùng ở một khu vực nhất định và thường được lựa chọn dựa trên các yếu tố như vị trí người dùng, độ trễ, yêu cầu pháp lý và chi phí dịch vụ.

Trong quá trình thực hành, tôi ưu tiên sử dụng Region `us-east-1` vì đây là Region phổ biến, hỗ trợ nhiều dịch vụ AWS và phù hợp cho mục đích học tập, thực hành.

### Availability Zone

Availability Zone là các vùng khả dụng nằm bên trong một Region. Mỗi AZ gồm một hoặc nhiều trung tâm dữ liệu vật lý, có hệ thống điện, mạng và làm mát riêng biệt. Các AZ trong cùng một Region được kết nối với nhau bằng đường truyền tốc độ cao và độ trễ thấp.

Qua phần này, tôi hiểu rằng nếu toàn bộ hệ thống chỉ được triển khai trong một AZ, ứng dụng có thể bị gián đoạn khi AZ đó gặp sự cố. Vì vậy, trong các hệ thống thực tế, tài nguyên thường được phân bổ trên nhiều AZ để tăng tính sẵn sàng.

### Edge Location

Edge Location là các điểm hiện diện của AWS được sử dụng để đưa nội dung đến gần người dùng hơn, đặc biệt khi sử dụng các dịch vụ như Amazon CloudFront. Điều này giúp giảm độ trễ và cải thiện tốc độ truy cập cho người dùng cuối.

## 3. Kết quả đạt được

Sau bài lab này, tôi nắm được cấu trúc phân tầng cơ bản của AWS Global Infrastructure:

**AWS Global Infrastructure → Region → Availability Zone → Data Center → Edge Location**

Tôi cũng hiểu rằng việc lựa chọn Region và thiết kế tài nguyên trên nhiều AZ là một phần quan trọng trong quá trình xây dựng hệ thống có độ sẵn sàng cao, khả năng mở rộng và khả năng chịu lỗi tốt hơn.

---

# LAB 2: AWS IAM Access Control - Thiết lập nền tảng bảo mật

## 1. Tổng quan

Bảo mật là một trong những yếu tố quan trọng nhất khi làm việc trên môi trường cloud. Trong AWS, IAM đóng vai trò quản lý danh tính và quyền truy cập cho người dùng, nhóm người dùng, dịch vụ và ứng dụng.

Ở bài lab này, tôi tập trung vào việc thiết lập một môi trường truy cập an toàn hơn. Thay vì sử dụng tài khoản Root cho các tác vụ hằng ngày, tôi tạo một IAM Admin User riêng để thao tác, đồng thời bật MFA nhằm tăng cường bảo mật.

## 2. Kiến thức đã tìm hiểu

### Nguyên tắc quyền hạn tối thiểu

Nguyên tắc quyền hạn tối thiểu, hay **Principle of Least Privilege**, yêu cầu chỉ cấp đúng những quyền cần thiết cho từng người dùng hoặc dịch vụ. Điều này giúp giảm rủi ro nếu tài khoản bị lộ thông tin hoặc người dùng thao tác sai.

### Tài khoản Root

Tài khoản Root là tài khoản có quyền cao nhất trong AWS. Tài khoản này có thể thay đổi thông tin thanh toán, xóa tài nguyên quan trọng hoặc đóng tài khoản AWS. Vì vậy, tài khoản Root không nên được sử dụng thường xuyên.

Tôi hiểu rằng tài khoản Root chỉ nên dùng trong các trường hợp đặc biệt, còn các thao tác thực hành và quản trị thông thường nên được thực hiện bằng IAM User hoặc IAM Role.

### IAM Policy

IAM Policy là tài liệu JSON định nghĩa quyền truy cập trên AWS. Một policy thường bao gồm các thành phần chính:

* **Effect:** Xác định quyền được cho phép hoặc bị từ chối, ví dụ `Allow` hoặc `Deny`.
* **Action:** Chỉ định hành động được phép thực hiện, ví dụ `ec2:RunInstances` hoặc `s3:CreateBucket`.
* **Resource:** Xác định tài nguyên mà policy áp dụng.
* **Condition:** Điều kiện bổ sung nếu cần giới hạn quyền theo IP, thời gian hoặc ngữ cảnh cụ thể.

## 3. Quá trình thực hiện

Trong quá trình thực hành, tôi thực hiện các bước sau:

* Đăng nhập AWS Console bằng tài khoản Root.
* Truy cập dịch vụ IAM.
* Vào mục Users và chọn Create user.
* Tạo một IAM User có tên là `Admin-Khoa`.
* Tạo IAM Group tên là `Admins`.
* Gán policy `AdministratorAccess` cho group `Admins`.
* Thêm user `Admin-Khoa` vào group `Admins`.
* Đăng xuất tài khoản Root và đăng nhập lại bằng IAM User mới.
* Bật Virtual MFA cho tài khoản Root và IAM User để tăng bảo mật.
* Kiểm tra IAM Dashboard để xác nhận các trạng thái bảo mật cơ bản.

## 4. Minh chứng cần chụp

![Ảnh tạo IAM User Admin-Khoa](static/images/1-Workblog/Week-2/image.png)
![Ảnh tạo IAM User Admin-Khoa](static/images/1-Workblog/Week-2/image copy.png)

## 5. Kết quả đạt được

Sau bài lab này, tôi đã tạo được một IAM Admin User riêng để sử dụng trong quá trình thực hành. Việc tách hoạt động hằng ngày ra khỏi tài khoản Root giúp tài khoản an toàn hơn và phù hợp với nguyên tắc vận hành cloud chuyên nghiệp.

---

# LAB 3: Amazon VPC - Thiết kế mạng ảo riêng biệt

## 1. Tổng quan

Amazon VPC là dịch vụ cho phép tạo một mạng riêng ảo trên AWS. Đây là nền tảng mạng quan trọng để triển khai các tài nguyên như EC2, RDS, Load Balancer và nhiều dịch vụ khác trong một môi trường được kiểm soát.

Trong bài lab này, tôi thực hành tạo VPC riêng, chia subnet, gắn Internet Gateway và cấu hình Route Table để tài nguyên trong public subnet có thể truy cập Internet.

## 2. Kiến thức đã tìm hiểu

### CIDR Block

Khi tạo VPC, cần khai báo dải địa chỉ IP nội bộ bằng CIDR block. Tôi sử dụng dải `10.0.0.0/16` để tạo không gian mạng riêng. Dải này có thể được chia nhỏ thành nhiều subnet phục vụ cho các mục đích khác nhau.

### Public Subnet

Một subnet được xem là public khi Route Table của nó có route đi ra Internet Gateway. Public subnet thường dùng cho các tài nguyên cần nhận truy cập từ Internet như EC2 public hoặc Load Balancer.

### Internet Gateway

Internet Gateway là cổng kết nối giữa VPC và Internet. Để EC2 trong public subnet truy cập được Internet hoặc được truy cập từ Internet, VPC cần được gắn Internet Gateway và Route Table phải có route phù hợp.

### Route Table

Route Table quyết định cách traffic di chuyển trong VPC. Nếu Route Table không có route `0.0.0.0/0` trỏ đến Internet Gateway, tài nguyên trong subnet sẽ không thể giao tiếp với Internet.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập AWS Console và mở dịch vụ VPC.
* Chọn Create VPC.
* Tạo VPC tên `MyLabVPC`.
* Nhập CIDR block là `10.0.0.0/16`.
* Tạo subnet tên `Public-Subnet-1A`.
* Chọn Availability Zone `us-east-1a`.
* Gán CIDR block cho subnet là `10.0.1.0/24`.
* Tạo Internet Gateway tên `MyLab-IGW`.
* Attach Internet Gateway vào `MyLabVPC`.
* Vào Route Tables, chọn route table của VPC.
* Thêm route mới:

  * Destination: `0.0.0.0/0`
  * Target: Internet Gateway `MyLab-IGW`
* Lưu cấu hình và kiểm tra lại trạng thái mạng.

## 4. Minh chứng cần chụp

![Ảnh tạo IAM User Admin-Khoa](static/images/1-Workblog/Week-2/image copy 2.png)
![Ảnh tạo IAM User Admin-Khoa](static/images/1-Workblog/Week-2/image copy 3.png)

## 5. Kết quả đạt được

Sau bài lab này, tôi đã xây dựng được một mạng VPC cơ bản gồm public subnet và Internet Gateway. Đây là nền tảng để tiếp tục triển khai EC2 instance trong bài lab tiếp theo.

---

# LAB 4: Amazon EC2 - Triển khai máy chủ Web Server

## 1. Tổng quan

Sau khi đã có VPC và public subnet, tôi tiếp tục triển khai một máy chủ ảo bằng Amazon EC2. Mục tiêu của bài lab là khởi tạo một EC2 instance chạy Amazon Linux, cấu hình Security Group và sử dụng User Data để tự động cài đặt Apache Web Server.

Việc dùng User Data giúp tôi hiểu cách tự động hóa quá trình cài đặt phần mềm khi máy chủ được khởi tạo, thay vì phải SSH vào máy và cài đặt thủ công từng bước.

## 2. Kiến thức đã tìm hiểu

### Amazon Linux 2023

Amazon Linux 2023 là hệ điều hành Linux do AWS cung cấp và tối ưu cho môi trường cloud. Hệ điều hành này phù hợp để thực hành các bài lab cơ bản với EC2.

### Instance Type

Instance Type quyết định cấu hình phần cứng của EC2 như CPU, RAM và hiệu năng mạng. Trong bài lab, tôi chọn cấu hình nhỏ như `t2.micro` hoặc `t3.micro` để phù hợp với phạm vi Free Tier.

### Security Group

Security Group hoạt động như một tường lửa ảo cho EC2 instance. Tôi cấu hình các rule inbound cần thiết:

* SSH port `22`: chỉ cho phép IP cá nhân truy cập để quản trị.
* HTTP port `80`: cho phép người dùng truy cập website qua trình duyệt.

### User Data

User Data là đoạn script được chạy tự động khi EC2 khởi động lần đầu. Tôi sử dụng User Data để cài đặt Apache Web Server và tạo file `index.html`.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập EC2 Console.
* Chọn Launch instance.
* Đặt tên máy chủ là `MyWebServer`.
* Chọn Amazon Linux 2023 AMI.
* Chọn instance type `t2.micro` hoặc `t3.micro`.
* Tạo Key Pair tên `my-key`.
* Chọn VPC `MyLabVPC`.
* Chọn subnet `Public-Subnet-1A`.
* Bật Auto-assign Public IP.
* Tạo Security Group tên `Web-SG`.
* Mở port `22` cho SSH từ IP cá nhân.
* Mở port `80` cho HTTP từ mọi IPv4.
* Thêm User Data để tự động cài Apache.
* Chọn Launch instance và chờ instance chuyển sang trạng thái Running.
* Truy cập Public IP bằng trình duyệt để kiểm tra website.

Script User Data sử dụng:

```bash
#!/bin/bash
sudo dnf update -y
sudo dnf install httpd -y
sudo systemctl start httpd
sudo systemctl enable httpd
echo "<h1>Welcome to Khoa's AWS Web Server</h1>" > /var/www/html/index.html
```

## 4. Minh chứng cần chụp

![Ảnh tạo IAM User Admin-Khoa](static/images/1-Workblog/Week-2/image copy 4.png)
![Ảnh tạo IAM User Admin-Khoa](static/images/1-Workblog/Week-2/image copy 5.png)
![Ảnh tạo IAM User Admin-Khoa](static/images/1-Workblog/Week-2/image copy 6.png)
![Ảnh tạo IAM User Admin-Khoa](static/images/1-Workblog/Week-2/image copy 7.png)
![Ảnh tạo IAM User Admin-Khoa](static/images/1-Workblog/Week-2/image copy 8.png)

## 5. Kết quả đạt được

Sau khi EC2 chạy thành công, tôi truy cập Public IP trên trình duyệt và website hiển thị nội dung đã cấu hình trong file `index.html`. Điều này chứng minh EC2, Security Group, User Data và Apache Web Server đã hoạt động đúng.

---

# LAB 5: Amazon S3 - Static Website Hosting

## 1. Tổng quan

Amazon S3 là dịch vụ lưu trữ đối tượng của AWS. Ngoài việc dùng để lưu trữ file, hình ảnh hoặc backup, S3 còn có thể được sử dụng để hosting website tĩnh.

Trong bài lab này, tôi tạo một S3 Bucket, bật Static Website Hosting, cấu hình Bucket Policy cho phép đọc file công khai và upload file `index.html` để kiểm tra website.

## 2. Kiến thức đã tìm hiểu

### Object Storage

S3 lưu trữ dữ liệu theo dạng object. Mỗi object bao gồm nội dung file, metadata và key định danh. Cách lưu trữ này khác với mô hình thư mục truyền thống trên máy tính.

### Bucket Name

Tên S3 Bucket phải là duy nhất trên toàn hệ thống AWS. Vì vậy, khi tạo bucket, tôi cần đặt tên riêng để tránh trùng với bucket của tài khoản khác.

### Block Public Access

Mặc định, S3 chặn truy cập công khai để bảo vệ dữ liệu. Khi dùng S3 để hosting website tĩnh, cần cấu hình lại public access một cách có kiểm soát.

### Bucket Policy

Bucket Policy là chính sách phân quyền cấp bucket, được viết bằng JSON. Để website tĩnh có thể truy cập công khai, policy cần cho phép hành động `s3:GetObject`.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập dịch vụ S3.
* Chọn Create bucket.
* Đặt tên bucket, ví dụ `khoa-static-website-2026`.
* Chọn Region `us-east-1`.
* Tắt Block Public Access cho bucket.
* Xác nhận cho phép cấu hình public access.
* Tạo bucket.
* Vào tab Properties.
* Bật Static Website Hosting.
* Chọn Host a static website.
* Nhập Index document là `index.html`.
* Lưu cấu hình.
* Vào tab Permissions.
* Chọn Bucket Policy và thêm policy cho phép public read.
* Upload file `index.html`.
* Truy cập Bucket Website Endpoint để kiểm tra kết quả.

Bucket Policy mẫu:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadGetObject",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::khoa-static-website-2026/*"
    }
  ]
}
```

## 4. Minh chứng cần chụp

![Ảnh tạo IAM User Admin-Khoa](static/images/1-Workblog/Week-2/image copy 9.png)
![Ảnh tạo IAM User Admin-Khoa](static/images/1-Workblog/Week-2/image copy 10.png)
![Ảnh tạo IAM User Admin-Khoa](static/images/1-Workblog/Week-2/image copy 11.png)
![Ảnh tạo IAM User Admin-Khoa](static/images/1-Workblog/Week-2/image copy 12.png)

## 5. Kết quả đạt được

Sau khi hoàn tất cấu hình, website tĩnh đã có thể truy cập thông qua endpoint do S3 cung cấp. Qua bài lab này, tôi hiểu rằng S3 là một giải pháp đơn giản và hiệu quả để triển khai các website tĩnh mà không cần quản lý máy chủ.

---

# LAB 6: Amazon RDS - Khởi tạo cơ sở dữ liệu quan hệ

## 1. Tổng quan

Amazon RDS là dịch vụ cơ sở dữ liệu quan hệ được quản lý bởi AWS. Thay vì tự cài đặt MySQL trên EC2 và tự quản lý backup, cập nhật hoặc giám sát, RDS giúp tự động hóa nhiều tác vụ quản trị cơ sở dữ liệu.

Trong bài lab này, tôi thực hành tạo một RDS MySQL instance, chọn cấu hình phù hợp với mục tiêu học tập và kiểm tra trạng thái hoạt động của database.

## 2. Kiến thức đã tìm hiểu

### Managed Database Service

RDS giúp giảm tải công việc quản trị cơ sở dữ liệu như cài đặt, backup, cập nhật hệ thống và giám sát. Nhờ đó, lập trình viên có thể tập trung nhiều hơn vào việc thiết kế dữ liệu và tối ưu truy vấn.

### Database Engine

RDS hỗ trợ nhiều hệ quản trị cơ sở dữ liệu như MySQL, PostgreSQL, MariaDB, Oracle và SQL Server. Trong bài lab này, tôi chọn MySQL vì đây là hệ quản trị phổ biến và phù hợp với nhiều ứng dụng web.

### DB Instance

DB Instance là máy chủ database được AWS cấp phát. Khi tạo database, cần lựa chọn instance type, storage và network setting phù hợp để tránh phát sinh chi phí không cần thiết.

### Endpoint

Sau khi database được tạo thành công, AWS cung cấp endpoint dạng DNS. Backend application có thể sử dụng endpoint này để kết nối đến database thay vì dùng IP cố định.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập dịch vụ Amazon RDS.
* Chọn Create database.
* Chọn Standard create.
* Chọn database engine là MySQL.
* Chọn template phù hợp cho môi trường học tập.
* Đặt DB instance identifier là `my-khoa-db`.
* Giữ master username là `admin`.
* Chọn Credentials management là Self managed.
* Nhập mật khẩu quản trị phù hợp, ví dụ `KhoaDB.2026`.
* Chọn instance type `db.t3.micro`.
* Chọn storage type là General Purpose SSD `gp3`.
* Đặt Allocated storage là `20 GiB`.
* Kiểm tra lại các thông số network và security.
* Chọn Create database.
* Chờ database chuyển sang trạng thái Available.
* Ghi lại endpoint để sử dụng cho các bài thực hành sau.

## 4. Minh chứng cần chụp

![Ảnh tạo IAM User Admin-Khoa](static/images/1-Workblog/Week-2/image copy 13.png)
![Ảnh tạo IAM User Admin-Khoa](static/images/1-Workblog/Week-2/image copy 14.png)
![Ảnh tạo IAM User Admin-Khoa](static/images/1-Workblog/Week-2/image copy 15.png)

## 5. Kết quả đạt được

Sau khi cấu hình hoàn tất, RDS instance đã chuyển sang trạng thái **Available**. Tôi đã lấy được endpoint kết nối và hiểu cách ứng dụng backend có thể sử dụng endpoint này để truy vấn dữ liệu.

Qua bài lab này, tôi nhận ra database là thành phần cần được bảo vệ kỹ. Trong môi trường thực tế, database không nên mở public tùy tiện mà nên được đặt trong private subnet và chỉ cho phép truy cập từ các thành phần backend được cấp quyền.

---

## V. Thách thức gặp phải và cách xử lý

Trong quá trình thực hành các bài lab tuần này, tôi gặp một số vấn đề thực tế liên quan đến cấu hình mạng, quyền truy cập và chi phí. Những vấn đề này giúp tôi hiểu rõ hơn cách AWS vận hành.

### 1. Kiểm soát chi phí khi tạo tài nguyên

Khi tạo VPC hoặc các thành phần mạng, AWS có thể đề xuất thêm NAT Gateway. Sau khi tìm hiểu, tôi nhận ra NAT Gateway có thể phát sinh chi phí theo giờ chạy. Vì mục tiêu của tôi là thực hành trong phạm vi Free Tier hoặc Sandbox, tôi cần kiểm tra kỹ trước khi tạo tài nguyên.

Cách xử lý của tôi là chỉ tạo những thành phần cần thiết cho bài lab, hạn chế bật các dịch vụ không bắt buộc và xóa tài nguyên sau khi hoàn thành thực hành.

### 2. Lỗi không truy cập được EC2 bằng Public IP

Trong quá trình triển khai EC2, có lúc tôi không thể truy cập website bằng Public IP. Tôi kiểm tra lại từng lớp cấu hình và nhận thấy nguyên nhân có thể đến từ subnet, route table, public IP, security group hoặc Apache chưa chạy.

Tôi xử lý bằng cách:

* Kiểm tra EC2 có nằm trong public subnet hay không.
* Kiểm tra instance đã được gán Public IPv4 chưa.
* Đảm bảo Route Table có route `0.0.0.0/0` trỏ đến Internet Gateway.
* Kiểm tra Security Group đã mở port `80`.
* Kiểm tra Apache đã được cài đặt và khởi động thành công.

Sau khi rà soát lại từng thành phần, website đã truy cập được bình thường.

### 3. Lỗi khi cấu hình RDS

Trong quá trình tạo RDS, tôi cần chú ý kỹ các thông số như mật khẩu, instance type, loại ổ đĩa và dung lượng lưu trữ. Nếu chọn sai cấu hình, database có thể không phù hợp với phạm vi Free Tier hoặc gây phát sinh chi phí.

Tôi xử lý bằng cách chọn cấu hình nhỏ như `db.t3.micro`, dùng storage `gp3` với dung lượng `20 GiB`, đồng thời tránh sử dụng các ký tự dễ gây lỗi trong mật khẩu.

### 4. Phân biệt Security Group và Network ACL

Khi học VPC, tôi nhận ra Security Group và Network ACL đều liên quan đến bảo mật mạng nhưng hoạt động ở hai lớp khác nhau.

* Security Group hoạt động ở cấp instance và có tính stateful.
* Network ACL hoạt động ở cấp subnet và có tính stateless.

Điều này giúp tôi hiểu rằng khi gặp lỗi kết nối mạng, cần kiểm tra đúng lớp bảo mật thay vì chỉ xem một cấu hình duy nhất.

## VI. Kết quả đạt được trong tuần 2

Sau khi hoàn thành tuần 2, tôi đã đạt được các kết quả sau:

* Hiểu được cấu trúc AWS Global Infrastructure.
* Phân biệt được Region, Availability Zone và Edge Location.
* Hiểu vai trò của IAM trong quản lý định danh và quyền truy cập.
* Tạo được IAM Admin User `Admin-Khoa`.
* Bật MFA để tăng cường bảo mật tài khoản.
* Hiểu nguyên tắc Least Privilege khi phân quyền.
* Tạo được Amazon VPC với CIDR block riêng.
* Cấu hình Public Subnet, Internet Gateway và Route Table.
* Triển khai thành công EC2 instance trong VPC.
* Cấu hình Security Group cho EC2.
* Sử dụng User Data để tự động cài đặt Apache Web Server.
* Truy cập thành công website chạy trên EC2 thông qua Public IP.
* Tạo và cấu hình S3 Bucket `khoa-static-website-2026`.
* Bật Static Website Hosting cho S3.
* Cấu hình Bucket Policy cho phép đọc file công khai.
* Upload file `index.html` và kiểm tra website endpoint.
* Tạo Amazon RDS MySQL instance `my-khoa-db`.
* Hiểu cách lấy và sử dụng endpoint của database.
* Nhận biết được các rủi ro về chi phí khi tạo tài nguyên AWS.
* Ghi chú lại lỗi gặp phải và cách xử lý để bổ sung vào portfolio thực tập.

## VII. Suy ngẫm sau tuần 2

Tuần 2 giúp tôi nhận ra rằng việc xây dựng hạ tầng cloud không chỉ là tạo từng dịch vụ riêng lẻ. Các thành phần trên AWS luôn có sự liên kết chặt chẽ với nhau. Một EC2 instance muốn truy cập được từ Internet cần có VPC, subnet, Internet Gateway, Route Table, Public IP và Security Group phù hợp. Một website tĩnh trên S3 muốn hoạt động cần có Static Website Hosting, Bucket Policy và quyền public access chính xác. Một database RDS muốn sử dụng an toàn cần được cấu hình network và security group cẩn thận.

Điều tôi học được nhiều nhất là cloud engineering yêu cầu tư duy hệ thống. Người thực hành cần hiểu đường đi của dữ liệu, cách phân quyền, cách thiết kế mạng, cách kiểm soát chi phí và cách xử lý lỗi theo từng lớp hạ tầng.

Sau tuần này, tôi cảm thấy tự tin hơn với các dịch vụ nền tảng như IAM, VPC, EC2, S3 và RDS. Đây là bước chuẩn bị quan trọng để tôi tiếp tục học các nội dung nâng cao hơn như Auto Scaling, Load Balancer, CloudWatch và triển khai ứng dụng hoàn chỉnh trong các tuần tiếp theo.

## VIII. Kế hoạch cho tuần tiếp theo

Trong tuần tiếp theo, tôi sẽ tiếp tục mở rộng kiến thức sang các nội dung nâng cao hơn liên quan đến khả năng mở rộng, cân bằng tải và giám sát hệ thống.

Các nội dung dự kiến gồm:

* Tìm hiểu Elastic Load Balancing.
* Thực hành tạo Target Group và Application Load Balancer.
* Cấu hình Auto Scaling Group cho EC2.
* Tìm hiểu CloudWatch để giám sát metric và log.
* Triển khai một ứng dụng đơn giản trên EC2.
* Kết nối ứng dụng với Amazon RDS.
* Kiểm tra cách tối ưu chi phí khi chạy nhiều tài nguyên cùng lúc.
* Tiếp tục cập nhật portfolio Hugo và bổ sung ảnh minh chứng thực hành.

