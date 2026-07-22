---
title: "Bản đề xuất"
date: 2026-06-05
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

Trong bối cảnh chuyển đổi số diễn ra mạnh mẽ trên nhiều lĩnh vực như giáo dục, giải trí, đào tạo doanh nghiệp và truyền thông số, nhu cầu xây dựng các nền tảng phát video theo yêu cầu (Video-on-Demand – VoD) ngày càng gia tăng. Người dùng mong muốn có thể tải lên, quản lý và xem nội dung video mọi lúc, mọi nơi với chất lượng ổn định, thời gian phản hồi nhanh và khả năng truy cập đồng thời từ nhiều thiết bị khác nhau.

Tuy nhiên, việc xây dựng một hệ thống Video-on-Demand theo mô hình truyền thống thường gặp nhiều khó khăn. Máy chủ ứng dụng phải đảm nhận đồng thời nhiều nhiệm vụ như tiếp nhận video, lưu trữ dữ liệu, chuyển đổi định dạng, phân phối nội dung và quản lý người dùng. Khi số lượng video hoặc số lượng người truy cập tăng lên, hệ thống dễ gặp tình trạng quá tải, yêu cầu mở rộng hạ tầng liên tục và phát sinh chi phí vận hành lớn.

Bên cạnh đó, quá trình chuyển mã (Transcoding) video đòi hỏi nhiều tài nguyên tính toán. Nếu xử lý trực tiếp trên máy chủ ứng dụng sẽ làm ảnh hưởng đến hiệu năng của toàn bộ hệ thống, đồng thời làm tăng thời gian phản hồi đối với các yêu cầu khác của người dùng.

Để giải quyết các vấn đề trên, nhóm đề xuất xây dựng **Serverless Video-on-Demand Platform on AWS**, một nền tảng phát video theo yêu cầu dựa trên kiến trúc **Serverless** kết hợp với **Event-Driven Architecture**. Giải pháp tận dụng các dịch vụ được quản lý hoàn toàn của Amazon Web Services nhằm tự động hóa toàn bộ quy trình tải lên, xử lý, lưu trữ và phân phối video mà không cần quản trị máy chủ.

Thông qua việc sử dụng Amazon Cognito, Amazon API Gateway, AWS Lambda, Amazon S3, Amazon DynamoDB, Amazon EventBridge, Amazon SQS, EventBridge Pipes, AWS Step Functions, AWS Elemental MediaConvert, Amazon CloudFront và Amazon CloudWatch, hệ thống hướng tới mục tiêu xây dựng một nền tảng có khả năng mở rộng linh hoạt, hoạt động ổn định, tối ưu chi phí và dễ dàng bảo trì trong quá trình vận hành.

Đề xuất này đóng vai trò là nền tảng để nhóm triển khai các giai đoạn phân tích, thiết kế, xây dựng, kiểm thử và hoàn thiện sản phẩm trong suốt quá trình thực hiện đồ án.

## Bài toán đặt ra

Các nền tảng phát video trực tuyến hiện nay phải xử lý nhiều tác vụ khác nhau như quản lý người dùng, tải video lên hệ thống, lưu trữ dữ liệu, chuyển đổi định dạng video, cập nhật trạng thái xử lý và phân phối nội dung đến người xem. Nếu tất cả các chức năng này được triển khai trên cùng một máy chủ ứng dụng, hệ thống sẽ nhanh chóng trở thành điểm nghẽn khi số lượng người dùng hoặc số lượng video tăng lên.

Ngoài áp lực về tài nguyên tính toán, việc mở rộng hệ thống theo mô hình truyền thống còn yêu cầu bổ sung máy chủ, cấu hình cân bằng tải, mở rộng cơ sở dữ liệu và thực hiện nhiều thao tác quản trị hạ tầng phức tạp. Điều này không chỉ làm tăng chi phí đầu tư ban đầu mà còn kéo theo chi phí vận hành và bảo trì trong suốt vòng đời của hệ thống.

Đối với bài toán Video-on-Demand, quá trình chuyển đổi video sang nhiều độ phân giải và định dạng phát trực tuyến thường tiêu tốn nhiều thời gian xử lý. Nếu máy chủ phải vừa phục vụ người dùng vừa thực hiện chuyển mã video, hiệu năng tổng thể sẽ giảm đáng kể và trải nghiệm người dùng không được đảm bảo.

Ngoài ra, hệ thống cũng cần đảm bảo khả năng xử lý bất đồng bộ, cho phép nhiều video được xử lý đồng thời mà không làm ảnh hưởng đến các chức năng khác. Đây là yêu cầu quan trọng đối với các nền tảng phát video hiện đại.

## Giải pháp đề xuất

Nhằm khắc phục các hạn chế trên, nhóm đề xuất triển khai hệ thống theo mô hình **Serverless Video-on-Demand Platform** trên nền tảng Amazon Web Services.

Kiến trúc đề xuất sử dụng Amazon Cognito để xác thực người dùng trước khi truy cập hệ thống. Sau khi đăng nhập thành công, người dùng sẽ gửi yêu cầu thông qua Amazon API Gateway. AWS Lambda tiếp nhận yêu cầu, xử lý nghiệp vụ và tạo Pre-signed URL để người dùng tải video trực tiếp lên Amazon S3 Raw Upload Bucket mà không cần truyền dữ liệu qua Backend.

Sau khi video được tải lên thành công, Amazon EventBridge sẽ phát hiện sự kiện phát sinh và chuyển thông tin đến Amazon SQS. EventBridge Pipes tiếp tục truyền dữ liệu sang AWS Step Functions nhằm điều phối toàn bộ quy trình xử lý video. AWS Elemental MediaConvert chịu trách nhiệm chuyển mã video sang định dạng HLS phù hợp cho việc phát trực tuyến.

Kết quả xử lý được lưu trữ trong Amazon S3 Processed Media Bucket, đồng thời AWS Lambda cập nhật trạng thái xử lý vào Amazon DynamoDB để người dùng có thể theo dõi tiến trình trên giao diện Web.

Video sau khi hoàn tất sẽ được Amazon CloudFront phân phối thông qua mạng CDN toàn cầu nhằm giảm độ trễ và cải thiện tốc độ truy cập. Toàn bộ hoạt động của hệ thống được giám sát bởi Amazon CloudWatch để hỗ trợ theo dõi nhật ký, cảnh báo và đánh giá hiệu năng.

Nhờ áp dụng kiến trúc Serverless kết hợp Event-Driven Architecture, hệ thống chỉ sử dụng tài nguyên khi phát sinh yêu cầu thực tế, giúp tối ưu chi phí, giảm sự phụ thuộc giữa các thành phần và nâng cao khả năng mở rộng khi quy mô hệ thống tăng lên.

---

## Kiến trúc đề xuất

Kiến trúc của hệ thống được đề xuất dựa trên nguyên tắc phân tách các thành phần theo từng lớp chức năng độc lập. Mỗi dịch vụ AWS đảm nhiệm một vai trò riêng và giao tiếp với nhau thông qua API hoặc các sự kiện phát sinh trong quá trình xử lý. Cách tổ chức này giúp hệ thống dễ dàng mở rộng, bảo trì và thay thế từng thành phần mà không ảnh hưởng đến toàn bộ kiến trúc.

<img src="/images/2-Proposal/1.png" alt="Serverless Video-on-Demand Platform Architecture" style="max-width:100%; height:auto;" />

Hình 2.1. Kiến trúc đề xuất của hệ thống Serverless Video-on-Demand Platform on AWS.

Kiến trúc được chia thành các lớp chính gồm Identity & Access Layer, Edge & Content Delivery Layer, Application Layer, Data Layer, Event Processing Layer, Media Processing Layer và Monitoring Layer.

## Mô tả kiến trúc đề xuất

Lớp đầu tiên của hệ thống là **Identity & Access Layer**, trong đó Amazon Cognito được đề xuất để quản lý người dùng và thực hiện xác thực. Dịch vụ này hỗ trợ đăng ký, đăng nhập, quản lý phiên làm việc và cấp phát JSON Web Token (JWT). Sau khi xác thực thành công, JWT sẽ được sử dụng trong các yêu cầu gửi đến Amazon API Gateway nhằm đảm bảo chỉ những người dùng hợp lệ mới có thể truy cập các chức năng của hệ thống.

Tiếp theo là **Edge & Content Delivery Layer**, bao gồm Amazon CloudFront, AWS WAF và Amazon S3 Frontend Bucket. Amazon S3 được sử dụng để lưu trữ giao diện Web dưới dạng Static Website, trong khi CloudFront chịu trách nhiệm phân phối nội dung thông qua hệ thống CDN toàn cầu nhằm giảm độ trễ và tăng tốc độ truy cập. AWS WAF được bố trí phía trước CloudFront nhằm lọc các yêu cầu độc hại, hạn chế các hình thức tấn công phổ biến vào ứng dụng Web và tăng cường mức độ bảo mật cho toàn bộ hệ thống.

**Application Layer** được xây dựng trên Amazon API Gateway kết hợp với AWS Lambda. API Gateway đóng vai trò là cổng giao tiếp giữa người dùng và các dịch vụ Backend. Các Lambda Function chịu trách nhiệm xử lý nghiệp vụ như tạo Presigned URL, quản lý thông tin video, truy vấn danh sách video, cập nhật trạng thái xử lý và giao tiếp với Amazon DynamoDB. Việc sử dụng kiến trúc Function-as-a-Service giúp các Lambda Function chỉ được kích hoạt khi phát sinh yêu cầu, từ đó tối ưu chi phí sử dụng tài nguyên.

Tại **Data Layer**, Amazon DynamoDB được đề xuất để lưu trữ metadata của video. Mỗi bản ghi bao gồm các thông tin như Video ID, tên video, trạng thái xử lý, thời gian tải lên, đường dẫn phát video và các thông tin cần thiết khác. DynamoDB được lựa chọn nhờ khả năng mở rộng linh hoạt, độ trễ thấp và tương thích tốt với các ứng dụng Serverless.

Video gốc sau khi được tải lên sẽ được lưu trong **Amazon S3 Raw Upload Bucket**. Khi xuất hiện sự kiện Object Created, Amazon EventBridge sẽ phát hiện thay đổi và phát sinh sự kiện tương ứng. Sự kiện này được chuyển đến Amazon SQS nhằm tách biệt quá trình tải video và xử lý video, đồng thời giúp hệ thống có thể xử lý bất đồng bộ và tăng khả năng chịu tải khi có nhiều video được tải lên đồng thời.

Từ Amazon SQS, EventBridge Pipes sẽ tiếp nhận thông điệp và truyền đến AWS Step Functions. Step Functions đóng vai trò điều phối toàn bộ quy trình xử lý video, bao gồm khởi tạo tác vụ chuyển mã, theo dõi trạng thái và cập nhật kết quả sau khi quá trình xử lý hoàn tất. Việc sử dụng Step Functions giúp quy trình xử lý được quản lý tập trung, dễ dàng mở rộng và thuận tiện trong việc giám sát từng bước thực hiện.

Đối với **Media Processing Layer**, nhóm đề xuất sử dụng AWS Elemental MediaConvert để chuyển đổi video sang định dạng HLS phù hợp với nhu cầu phát trực tuyến. Sau khi hoàn thành, toàn bộ tệp đầu ra sẽ được lưu trữ trong Amazon S3 Processed Media Bucket. Định dạng HLS cho phép video được phát với nhiều mức chất lượng khác nhau, góp phần nâng cao trải nghiệm người dùng trên các thiết bị và điều kiện mạng khác nhau.

Cuối cùng, **Monitoring Layer** sử dụng Amazon CloudWatch nhằm thu thập nhật ký hoạt động, giám sát hiệu năng và hỗ trợ cảnh báo khi phát sinh lỗi trong quá trình vận hành. Các thông tin này giúp nhóm dễ dàng theo dõi trạng thái của từng dịch vụ, đánh giá hiệu năng hệ thống và nhanh chóng xử lý các sự cố nếu có.

## Kế hoạch triển khai

Việc triển khai hệ thống được đề xuất theo từng giai đoạn nhằm giảm thiểu rủi ro và thuận tiện trong quá trình kiểm thử. Giai đoạn đầu tập trung xây dựng hạ tầng lưu trữ, xác thực người dùng và các API cơ bản. Tiếp theo, nhóm triển khai quy trình tải video lên Amazon S3 và xây dựng cơ chế xử lý sự kiện thông qua EventBridge, Amazon SQS và EventBridge Pipes.

Sau khi hoàn thiện luồng xử lý sự kiện, nhóm tiếp tục triển khai AWS Step Functions và AWS Elemental MediaConvert để tự động hóa quá trình chuyển mã video. Cuối cùng, hệ thống được tích hợp với Amazon CloudFront để phân phối nội dung, đồng thời sử dụng Amazon CloudWatch để giám sát hoạt động và đánh giá hiệu năng tổng thể.

Trong suốt quá trình triển khai, từng thành phần sẽ được kiểm thử độc lập trước khi tiến hành tích hợp nhằm bảo đảm tính ổn định và giảm thiểu lỗi phát sinh khi vận hành.

## Chi phí dự kiến

Đề xuất ưu tiên sử dụng các dịch vụ Serverless của AWS nhằm tối ưu chi phí đầu tư ban đầu. Phần lớn các dịch vụ như AWS Lambda, Amazon API Gateway, Amazon EventBridge, Amazon SQS và AWS Step Functions đều được tính phí dựa trên số lượng yêu cầu thực tế thay vì phải duy trì máy chủ hoạt động liên tục.

Amazon S3 được sử dụng để lưu trữ dữ liệu với chi phí thấp, trong khi Amazon CloudFront chỉ phát sinh chi phí dựa trên dung lượng dữ liệu truyền tải đến người dùng. Nhờ mô hình thanh toán theo mức sử dụng, hệ thống có thể vận hành với chi phí phù hợp trong giai đoạn thử nghiệm và dễ dàng mở rộng khi quy mô người dùng tăng lên.

## Kết quả mong đợi

Thông qua kiến trúc được đề xuất, hệ thống kỳ vọng xây dựng thành công một nền tảng Video-on-Demand hoạt động trên nền tảng Amazon Web Services với khả năng mở rộng linh hoạt, hiệu năng ổn định và mức độ bảo mật cao.

Hệ thống dự kiến cho phép người dùng đăng nhập, tải video lên, theo dõi trạng thái xử lý và phát video trực tuyến thông qua định dạng HLS. Đồng thời, toàn bộ quy trình chuyển mã video sẽ được tự động hóa dựa trên mô hình Event-Driven Architecture, giúp giảm thiểu sự can thiệp thủ công và nâng cao hiệu quả vận hành.

Ngoài ra, việc sử dụng các dịch vụ Serverless còn giúp giảm đáng kể công sức quản trị hạ tầng, tối ưu chi phí sử dụng tài nguyên và tạo tiền đề thuận lợi cho việc mở rộng hệ thống trong tương lai.

## Kết luận

Kiến trúc được đề xuất đáp ứng đầy đủ các yêu cầu của bài toán Video-on-Demand trên nền tảng điện toán đám mây. Việc kết hợp kiến trúc Serverless với Event-Driven Architecture giúp các thành phần hoạt động độc lập, tăng khả năng mở rộng, nâng cao độ ổn định và tối ưu chi phí vận hành.

Đề xuất này sẽ là cơ sở để nhóm triển khai các bước phân tích, thiết kế, xây dựng, kiểm thử và đánh giá hệ thống trong các chương tiếp theo của báo cáo, đồng thời hướng tới mục tiêu xây dựng một nền tảng Video-on-Demand hiện đại, linh hoạt và phù hợp với các xu hướng phát triển của điện toán đám mây.
