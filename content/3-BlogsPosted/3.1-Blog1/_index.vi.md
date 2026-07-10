---
title: "Blog 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---


# XÁC THỰC NGƯỜI DÙNG VÀ QUẢN LÝ SESSION VỚI AMAZON AURORA DSQL

Trong các ứng dụng backend, authentication và session management gần như là phần không thể thiếu.
Người dùng đăng ký, đăng nhập, giữ phiên làm việc, đăng xuất, thu hồi session — nghe có vẻ quen thuộc, nhưng khi hệ thống bắt đầu mở rộng thì bài toán này không còn đơn giản.
Một vấn đề thường gặp là database phải đảm bảo dữ liệu nhất quán gần như ngay lập tức. Ví dụ: người dùng vừa đăng ký xong thì phải đăng nhập được ngay, session vừa tạo phải dùng được ngay ở request tiếp theo, session vừa bị revoke thì không được phép tiếp tục truy cập.
Nếu hệ thống gặp replication lag hoặc phải tự quản lý nhiều database instance, read replica, credential, scaling và maintenance window, phần authentication có thể trở nên phức tạp hơn rất nhiều.
Để giải quyết bài toán này, AWS giới thiệu cách xây dựng dịch vụ xác thực người dùng và quản lý session với Amazon Aurora DSQL.
Giải pháp kết hợp Amazon Aurora DSQL, Amazon ECS Express Mode chạy trên AWS Fargate và AWS IAM để tạo một kiến trúc backend serverless, có khả năng mở rộng, nhất quán dữ liệu mạnh và hạn chế việc quản lý database credential thủ công.
━━━━━━━━━━━━━━━
MỘT SỐ ĐIỂM NỔI BẬT
━━━━━━━━━━━━━━━
• Dùng Amazon Aurora DSQL cho authentication workload:
Aurora DSQL là database serverless, tương thích PostgreSQL và hỗ trợ strong read-after-write consistency. Điều này rất phù hợp với các luồng đăng ký, đăng nhập và kiểm tra session vì dữ liệu vừa ghi có thể được đọc lại ngay.
• Không cần quản lý database instance:
Thay vì phải tự provision instance, cấu hình read replica hay tính toán capacity, Aurora DSQL tự động scale theo workload. Với authentication service, đây là điểm quan trọng vì lượng login có thể tăng đột biến.
• Kết nối database bằng IAM authentication:
Ứng dụng không cần lưu database password trong environment variable hoặc file cấu hình. Amazon ECS task role được cấp quyền kết nối đến Aurora DSQL, còn connector sẽ tự xử lý IAM token ngắn hạn.
• Chạy ứng dụng trên Amazon ECS Express Mode/Fargate:
Ứng dụng Node.js/Express được container hóa và chạy trên Fargate. ECS Express Mode hỗ trợ triển khai nhanh, tự động scale, tích hợp load balancing và giảm bớt phần quản lý hạ tầng.
• Thiết kế bảng users và sessions rõ ràng:
Dữ liệu người dùng được lưu trong bảng users, còn session được lưu trong bảng sessions. Mỗi session có token hash, thời gian tạo, thời gian hết hạn và thời điểm bị revoke nếu có.
• Không lưu session token dạng plaintext:
Session token chỉ được trả về cho client một lần khi tạo. Trong database chỉ lưu SHA-256 hash của token. Cách này giúp giảm rủi ro nếu dữ liệu trong database bị truy cập ngoài ý muốn.
• Bảo mật password bằng bcrypt:
Password của người dùng được hash bằng bcrypt trước khi lưu. Đây là cách tiếp cận phổ biến để bảo vệ thông tin đăng nhập trong backend application.
• Hạn chế lộ thông tin khi login sai:
Hệ thống trả về lỗi chung cho cả trường hợp email không tồn tại và password sai. Điều này giúp tránh user enumeration, tức là tránh để attacker đoán được tài khoản nào đang tồn tại trong hệ thống.
━━━━━━━━━━━━━━━
LUỒNG XÁC THỰC HOẠT ĐỘNG NHƯ THẾ NÀO?
━━━━━━━━━━━━━━━
Client gửi request HTTPS đến backend application.
Ứng dụng Node.js/Express chạy trên Amazon ECS Express Mode tiếp nhận request, kiểm tra input, xử lý logic đăng ký hoặc đăng nhập.
Khi người dùng đăng ký, ứng dụng hash password bằng bcrypt, tạo UUID cho user và lưu thông tin vào bảng users trong Aurora DSQL.
Khi người dùng đăng nhập, ứng dụng kiểm tra email và password. Nếu hợp lệ, hệ thống tạo session token, hash token bằng SHA-256 rồi lưu token hash vào bảng sessions.
Ở các request cần xác thực, client gửi session token. Backend hash token nhận được, so sánh với token hash trong database và kiểm tra session còn hiệu lực hay không.
Khi người dùng logout hoặc revoke session, hệ thống cập nhật trường revoked_at. Nhờ strong consistency của Aurora DSQL, session bị revoke sẽ bị từ chối ngay ở những request sau.
━━━━━━━━━━━━━━━
VÌ SAO AURORA DSQL PHÙ HỢP CHO BÀI TOÁN NÀY?
━━━━━━━━━━━━━━━
Điểm mình thấy đáng chú ý nhất là authentication không chỉ cần database “lưu được dữ liệu”, mà còn cần dữ liệu nhất quán và phản hồi nhanh.
Với một số hệ thống phân tán, dữ liệu vừa ghi có thể chưa đọc được ngay do replication lag. Điều này gây khó chịu trong các luồng như đăng ký xong đăng nhập ngay, hoặc revoke session nhưng session vẫn còn hiệu lực trong một khoảng thời gian ngắn.
Aurora DSQL giải quyết vấn đề này bằng strong read-after-write consistency. Với authentication service, đây là lợi thế rất lớn vì trạng thái user và session cần chính xác gần như ngay lập tức.
Ngoài ra, việc dùng IAM authentication cũng giúp giảm rủi ro bảo mật. Thay vì quản lý database password thủ công, hệ thống dùng IAM role và token ngắn hạn để kết nối database.
━━━━━━━━━━━━━━━
BÀI HỌC RÚT RA
━━━━━━━━━━━━━━━
Điều mình rút ra từ bài viết này: authentication service tuy là chức năng rất quen thuộc, nhưng khi thiết kế trên cloud vẫn cần quan tâm nhiều đến consistency, security và scalability.
Amazon Aurora DSQL không chỉ phù hợp cho các ứng dụng cần SQL, mà còn rất đáng chú ý với những workload yêu cầu dữ liệu nhất quán mạnh như đăng ký, đăng nhập và quản lý session.
Khi kết hợp Aurora DSQL với ECS Express Mode/Fargate và IAM authentication, backend có thể giảm được nhiều phần vận hành thủ công: không cần quản lý database instance, không cần lưu database password, dễ scale hơn và vẫn giữ được mô hình bảo mật rõ ràng.
Với mình, đây là một ví dụ khá hay về cách thiết kế backend hiện đại trên AWS: tách rõ compute, database và security layer, đồng thời tận dụng các dịch vụ managed/serverless để giảm gánh nặng vận hành.


![alt text](/my-hugo-site/images/3-Blogpost/huy.png)


Link bài viết gốc AWS:
[https://aws.amazon.com/.../user-authentication-and.../...](https://aws.amazon.com/.../user-authentication-and.../...)
#AWS#AmazonAurora#AuroraDSQL#Backend#Database#IAM#Security #SessionManagement #CloudComputing #AWSStudyGroup