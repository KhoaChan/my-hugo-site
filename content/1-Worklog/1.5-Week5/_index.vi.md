---
title: "Worklog Tuần 5"
date: 2026-05-15
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---



## I. Tóm tắt tổng quan

Tuần 5 là giai đoạn tôi tập trung nhiều hơn vào các nội dung liên quan đến **quản trị danh tính, bảo mật dữ liệu và lưu trữ cấp doanh nghiệp** trên AWS. Nếu các tuần trước chủ yếu xoay quanh hạ tầng mạng, máy chủ, cân bằng tải và triển khai ứng dụng, thì tuần này tôi đi sâu hơn vào cách kiểm soát quyền truy cập, giới hạn phạm vi thao tác của người dùng và bảo vệ dữ liệu bằng mã hóa.

Các bài lab trong tuần bao gồm **Amazon FSx for Windows File Server**, **Amazon S3 Static Website Hosting**, **IAM Resource Tags**, **IAM Permission Boundary**, **AWS KMS**, **IAM Conditions** và **IAM Role for EC2 Applications**. Đây là các nội dung quan trọng trong việc xây dựng hệ thống cloud an toàn, dễ quản trị và phù hợp với các yêu cầu bảo mật thực tế trong doanh nghiệp.

Thông qua tuần học này, tôi hiểu rõ hơn rằng bảo mật trên AWS không chỉ dừng lại ở việc tạo user hoặc gán quyền cơ bản. Một hệ thống cloud chuyên nghiệp cần áp dụng nguyên tắc **Least Privilege**, phân quyền theo ngữ cảnh, sử dụng IAM Role thay cho Access Key, mã hóa dữ liệu nhạy cảm và quản lý tài nguyên bằng thẻ để dễ kiểm soát.

---

## II. Mục tiêu trong tuần

Trong tuần 5, tôi đặt ra các mục tiêu chính sau:

* Tìm hiểu Amazon FSx for Windows File Server và vai trò của file storage trong môi trường doanh nghiệp.
* Hiểu cách FSx tích hợp với AWS Managed Microsoft Active Directory.
* Cấu hình S3 Static Website Hosting và kiểm soát quyền public access.
* Tìm hiểu cách quản trị quyền truy cập EC2 dựa trên Resource Tags.
* Áp dụng IAM Policy Condition để giới hạn quyền theo thẻ tài nguyên.
* Tìm hiểu và cấu hình IAM Permission Boundary.
* Hiểu cách Permission Boundary giới hạn quyền tối đa của IAM User hoặc IAM Role.
* Tạo và sử dụng AWS KMS Customer Managed Key để mã hóa dữ liệu.
* Phân biệt quyền quản trị khóa và quyền sử dụng khóa trong KMS.
* Tìm hiểu IAM Conditions theo IP, thời gian hoặc ngữ cảnh truy cập.
* Thay thế Access Keys bằng IAM Role cho ứng dụng chạy trên EC2.
* Rèn luyện tư duy bảo mật dữ liệu và kiểm soát quyền hạn trong môi trường cloud.

---

## III. Nhật ký hoạt động trong tuần

| Thời gian | Danh mục hoạt động      | Nội dung công việc thực hiện                                                                                         | Kết quả/Minh chứng đạt được                                                    |
| --------- | ----------------------- | -------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| Ngày 1    | Lưu trữ doanh nghiệp    | Thực hành Amazon FSx for Windows File Server, cấu hình dung lượng lưu trữ, VPC và yêu cầu tích hợp Active Directory. | Hiểu cách triển khai hệ thống file storage cho môi trường Windows doanh nghiệp |
| Ngày 2    | Bảo mật và hosting S3   | Cấu hình S3 bucket, bật Static Website Hosting, thiết lập Bucket Policy và kiểm soát public access.                  | Website tĩnh có thể hoạt động thông qua S3 endpoint                            |
| Ngày 3    | IAM và Resource Tags    | Tạo IAM Policy sử dụng điều kiện dựa trên Resource Tags để kiểm soát quyền thao tác EC2.                             | Quyền truy cập EC2 được giới hạn theo tag của tài nguyên                       |
| Ngày 4    | Permission Boundary     | Tạo IAM Permission Boundary để giới hạn phạm vi quyền tối đa của user hoặc role.                                     | Ngăn chặn user vượt quá phạm vi quyền được cho phép                            |
| Ngày 5    | Mã hóa dữ liệu với KMS  | Tạo Customer Managed Key, cấu hình quyền quản trị và quyền sử dụng khóa, thực hành mã hóa dữ liệu.                   | Hiểu cách mã hóa dữ liệu bằng AWS KMS                                          |
| Ngày 6    | IAM Conditions nâng cao | Tìm hiểu cách giới hạn quyền truy cập theo IP, thời gian hoặc điều kiện cụ thể trong IAM Policy.                     | Nắm được cách tăng cường bảo mật bằng điều kiện truy cập                       |
| Ngày 7    | IAM Role cho EC2        | Loại bỏ cách dùng Access Key trực tiếp, tìm hiểu và áp dụng IAM Role cho ứng dụng chạy trên EC2.                     | Ứng dụng EC2 có thể truy cập dịch vụ AWS an toàn hơn thông qua IAM Role        |

---

# IV. Thực hành kỹ thuật chi tiết qua các bài Lab

---

# LAB 25: Amazon FSx for Windows File Server

## 1. Tổng quan

Amazon FSx for Windows File Server là dịch vụ lưu trữ tệp được quản lý hoàn toàn bởi AWS, hỗ trợ giao thức SMB và tương thích tốt với môi trường Windows Server. Dịch vụ này phù hợp cho các doanh nghiệp đang sử dụng hệ sinh thái Windows, Active Directory và các ứng dụng cần chia sẻ file theo mô hình truyền thống.

Trong bài lab này, tôi tìm hiểu cách cấu hình Amazon FSx, lựa chọn dung lượng lưu trữ, kiểm tra yêu cầu về mạng và nhận biết vai trò của AWS Managed Microsoft Active Directory trong việc xác thực người dùng và quản lý quyền truy cập file.

## 2. Kiến thức đã tìm hiểu

### Amazon FSx for Windows File Server

Amazon FSx cung cấp hệ thống file storage được quản lý, giúp người dùng không cần tự vận hành máy chủ file truyền thống. Dịch vụ hỗ trợ SMB, NTFS permissions, tích hợp Active Directory và có khả năng sao lưu dữ liệu tự động.

### SMB Protocol

SMB là giao thức chia sẻ file phổ biến trong môi trường Windows. Nhờ hỗ trợ SMB, FSx có thể được sử dụng bởi các máy Windows hoặc ứng dụng Windows mà không cần thay đổi nhiều cấu hình.

### AWS Managed Microsoft Active Directory

FSx for Windows yêu cầu tích hợp với Active Directory để xác thực người dùng và phân quyền truy cập file. Đây là điểm khác biệt quan trọng so với một số dịch vụ lưu trữ khác như Amazon S3 hoặc Amazon EFS.

### Backup và Maintenance

FSx hỗ trợ sao lưu tự động và cấu hình thời gian bảo trì định kỳ. Đây là các yếu tố quan trọng để đảm bảo dữ liệu được bảo vệ và hệ thống hoạt động ổn định trong thời gian dài.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập dịch vụ Amazon FSx.
* Chọn tạo file system mới.
* Chọn loại file system là Windows File Server.
* Cấu hình dung lượng lưu trữ, ví dụ `32 GiB`.
* Chọn loại lưu trữ SSD để đảm bảo hiệu năng đọc/ghi tốt hơn.
* Chọn VPC và subnet phù hợp.
* Kiểm tra yêu cầu tích hợp AWS Managed Microsoft Active Directory.
* Cấu hình backup tự động hằng ngày.
* Kiểm tra thời gian bảo trì định kỳ.
* Rà soát thông tin cấu hình trước khi tạo file system.
* Ghi chú các yêu cầu về chi phí và tài nguyên phụ thuộc.

## 4. Minh chứng thực hành

![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-5/image.png)

## 5. Kết quả đạt được

Sau bài lab này, tôi hiểu được cách Amazon FSx hỗ trợ nhu cầu lưu trữ file trong môi trường doanh nghiệp sử dụng Windows. Tôi cũng nhận ra rằng FSx phù hợp cho các hệ thống cần SMB, NTFS và Active Directory, nhưng cần cân nhắc chi phí và tài nguyên phụ thuộc trước khi triển khai.

---

# LAB 57: Starting with Amazon S3

## 1. Tổng quan

Amazon S3 là dịch vụ lưu trữ đối tượng có khả năng mở rộng cao, độ bền dữ liệu lớn và được sử dụng rộng rãi trong nhiều hệ thống AWS. Ngoài mục đích lưu trữ file, S3 còn có thể được dùng để triển khai website tĩnh thông qua tính năng Static Website Hosting.

Trong bài lab này, tôi thực hành tạo S3 bucket, cấu hình public access có kiểm soát, bật Static Website Hosting và sử dụng Bucket Policy để cho phép người dùng truy cập nội dung website.

## 2. Kiến thức đã tìm hiểu

### Object Storage

S3 lưu trữ dữ liệu dưới dạng object. Mỗi object bao gồm dữ liệu, metadata và key định danh. Cách lưu trữ này phù hợp với file tĩnh, hình ảnh, log, backup và dữ liệu không cần chỉnh sửa trực tiếp như file system truyền thống.

### Static Website Hosting

Static Website Hosting cho phép S3 bucket phục vụ các file HTML, CSS và JavaScript như một website tĩnh. Cách này không cần quản lý máy chủ, phù hợp với landing page, portfolio hoặc website tài liệu.

### Bucket Policy

Bucket Policy được dùng để kiểm soát quyền truy cập ở cấp bucket. Khi hosting website tĩnh, cần cho phép hành động `s3:GetObject` đối với các object cần public.

### Public Access Block

Mặc định, S3 chặn truy cập công khai để bảo vệ dữ liệu. Khi cần public website, cần tắt hoặc điều chỉnh Block Public Access một cách cẩn thận để tránh làm lộ dữ liệu không mong muốn.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập dịch vụ Amazon S3.
* Tạo một S3 bucket mới.
* Đặt tên bucket phù hợp và không trùng toàn hệ thống.
* Chọn region phù hợp.
* Cấu hình Block Public Access theo mục tiêu lab.
* Bật tính năng Static Website Hosting.
* Khai báo file `index.html` làm trang chủ.
* Tạo hoặc upload file `index.html`.
* Cấu hình Bucket Policy cho phép public read.
* Kiểm tra S3 website endpoint.
* Truy cập endpoint bằng trình duyệt để xác nhận website hoạt động.

## 4. Minh chứng thực hành

![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-5/image%20copy.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-5/image%20copy%202.png)

## 5. Kết quả đạt được

Sau bài lab này, tôi đã hiểu cách sử dụng Amazon S3 để triển khai website tĩnh. Tôi cũng nhận thấy rằng khi public bucket, cần kiểm tra kỹ Bucket Policy và chỉ public đúng những file cần thiết để đảm bảo an toàn dữ liệu.

---

# LAB 28: Quản trị truy cập EC2 thông qua Resource Tags bằng IAM

## 1. Tổng quan

Trong môi trường AWS có nhiều tài nguyên, việc phân quyền dựa trên từng resource riêng lẻ có thể trở nên khó quản lý. Resource Tags giúp phân loại tài nguyên theo phòng ban, dự án, môi trường hoặc mục đích sử dụng. Khi kết hợp tags với IAM Policy Conditions, người quản trị có thể kiểm soát quyền truy cập linh hoạt hơn.

Trong bài lab này, tôi thực hành tạo IAM Policy cho phép thao tác với EC2 dựa trên tag của tài nguyên. Ví dụ, chỉ những EC2 instance có tag `Department: Finance` mới được phép start hoặc stop.

## 2. Kiến thức đã tìm hiểu

### Resource Tags

Resource Tags là các cặp key-value được gắn vào tài nguyên AWS. Tags giúp quản lý, tìm kiếm, phân loại và kiểm soát chi phí hoặc quyền truy cập.

### IAM Policy Condition

Condition trong IAM Policy cho phép giới hạn quyền theo điều kiện cụ thể. Với EC2, có thể dùng `aws:ResourceTag` để chỉ cho phép thao tác trên tài nguyên có tag phù hợp.

### Department-level Isolation

Phân quyền theo tag giúp cô lập tài nguyên theo phòng ban hoặc nhóm dự án. Ví dụ, user thuộc phòng Finance chỉ có thể thao tác với tài nguyên có tag Finance.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập dịch vụ IAM.
* Tạo một IAM Policy mới.
* Viết policy JSON sử dụng điều kiện `aws:ResourceTag/Department`.
* Cấu hình quyền cho các hành động EC2 như StartInstances và StopInstances.
* Tạo hoặc chọn IAM User/Role phù hợp.
* Gán policy vừa tạo cho User hoặc Role.
* Gắn tag `Department: Finance` cho EC2 instance cần quản lý.
* Kiểm tra quyền thao tác với EC2 có tag phù hợp.
* Kiểm tra trường hợp EC2 không có tag hoặc tag không khớp.

## 4. Minh chứng thực hành

![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-5/image%20copy%203.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-5/image%20copy%204.png)

## 5. Kết quả đạt được

Sau bài lab này, tôi hiểu được cách sử dụng Resource Tags kết hợp với IAM Policy để kiểm soát quyền truy cập tài nguyên. Cách làm này giúp phân quyền linh hoạt, dễ quản trị và phù hợp với môi trường có nhiều nhóm hoặc phòng ban khác nhau.

---

# LAB 30: Giới hạn quyền hạn người dùng với IAM Permission Boundary

## 1. Tổng quan

IAM Permission Boundary là một tính năng bảo mật nâng cao cho phép xác định giới hạn quyền tối đa mà một IAM User hoặc IAM Role có thể có. Ngay cả khi entity đó được gán thêm policy có quyền rộng hơn, quyền thực tế vẫn không thể vượt quá phạm vi mà Permission Boundary cho phép.

Trong bài lab này, tôi tìm hiểu cách tạo một policy đóng vai trò Permission Boundary, gán boundary cho IAM User và kiểm tra cách boundary ngăn người dùng thực hiện các hành động vượt phạm vi.

## 2. Kiến thức đã tìm hiểu

### Permission Boundary

Permission Boundary không trực tiếp cấp quyền, mà chỉ xác định giới hạn quyền tối đa. Một user chỉ có thể thực hiện hành động nếu hành động đó vừa được policy cho phép, vừa nằm trong phạm vi boundary cho phép.

### Least Privilege

Permission Boundary hỗ trợ thực thi nguyên tắc Least Privilege ở cấp độ cao hơn. Đây là công cụ hữu ích trong môi trường doanh nghiệp, nơi nhiều nhóm có thể được phép tạo role hoặc user nhưng vẫn cần bị giới hạn phạm vi.

### Privilege Escalation Prevention

Nếu không có boundary, user có quyền tạo policy hoặc role có thể tự cấp thêm quyền cao hơn. Permission Boundary giúp giảm rủi ro leo thang đặc quyền ngoài ý muốn.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập dịch vụ IAM.
* Tạo một policy mới dùng làm Permission Boundary.
* Xác định phạm vi dịch vụ tối đa mà user được phép thao tác.
* Tạo hoặc chọn IAM User cần giới hạn quyền.
* Gán Permission Boundary cho user đó.
* Gán thêm policy thao tác cho user để kiểm tra.
* Thử thực hiện hành động nằm trong phạm vi boundary.
* Thử thực hiện hành động vượt quá phạm vi boundary.
* Xác nhận rằng boundary đã chặn quyền vượt mức.

## 4. Minh chứng thực hành

![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-5/image%20copy%205.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-5/image%20copy%206.png)

## 5. Kết quả đạt được

Sau bài lab này, tôi hiểu rõ hơn về vai trò của IAM Permission Boundary trong việc giới hạn quyền tối đa. Đây là một cơ chế quan trọng giúp kiểm soát quyền hạn trong các hệ thống lớn và giảm nguy cơ user hoặc role được cấp quyền vượt quá phạm vi mong muốn.

---

# LAB 33: AWS Key Management Service - KMS

## 1. Tổng quan

AWS Key Management Service là dịch vụ giúp tạo, quản lý và sử dụng khóa mã hóa trên AWS. KMS được dùng để bảo vệ dữ liệu nhạy cảm, đặc biệt khi dữ liệu được lưu trữ trong các dịch vụ như S3, EBS, RDS hoặc các ứng dụng tùy chỉnh.

Trong bài lab này, tôi thực hành tạo một Customer Managed Key, cấu hình quyền quản trị khóa và quyền sử dụng khóa, sau đó tìm hiểu cách dùng khóa KMS để mã hóa dữ liệu.

## 2. Kiến thức đã tìm hiểu

### AWS KMS

AWS KMS là dịch vụ quản lý khóa mã hóa tập trung. Người dùng có thể tạo khóa, cấu hình quyền sử dụng khóa, bật hoặc tắt khóa, xoay vòng khóa và theo dõi hoạt động sử dụng khóa thông qua CloudTrail.

### Customer Managed Key

Customer Managed Key là khóa do người dùng tạo và quản lý. Loại khóa này cho phép kiểm soát chi tiết hơn về policy, quyền quản trị và quyền sử dụng so với AWS managed keys.

### Key Administrators và Key Users

Trong KMS, cần phân biệt người có quyền quản trị khóa và người có quyền sử dụng khóa. Key Administrators có thể quản lý cấu hình khóa, còn Key Users chỉ được dùng khóa để encrypt hoặc decrypt dữ liệu.

### Encryption at Rest

Encryption at Rest là mã hóa dữ liệu khi dữ liệu được lưu trữ. Đây là một yêu cầu bảo mật quan trọng đối với dữ liệu nhạy cảm trong môi trường cloud.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập dịch vụ AWS KMS.
* Chọn tạo khóa mới.
* Chọn loại khóa đối xứng Symmetric Key.
* Đặt tên khóa, ví dụ `Lab33-My-KMS-Key`.
* Cấu hình Key Administrators.
* Cấu hình Key Users.
* Xem lại Key Policy.
* Tạo khóa KMS.
* Sử dụng khóa để mã hóa dữ liệu mẫu.
* Kiểm tra kết quả dữ liệu sau khi được mã hóa.
* Ghi chú cách CloudTrail có thể dùng để truy vết hoạt động liên quan đến khóa.

## 4. Minh chứng thực hành

![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-5/image%20copy%207.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-5/image%20copy%208.png)

## 5. Kết quả đạt được

Sau bài lab này, tôi đã hiểu cách AWS KMS hỗ trợ mã hóa dữ liệu và quản lý khóa tập trung. Tôi cũng nhận thấy rằng phân quyền rõ ràng giữa người quản trị khóa và người sử dụng khóa là rất quan trọng để đảm bảo an toàn dữ liệu.

---

# LAB 44: IAM Role với điều kiện IP và thời gian

## 1. Tổng quan

Trong các hệ thống cần bảo mật cao, việc cấp quyền đơn thuần cho user hoặc role là chưa đủ. Quyền truy cập nên được giới hạn thêm theo điều kiện cụ thể như địa chỉ IP, thời gian truy cập hoặc ngữ cảnh sử dụng.

Trong bài lab này, tôi tìm hiểu cách sử dụng IAM Policy Conditions để giới hạn quyền truy cập dựa trên IP hoặc thời gian. Đây là cách giúp giảm rủi ro nếu thông tin đăng nhập bị sử dụng ngoài môi trường được phép.

## 2. Kiến thức đã tìm hiểu

### IAM Conditions

IAM Conditions cho phép bổ sung điều kiện vào policy. Một hành động chỉ được phép nếu request đáp ứng đầy đủ các điều kiện đã định nghĩa.

### IP-based Access Control

Có thể dùng condition theo IP để chỉ cho phép truy cập từ một dải địa chỉ đáng tin cậy, ví dụ IP công ty hoặc IP cá nhân.

### Time-based Access Control

Có thể giới hạn quyền truy cập trong một khoảng thời gian cụ thể. Điều này phù hợp với các tác vụ tạm thời hoặc các quyền chỉ được sử dụng trong giờ làm việc.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập dịch vụ IAM.
* Tạo hoặc chỉnh sửa một IAM Policy.
* Thêm điều kiện giới hạn theo địa chỉ IP nếu cần.
* Thêm điều kiện giới hạn theo thời gian nếu cần.
* Gán policy cho user hoặc role.
* Kiểm tra quyền truy cập khi đáp ứng điều kiện.
* Kiểm tra quyền truy cập khi không đáp ứng điều kiện.
* Ghi chú kết quả để hiểu cách policy condition hoạt động.

## 4. Minh chứng thực hành

## 5. Kết quả đạt được

Sau bài lab này, tôi hiểu cách IAM Conditions giúp tăng cường bảo mật truy cập. Việc giới hạn theo IP hoặc thời gian giúp giảm nguy cơ tài khoản bị sử dụng sai mục đích hoặc truy cập ngoài phạm vi kiểm soát.

---

# LAB 48: Authorization với IAM Role cho EC2

## 1. Tổng quan

Trước đây, nhiều ứng dụng truy cập AWS bằng cách lưu Access Key và Secret Key trực tiếp trong máy chủ hoặc file cấu hình. Cách làm này có rủi ro bảo mật cao vì nếu key bị lộ, kẻ tấn công có thể truy cập tài nguyên AWS.

Trong bài lab này, tôi tìm hiểu cách sử dụng IAM Role cho EC2 để cấp quyền cho ứng dụng chạy trên instance mà không cần lưu Access Keys trực tiếp. Đây là phương pháp an toàn hơn và phù hợp với best practice của AWS.

## 2. Kiến thức đã tìm hiểu

### IAM Role for EC2

IAM Role for EC2 cho phép EC2 instance nhận quyền tạm thời để truy cập các dịch vụ AWS. Ứng dụng chạy trên instance có thể sử dụng quyền này thông qua instance metadata mà không cần lưu key cố định.

### Temporary Credentials

Temporary Credentials có thời hạn sử dụng nhất định và được AWS tự động xoay vòng. Điều này an toàn hơn so với Access Keys dài hạn.

### Removing Access Keys

Việc loại bỏ Access Keys khỏi ứng dụng giúp giảm nguy cơ lộ thông tin xác thực. Đây là một bước quan trọng khi triển khai ứng dụng thực tế trên cloud.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập dịch vụ IAM.
* Tạo một IAM Role mới cho EC2.
* Gán policy phù hợp cho role.
* Gắn IAM Role vào EC2 instance.
* Truy cập EC2 instance để kiểm tra quyền.
* Chạy lệnh AWS CLI từ EC2 để xác nhận role hoạt động.
* Loại bỏ việc sử dụng Access Key trực tiếp trong ứng dụng.
* Kiểm tra ứng dụng có thể truy cập dịch vụ AWS thông qua role.

## 4. Minh chứng thực hành

## 5. Kết quả đạt được

Sau bài lab này, tôi hiểu rằng IAM Role là phương pháp an toàn hơn để cấp quyền cho ứng dụng chạy trên EC2. Thay vì lưu Access Key trực tiếp, ứng dụng có thể sử dụng quyền tạm thời từ role, giúp giảm rủi ro lộ thông tin xác thực.

---

## V. Thách thức gặp phải và cách xử lý

Trong tuần 5, tôi gặp một số vấn đề thực tế liên quan đến dịch vụ lưu trữ doanh nghiệp, phân quyền nâng cao và mã hóa dữ liệu.

### 1. Yêu cầu Active Directory khi triển khai FSx

Khi thực hành Amazon FSx for Windows File Server, tôi nhận thấy dịch vụ này yêu cầu tích hợp với AWS Managed Microsoft Active Directory. Nếu chưa có Directory phù hợp, quá trình tạo file system có thể bị chặn.

Cách xử lý là cần chuẩn bị Active Directory trước khi triển khai FSx hoặc cân nhắc sử dụng dịch vụ lưu trữ khác phù hợp hơn với nhu cầu thực hành.

### 2. Quản lý chi phí với các dịch vụ enterprise

Một số dịch vụ như FSx hoặc Managed Microsoft AD có thể phát sinh chi phí nếu để chạy lâu. Vì vậy, cần kiểm tra kỹ cấu hình và dọn dẹp tài nguyên sau khi hoàn thành bài lab.

Tôi xử lý bằng cách ghi chú các tài nguyên đã tạo, chụp minh chứng ngay khi hoàn thành và xóa tài nguyên không cần thiết để tránh phát sinh chi phí.

### 3. Độ phức tạp của IAM Conditions

Khi viết policy có điều kiện, nếu sai key condition hoặc sai cú pháp JSON, policy có thể không hoạt động như mong muốn. Điều này đòi hỏi phải kiểm tra kỹ cấu trúc policy trước khi áp dụng.

Cách xử lý là kiểm tra từng điều kiện nhỏ, thử nghiệm với tài nguyên mẫu và xác nhận kết quả bằng hành động thực tế.

### 4. Phân quyền KMS cần rõ ràng

Trong KMS, nếu người dùng không có quyền phù hợp, họ có thể không sử dụng được khóa để mã hóa hoặc giải mã. Ngược lại, nếu cấp quyền quá rộng, khóa có thể bị sử dụng sai mục đích.

Tôi rút ra rằng cần tách rõ Key Administrators và Key Users để đảm bảo an toàn trong quá trình quản lý khóa.

---

## VI. Suy ngẫm sau tuần 5

Tuần 5 giúp tôi thay đổi cách nhìn về bảo mật trên AWS. Trước đây, tôi thường nghĩ việc có quyền Administrator là đủ để thực hành nhanh. Tuy nhiên, trong môi trường thực tế, việc cấp quyền rộng như vậy có thể tạo ra rủi ro rất lớn.

Tôi nhận ra rằng các cơ chế như Permission Boundary, IAM Conditions, Resource Tags, IAM Role và KMS đều hướng đến cùng một mục tiêu: kiểm soát quyền truy cập một cách chặt chẽ và bảo vệ dữ liệu quan trọng. Một hệ thống cloud an toàn không chỉ dựa vào mật khẩu mạnh, mà còn phụ thuộc vào cách thiết kế quyền hạn, cách mã hóa dữ liệu và cách loại bỏ thông tin xác thực dài hạn.

Bài học quan trọng nhất trong tuần này là bảo mật phải được thiết kế ngay từ đầu, không phải chỉ bổ sung sau khi hệ thống đã hoàn thành. Việc áp dụng Least Privilege, Separation of Duties và Encryption at Rest giúp hệ thống đáng tin cậy hơn và phù hợp hơn với môi trường doanh nghiệp.

---

## VII. Kế hoạch cho tuần tiếp theo

Trong tuần tiếp theo, tôi sẽ tiếp tục hoàn thiện kiến thức chuyên môn và bắt đầu định hướng rõ hơn cho dự án thực tế.

Các nội dung dự kiến gồm:

* Hoàn thành các bài lab thuộc module tiếp theo.
* Tiếp tục học tập và thực hành tại văn phòng AWS Vietnam.
* Trao đổi thêm với mentor hoặc chuyên gia kỹ thuật nếu gặp vấn đề trong quá trình thực hành.
* Khởi động ý tưởng cho đồ án cuối kỳ.
* Nghiên cứu hệ thống Website Đặt tour du lịch Việt Nam tích hợp Chatbot AI tư vấn thông minh.
* Phân tích kiến trúc hệ thống, luồng dữ liệu và các thành phần chính.
* Tìm hiểu thêm về Microservices và AI-driven applications.
* Áp dụng kiến thức IAM Role, Permission Boundary và KMS vào thiết kế bảo mật cho dự án.
* Cập nhật portfolio Hugo với nội dung kỹ thuật và ảnh minh chứng thực hành.

