---
title: "Worklog Tuần 4"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.4. </b> "
---



## I. Tóm tắt tổng quan

Tuần 4 là giai đoạn tôi tiếp tục mở rộng kiến thức từ hạ tầng mạng và vận hành cơ bản sang các nội dung nâng cao hơn liên quan đến bảo mật, kết nối mạng đa VPC, điều phối định tuyến tập trung và tự động hóa triển khai ứng dụng trên AWS.

Trong tuần này, tôi tập trung vào các bài lab quan trọng gồm **AWS Security Hub**, **VPC Peering**, **AWS Transit Gateway**, **AWS CodePipeline** và **AWS WAF**. Đây là các dịch vụ thường xuất hiện trong các hệ thống cloud có quy mô lớn hơn, nơi yêu cầu cao về bảo mật, khả năng quản trị, kết nối giữa nhiều mạng riêng và quy trình triển khai phần mềm tự động.

Thông qua các bài lab, tôi hiểu rõ hơn rằng một hệ thống AWS thực tế không chỉ cần máy chủ, cơ sở dữ liệu hay mạng VPC riêng lẻ. Hệ thống còn cần có cơ chế kiểm toán bảo mật tập trung, mô hình kết nối mạng phù hợp, hệ thống phòng vệ lớp ứng dụng và quy trình CI/CD để triển khai mã nguồn một cách nhanh chóng, có kiểm soát và giảm sai sót thủ công.

---

## II. Mục tiêu trong tuần

Trong tuần 4, tôi đặt ra các mục tiêu chính sau:

* Kích hoạt và tìm hiểu AWS Security Hub để quản lý tư thế bảo mật của tài khoản AWS.
* Phân tích các security findings và hiểu cách Security Hub đánh giá mức độ tuân thủ.
* Tạo kết nối VPC Peering giữa hai VPC có dải CIDR không trùng lặp.
* Cập nhật Route Table hai chiều để các VPC có thể giao tiếp nội bộ với nhau.
* Tìm hiểu AWS Transit Gateway và mô hình Hub-and-Spoke trong kiến trúc mạng nhiều VPC.
* Đấu nối các VPC vào Transit Gateway thông qua Transit Gateway Attachments.
* Hiểu cách cập nhật route table để định tuyến traffic qua Transit Gateway.
* Tìm hiểu quy trình CI/CD native trên AWS bằng CodeCommit, CodeBuild, CodeDeploy và CodePipeline.
* Cấu hình pipeline tự động triển khai ứng dụng lên EC2.
* Tìm hiểu AWS WAF và cách bảo vệ ứng dụng web trước các kiểu tấn công phổ biến.
* Rèn luyện tư duy kiểm soát chi phí, đặc biệt với các dịch vụ mạng nâng cao có thể phát sinh phí duy trì.

---

## III. Nhật ký hoạt động trong tuần

| Thời gian  | Danh mục hoạt động          | Nội dung công việc thực hiện                                                                        | Kết quả/Minh chứng đạt được                                           |
| ---------- | --------------------------- | --------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------- |
| Ngày 1     | An ninh tập trung           | Kích hoạt AWS Security Hub, bật các tiêu chuẩn bảo mật và kiểm tra dashboard tổng quan.             | Thiết lập được hệ thống kiểm toán bảo mật tập trung cho tài khoản AWS |
| Ngày 2     | Kết nối VPC Peering         | Tạo VPC Peering Connection giữa hai VPC có CIDR không trùng lặp và cập nhật Route Table hai chiều.  | Các VPC có thể giao tiếp nội bộ thông qua kết nối Peering             |
| Ngày 3 - 4 | Transit Gateway             | Tạo AWS Transit Gateway, cấu hình Transit Gateway Attachments và cập nhật route table của các VPC.  | Hoàn thành mô hình định tuyến tập trung Hub-and-Spoke                 |
| Ngày 5     | Bảo vệ ứng dụng với AWS WAF | Tạo Web ACL, cấu hình rules bảo vệ ứng dụng và liên kết WAF với Application Load Balancer.          | Ứng dụng web được tăng cường bảo vệ ở lớp biên                        |
| Ngày 6     | CI/CD với AWS CodePipeline  | Thiết lập quy trình triển khai tự động bằng CodeCommit, CodeBuild, CodeDeploy và CodePipeline.      | Ứng dụng có thể tự động triển khai khi có thay đổi mã nguồn           |
| Ngày 7     | Tổng hợp và tài liệu hóa    | Ghi chú quy trình thực hành, tổng hợp lỗi, kiểm tra tài nguyên và chuẩn bị nội dung portfolio Hugo. | Hoàn thiện Worklog Tuần 4 và chuẩn bị hình minh chứng                 |

---

# LAB 18: AWS Security Hub - Quản trị bảo mật tập trung

## 1. Tổng quan

Khi số lượng tài nguyên AWS tăng lên, việc kiểm tra thủ công từng cấu hình bảo mật là rất khó và dễ bỏ sót. Vì vậy, cần có một công cụ tập trung để thu thập, tổng hợp và đánh giá các vấn đề bảo mật trên toàn bộ tài khoản.

Trong bài lab này, tôi thực hành kích hoạt **AWS Security Hub** để theo dõi các phát hiện bảo mật, kiểm tra mức độ tuân thủ và quan sát dashboard tổng quan. Dịch vụ này giúp tôi hiểu cách AWS hỗ trợ quản lý tư thế bảo mật đám mây theo hướng tập trung và tự động hơn.

## 2. Kiến thức đã tìm hiểu

### AWS Security Hub

AWS Security Hub là dịch vụ giúp tổng hợp các phát hiện bảo mật từ nhiều nguồn khác nhau trong AWS. Dịch vụ này cung cấp một dashboard tập trung để người dùng theo dõi trạng thái bảo mật, mức độ nghiêm trọng của findings và điểm tuân thủ của tài khoản.

### Security Findings

Security findings là các phát hiện liên quan đến cấu hình hoặc hành vi có thể gây rủi ro bảo mật. Mỗi finding thường có mức độ nghiêm trọng như Low, Medium, High hoặc Critical.

### Security Standards

Security Hub hỗ trợ các bộ tiêu chuẩn bảo mật như AWS Foundational Security Best Practices và CIS AWS Foundations Benchmark. Các tiêu chuẩn này giúp hệ thống tự động kiểm tra tài nguyên theo các quy tắc bảo mật đã được định nghĩa.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập dịch vụ AWS Security Hub.
* Kích hoạt Security Hub trong tài khoản AWS.
* Cho phép hệ thống tạo các service-linked roles cần thiết.
* Bật các bộ tiêu chuẩn bảo mật như AWS Foundational Security Best Practices.
* Bật thêm CIS AWS Foundations Benchmark nếu phù hợp với môi trường lab.
* Chờ Security Hub thực hiện quá trình kiểm tra tài nguyên.
* Truy cập dashboard tổng quan.
* Quan sát các findings theo mức độ nghiêm trọng.
* Kiểm tra điểm tuân thủ và trạng thái các rule.
* Ghi chú lại các findings cần khắc phục hoặc cần theo dõi thêm.

## 4. Minh chứng thực hành

![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-4/image.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-4/image%20copy.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-4/image%20copy%202.png)

## 5. Kết quả đạt được

Sau khi hoàn thành bài lab, tôi đã kích hoạt được AWS Security Hub và hiểu cách dịch vụ này hỗ trợ kiểm toán bảo mật tập trung. Tôi cũng nhận thấy rằng việc theo dõi findings thường xuyên giúp phát hiện sớm các cấu hình chưa an toàn, từ đó cải thiện mức độ bảo mật tổng thể của tài khoản AWS.

---

# LAB 19: VPC Peering - Kết nối hai mạng VPC riêng biệt

## 1. Tổng quan

Theo mặc định, các VPC trong AWS được cô lập với nhau. Điều này giúp tăng tính bảo mật, nhưng trong nhiều trường hợp, các tài nguyên ở hai VPC khác nhau cần giao tiếp nội bộ với nhau. Khi đó, VPC Peering là một giải pháp phù hợp để kết nối trực tiếp hai VPC mà không cần đi qua Internet công cộng.

Trong bài lab này, tôi thực hành tạo **VPC Peering Connection**, chấp nhận yêu cầu kết nối và cập nhật Route Table ở cả hai phía để các tài nguyên có thể giao tiếp với nhau thông qua mạng nội bộ AWS.

## 2. Kiến thức đã tìm hiểu

### VPC Peering Connection

VPC Peering Connection là kết nối mạng logic một-đối-một giữa hai VPC. Kết nối này cho phép tài nguyên trong hai VPC giao tiếp với nhau bằng địa chỉ IP private.

### Non-overlapping CIDR

Điều kiện quan trọng khi tạo VPC Peering là hai VPC không được có dải CIDR trùng lặp. Nếu dải IP bị trùng, hệ thống sẽ không thể định tuyến chính xác traffic giữa hai mạng.

### Route Table Update

Sau khi tạo VPC Peering, cần cập nhật Route Table của cả hai VPC. Mỗi bên cần có route trỏ đến CIDR của VPC còn lại, với target là Peering Connection.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập dịch vụ VPC.
* Vào mục Peering connections.
* Chọn Create peering connection.
* Chọn VPC đóng vai trò requester.
* Chọn VPC đóng vai trò accepter.
* Kiểm tra hai VPC có CIDR không trùng lặp.
* Tạo yêu cầu kết nối Peering.
* Chuyển sang phía accepter và chọn Accept request.
* Kiểm tra trạng thái kết nối chuyển sang Active.
* Vào Route Table của VPC thứ nhất.
* Thêm route đến CIDR của VPC thứ hai, target là Peering Connection.
* Vào Route Table của VPC thứ hai.
* Thêm route đến CIDR của VPC thứ nhất, target là Peering Connection.
* Kiểm tra lại cấu hình định tuyến hai chiều.

## 4. Minh chứng thực hành

![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-4/image%20copy%203.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-4/image%20copy%204.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-4/image%20copy%205.png)

## 5. Kết quả đạt được

Sau khi hoàn thành bài lab, tôi đã thiết lập được kết nối VPC Peering giữa hai VPC. Tôi hiểu rằng VPC Peering phù hợp cho mô hình kết nối đơn giản giữa một số lượng nhỏ VPC, nhưng nếu số lượng VPC tăng lên nhiều thì việc quản lý kết nối sẽ trở nên phức tạp hơn.

---

# LAB 20: AWS Transit Gateway - Xây dựng mô hình định tuyến trung tâm

## 1. Tổng quan

Khi doanh nghiệp có nhiều VPC, việc dùng VPC Peering để kết nối từng cặp VPC có thể tạo ra mô hình mạng phức tạp và khó quản lý. AWS Transit Gateway giúp giải quyết vấn đề này bằng cách đóng vai trò là một bộ định tuyến trung tâm, cho phép nhiều VPC kết nối về một điểm chung.

Trong bài lab này, tôi thực hành tạo **AWS Transit Gateway**, tạo Transit Gateway Attachments và cập nhật Route Table của các VPC để traffic có thể đi qua Transit Gateway.

## 2. Kiến thức đã tìm hiểu

### AWS Transit Gateway

AWS Transit Gateway là dịch vụ định tuyến trung tâm giúp kết nối nhiều VPC, VPN hoặc Direct Connect Gateway. Dịch vụ này phù hợp với mô hình mạng Hub-and-Spoke, trong đó Transit Gateway đóng vai trò Hub và các VPC đóng vai trò Spoke.

### Transit Gateway Attachment

Transit Gateway Attachment là liên kết logic giữa Transit Gateway và tài nguyên mạng như VPC. Khi một VPC được attach vào Transit Gateway, nó có thể gửi traffic đến các mạng khác thông qua Hub trung tâm này.

### Hub-and-Spoke Architecture

Mô hình Hub-and-Spoke giúp đơn giản hóa việc quản lý mạng. Thay vì kết nối từng VPC với nhau, tất cả VPC đều kết nối về Transit Gateway, từ đó giảm số lượng kết nối cần quản lý.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập VPC Dashboard.
* Vào mục Transit gateways.
* Chọn Create Transit Gateway.
* Đặt tên Transit Gateway là `Hub-TGW`.
* Cấu hình ASN và các thiết lập cơ bản.
* Tạo Transit Gateway.
* Vào mục Transit Gateway Attachments.
* Tạo attachment cho VPC thứ nhất.
* Tạo attachment cho VPC thứ hai.
* Chọn các subnet phù hợp để kết nối với Transit Gateway.
* Kiểm tra trạng thái attachment.
* Cập nhật Route Table của từng VPC.
* Thêm route đến CIDR của VPC còn lại, target là Transit Gateway.
* Kiểm tra lại luồng định tuyến qua Transit Gateway.

## 4. Minh chứng thực hành

![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-4/image%20copy%206.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-4/image%20copy%207.png)
![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-4/image%20copy%208.png)

## 5. Kết quả đạt được

Sau bài lab này, tôi hiểu được cách AWS Transit Gateway giúp đơn giản hóa mô hình kết nối nhiều VPC. Tôi cũng nhận ra rằng Transit Gateway phù hợp hơn VPC Peering khi hệ thống có nhiều mạng riêng cần kết nối và quản lý tập trung.

---

# LAB 23: AWS CodePipeline - Triển khai ứng dụng lên EC2 bằng CI/CD

## 1. Tổng quan

Trong quá trình phát triển phần mềm, việc triển khai thủ công lên máy chủ dễ gây sai sót và mất thời gian. CI/CD giúp tự động hóa các bước lấy mã nguồn, build, kiểm thử và triển khai ứng dụng.

Trong bài lab này, tôi thực hành xây dựng pipeline triển khai ứng dụng Node.js lên EC2 bằng các dịch vụ native của AWS như **CodeCommit, CodeBuild, CodeDeploy và CodePipeline**. Qua đó, tôi hiểu hơn về cách AWS hỗ trợ tự động hóa quy trình phát hành phần mềm.

## 2. Kiến thức đã tìm hiểu

### AWS CodePipeline

AWS CodePipeline là dịch vụ điều phối quy trình CI/CD. Pipeline có thể bao gồm nhiều stage như Source, Build và Deploy. Khi có thay đổi mã nguồn, pipeline có thể tự động kích hoạt các bước triển khai.

### AWS CodeCommit

CodeCommit là dịch vụ lưu trữ mã nguồn Git do AWS cung cấp. Dịch vụ này có thể đóng vai trò Source Stage trong pipeline.

### AWS CodeBuild

CodeBuild là dịch vụ dùng để build và kiểm thử mã nguồn. Quá trình build thường được định nghĩa trong file `buildspec.yml`.

### AWS CodeDeploy

CodeDeploy giúp triển khai ứng dụng lên EC2 hoặc các môi trường khác. Trên EC2, CodeDeploy Agent cần được cài đặt để nhận lệnh deploy từ AWS.

### AppSpec File

File `appspec.yml` định nghĩa cách CodeDeploy triển khai ứng dụng, bao gồm vị trí copy file và các script cần chạy trong từng giai đoạn deploy.

## 3. Quá trình thực hiện

Các bước thực hiện:

* Truy cập dịch vụ CodeCommit.
* Tạo repository tên `NodeJS-App-Repo`.
* Chuẩn bị mã nguồn ứng dụng Node.js.
* Thêm file `buildspec.yml`.
* Thêm file `appspec.yml`.
* Push mã nguồn lên CodeCommit.
* Truy cập dịch vụ CodeDeploy.
* Tạo Application cho ứng dụng.
* Tạo Deployment Group.
* Gắn Deployment Group với EC2 instance thông qua tag.
* Đảm bảo EC2 đã cài CodeDeploy Agent.
* Cấu hình IAM Role phù hợp cho CodeDeploy.
* Truy cập CodePipeline.
* Tạo pipeline mới.
* Chọn CodeCommit làm Source Stage.
* Chọn CodeBuild làm Build Stage.
* Chọn CodeDeploy làm Deploy Stage.
* Chạy pipeline và theo dõi trạng thái từng stage.
* Kiểm tra ứng dụng sau khi pipeline hoàn tất.

## 4. Minh chứng thực hành

![Screenshot of creating IAM User Admin-Khoa](/my-hugo-site/images/1-Workblog/Week-4/image%20copy%209.png)

## 5. Kết quả đạt được

Sau khi hoàn thành bài lab, tôi đã hiểu được quy trình CI/CD cơ bản trên AWS. Pipeline có thể tự động lấy mã nguồn, build và deploy ứng dụng lên EC2, giúp giảm thao tác thủ công và tăng tính ổn định trong quá trình triển khai phần mềm.

---

## 5. Kết quả đạt được

Sau khi hoàn thành bài lab, tôi đã hiểu cách AWS WAF bảo vệ ứng dụng web ở tầng HTTP/HTTPS. Việc liên kết WAF với Application Load Balancer giúp tăng thêm một lớp phòng vệ trước khi request đi vào hệ thống backend.

---

## V. Thách thức gặp phải và cách xử lý

Trong tuần 4, tôi gặp một số vấn đề thực tế khi thực hành các dịch vụ nâng cao về bảo mật, kết nối mạng và CI/CD.

### 1. Giới hạn quyền IAM trong môi trường Sandbox

Một số dịch vụ AWS yêu cầu quyền tạo service-linked role hoặc quyền IAM nâng cao. Trong môi trường Sandbox hoặc tài khoản thực tập, các quyền này có thể bị giới hạn, khiến một số thao tác trên Console không thực hiện được.

Cách xử lý là kiểm tra kỹ thông báo lỗi, xác định quyền đang thiếu và thử thực hiện bằng AWS CLI nếu được phép. Ngoài ra, cần hiểu rõ vai trò của IAM Role khi các dịch vụ AWS cần giao tiếp với nhau.

### 2. Quản lý chi phí với các dịch vụ mạng nâng cao

Các dịch vụ như Transit Gateway, VPC Peering hoặc các thành phần bảo mật nâng cao có thể phát sinh chi phí nếu để chạy lâu. Vì vậy, cần có thói quen kiểm tra tài nguyên sau mỗi bài lab.

Cách xử lý của tôi là ghi chú những tài nguyên đã tạo, chụp minh chứng ngay sau khi hoàn thành và xóa các tài nguyên không còn sử dụng theo đúng thứ tự phụ thuộc.

### 3. Phức tạp khi cập nhật Route Table

Khi cấu hình VPC Peering hoặc Transit Gateway, chỉ tạo kết nối là chưa đủ. Route Table của từng VPC cần được cập nhật đúng để traffic có thể đi đến mạng đích.

Tôi xử lý bằng cách kiểm tra CIDR của từng VPC, thêm route đúng target và xác nhận trạng thái kết nối trước khi kiểm tra giao tiếp giữa các mạng.

### 4. Cấu hình CI/CD cần nhiều thành phần liên kết

CodePipeline yêu cầu nhiều dịch vụ hoạt động cùng nhau, gồm repository, build project, deployment group, EC2 instance, CodeDeploy Agent và IAM Role. Nếu một thành phần sai, pipeline có thể thất bại.

Để xử lý, tôi kiểm tra từng stage riêng biệt, xem log lỗi trong CodeBuild hoặc CodeDeploy và đảm bảo EC2 đã sẵn sàng nhận deploy.

---

## VI. Suy ngẫm sau tuần 4

Tuần 4 giúp tôi hiểu rõ hơn về cách một hệ thống cloud cấp doanh nghiệp được vận hành. Việc xây dựng hạ tầng không chỉ dừng lại ở việc tạo máy chủ hoặc database, mà còn cần bảo mật tập trung, kết nối mạng có kiểm soát, định tuyến quy mô lớn và quy trình triển khai ứng dụng tự động.

Tôi nhận ra rằng các dịch vụ như Security Hub, WAF, Transit Gateway và CodePipeline đều phục vụ cho những mục tiêu rất thực tế trong doanh nghiệp: bảo vệ hệ thống, kết nối nhiều môi trường, giảm thao tác thủ công và tăng khả năng kiểm soát vận hành.

Bài học quan trọng nhất trong tuần này là khi làm cloud engineering, cần tư duy theo cả hệ thống thay vì từng dịch vụ riêng lẻ. Một thay đổi nhỏ về IAM Role, Route Table hoặc Deployment Group đều có thể ảnh hưởng đến toàn bộ luồng vận hành.

---

## VII. Kế hoạch cho tuần tiếp theo

Trong tuần tiếp theo, tôi sẽ tiếp tục tập trung vào việc hoàn thiện tư duy kiến trúc hệ thống và chuẩn bị cho các nội dung liên quan đến dự án thực tế.

Các nội dung dự kiến gồm:

* Họp nhóm để thống nhất kiến trúc dự án tốt nghiệp.
* Phân tích hệ thống Website Đặt tour du lịch Việt Nam tích hợp Chatbot AI.
* Xác định các microservices chính của hệ thống.
* Lên kế hoạch phân chia mã nguồn và repository.
* Tìm hiểu cách triển khai ứng dụng thực tế trên AWS.
* Tham gia các workshop hoặc sự kiện kỹ thuật trong chương trình bootcamp.
* Tiếp tục nghiên cứu các kiến trúc mẫu từ AWS Solutions Architects.
* Cập nhật portfolio Hugo với ghi chú kỹ thuật và hình minh chứng thực hành.

