---
title: "Các bài blogs đã đăng"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3. </b> "
---



###  [Blog 1 - XÁC THỰC NGƯỜI DÙNG VÀ QUẢN LÝ SESSION VỚI AMAZON AURORA DSQL](3.1-Blog1/)
Trong các ứng dụng backend, authentication và session management gần như là phần không thể thiếu.
Người dùng đăng ký, đăng nhập, giữ phiên làm việc, đăng xuất, thu hồi session — nghe có vẻ quen thuộc, nhưng khi hệ thống bắt đầu mở rộng thì bài toán này không còn đơn giản.

###  [Blog 2 - TỰ ĐỘNG SAO CHÉP CẤU HÌNH S3 BUCKET GIỮA CÁC REGION TRÊN AWS](3.2-Blog2/)
Khi các doanh nghiệp vận hành hệ thống trên AWS trong thời gian dài, việc có hàng trăm hoặc hàng nghìn Amazon S3 bucket trong cùng một Region là điều rất phổ biến. Mỗi bucket có thể được tạo ra ở những thời điểm khác nhau, bởi những nhóm khác nhau, bằng nhiều cách khác nhau như AWS Management Console, script nội bộ hoặc các công cụ tự động hóa cũ.

###  [Blog 3 - TỐI ƯU HÓA HIỆU NĂNG VÀ ĐIỀU PHỐI DỊCH VỤ TRONG KIẾN TRÚC SERVERLESS QUY MÔ LỚN](3.3-Blog3/)
Khi mới bắt đầu áp dụng mô hình **Serverless**, hầu hết chúng ta đều bị thu hút bởi lời hứa quen thuộc của các nhà cung cấp dịch vụ đám mây: chỉ cần tập trung vào việc viết mã, triển khai ứng dụng và hệ thống sẽ tự động mở rộng theo nhu cầu.
Tuy nhiên, giữa lý thuyết và thực tế vận hành ở quy mô doanh nghiệp luôn tồn tại một khoảng cách rất lớn.
Một câu hỏi quan trọng được đặt ra là: điều gì sẽ xảy ra khi hệ thống không chỉ xử lý vài nghìn request, mà phải mở rộng đến hàng trăm nghìn hoặc thậm chí **1 triệu AWS Lambda function chạy đồng thời**?