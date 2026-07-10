---
title: "Blog 2"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# TỰ ĐỘNG SAO CHÉP CẤU HÌNH S3 BUCKET GIỮA CÁC REGION TRÊN AWS

Khi các doanh nghiệp vận hành hệ thống trên AWS trong thời gian dài, việc có hàng trăm hoặc hàng nghìn Amazon S3 bucket trong cùng một Region là điều rất phổ biến. Mỗi bucket có thể được tạo ra ở những thời điểm khác nhau, bởi những nhóm khác nhau, bằng nhiều cách khác nhau như AWS Management Console, script nội bộ hoặc các công cụ tự động hóa cũ.

Theo thời gian, mỗi bucket sẽ tích lũy nhiều cấu hình riêng như bucket policy, lifecycle rule, encryption, access control, logging, tag, Object Lock hoặc Static Website Hosting. Những cấu hình này có thể liên quan trực tiếp đến bảo mật, tuân thủ, chi phí và cách dữ liệu được quản lý trong doanh nghiệp.

Vấn đề bắt đầu xuất hiện khi doanh nghiệp cần tạo lại các bucket đó ở một AWS Region khác với cấu hình giống hệt bucket gốc. Nếu làm thủ công, đội ngũ vận hành phải mở từng bucket, kiểm tra từng cấu hình, rồi áp dụng lại từng phần ở Region mới. Cách làm này không chỉ mất thời gian mà còn rất dễ sai sót, đặc biệt khi số lượng bucket ngày càng lớn.

Amazon S3 đã có các tính năng như Cross-Region Replication để đồng bộ object mới giữa các Region, hoặc S3 Batch Operations để sao chép object hiện có với quy mô lớn. Tuy nhiên, các tính năng này chủ yếu tập trung vào dữ liệu object bên trong bucket. Còn cấu hình cấp bucket vẫn cần được xử lý bằng các API riêng biệt.

Để giải quyết bài toán này, AWS giới thiệu một giải pháp sử dụng AWS Step Functions và AWS Lambda nhằm tự động sao chép cấu hình của một S3 bucket từ Region nguồn sang Region đích. Giải pháp này có thể tạo bucket mới ở Region đích, đọc cấu hình từ bucket nguồn, áp dụng lại các cấu hình đó lên bucket đích và ghi nhận toàn bộ quá trình vào Amazon DynamoDB và Amazon CloudWatch để phục vụ kiểm toán.

Đây là một kiến trúc phù hợp khi doanh nghiệp cần di chuyển, mở rộng hoặc tái tạo cấu hình bucket giữa các Region mà không muốn thực hiện thủ công từng bước.

---

## NHỮNG ĐIỂM NỔI BẬT

### Tự động tạo bucket ở Region đích

Giải pháp cho phép người dùng truyền vào bucket nguồn và Region đích. Nếu không chỉ định tên bucket đích, hệ thống có thể tự tạo một tên bucket mới với tiền tố riêng để tránh trùng lặp. Điều này giúp quá trình triển khai linh hoạt hơn, đặc biệt trong các bài toán cần nhân bản nhiều bucket.

### Sao chép cấu hình bucket bằng AWS Lambda

Một Lambda function sẽ đọc các cấu hình của bucket nguồn thông qua S3 API, sau đó áp dụng lại từng cấu hình lên bucket đích. Nhờ vậy, các thiết lập quan trọng như policy, lifecycle rule, encryption, CORS, tag, logging hoặc Object Lock có thể được tái tạo một cách có hệ thống.

### Điều phối quy trình bằng AWS Step Functions

AWS Step Functions đóng vai trò điều phối toàn bộ workflow. Thay vì chạy nhiều thao tác rời rạc, Step Functions chia quy trình thành các bước rõ ràng như tạo bucket, sao chép cấu hình, kiểm tra trạng thái và xử lý lỗi. Nếu một bước không trả về trạng thái thành công, workflow sẽ chuyển sang trạng thái thất bại và ghi nhận lại kết quả.

### Ghi log và kiểm toán tập trung

Mỗi lần chạy workflow đều được ghi lại trong Amazon DynamoDB. Thông tin bao gồm bucket nguồn, bucket đích, trạng thái thực thi và snapshot cấu hình đã sao chép. Bên cạnh đó, Amazon CloudWatch ghi lại log chi tiết của từng Lambda function và lịch sử thực thi của Step Functions. Điều này rất quan trọng với các doanh nghiệp cần audit trail rõ ràng.

### Phù hợp cho môi trường cross-Region trong cùng một AWS account

Giải pháp này được thiết kế để sao chép cấu hình bucket giữa các Region trong cùng một AWS account. Các Lambda function sử dụng IAM role được tạo bởi AWS CDK để có quyền đọc cấu hình bucket nguồn và ghi cấu hình lên bucket đích.

---

## KIẾN TRÚC GIẢI PHÁP HOẠT ĐỘNG NHƯ THẾ NÀO?

Về tổng thể, workflow gồm hai Lambda function chính được điều phối bởi AWS Step Functions.

Lambda function đầu tiên có nhiệm vụ tạo destination bucket tại Region đích. Sau khi bucket được tạo, function này ghi một bản ghi ban đầu vào DynamoDB để đánh dấu quá trình replicate đã bắt đầu.

Lambda function thứ hai sẽ đọc các cấu hình từ source bucket, sau đó áp dụng lại các cấu hình đó lên destination bucket. Nếu source bucket có bật server access logging, function này cũng có thể tạo thêm một logging bucket chuyên dụng ở Region đích để đảm bảo cấu hình logging sau khi sao chép vẫn trỏ đến một bucket hợp lệ.

AWS Step Functions sẽ kiểm tra trạng thái trả về của từng bước. Nếu bước tạo bucket thành công, workflow tiếp tục sang bước sao chép cấu hình. Nếu bước sao chép cấu hình hoàn tất, workflow kết thúc với trạng thái thành công. Ngược lại, nếu có lỗi ở bất kỳ bước nào, workflow sẽ chuyển sang trạng thái thất bại và cập nhật kết quả vào DynamoDB.

Có thể hình dung luồng hoạt động như sau:

```txt
Source S3 Bucket 
→ Step Functions Workflow 
→ Lambda Create Bucket 
→ Destination S3 Bucket 
→ Lambda Copy Configuration 
→ DynamoDB Audit Table & CloudWatch Logs

Trong kiến trúc này:

Source Bucket là bucket gốc chứa các cấu hình cần sao chép.
Destination Bucket là bucket mới được tạo ở Region đích.
AWS Step Functions là thành phần điều phối toàn bộ quy trình.
AWS Lambda thực hiện các thao tác tạo bucket và sao chép cấu hình.
Amazon DynamoDB lưu lại lịch sử thực thi và snapshot cấu hình.
Amazon CloudWatch ghi log chi tiết để phục vụ giám sát và debug.
NHỮNG CẤU HÌNH S3 CÓ THỂ ĐƯỢC SAO CHÉP

Điểm mạnh của giải pháp này là nó không chỉ tạo bucket mới, mà còn cố gắng tái tạo nhiều cấu hình quan trọng ở cấp bucket.

Các cấu hình được hỗ trợ bao gồm:

Bucket policy.
Lifecycle rules.
Versioning, bao gồm cả thiết lập MFA Delete trong một số điều kiện nhất định.
Server-side encryption, gồm SSE-S3 và SSE-KMS.
CORS rules.
Server access logging.
Bucket tags.
Block Public Access settings.
Bucket ownership controls.
Bucket ACLs trong trường hợp phù hợp.
Object Lock configuration.
Requester Pays.
Static website hosting configuration.

Nhờ vậy, destination bucket sau khi được tạo sẽ có cấu hình gần giống với source bucket nhất có thể, thay vì chỉ là một bucket trống ở Region mới.

TÌNH HUỐNG THỰC TẾ

Giả sử một doanh nghiệp tài chính đang vận hành hàng nghìn S3 bucket tại Region us-east-1. Các bucket này chứa dữ liệu báo cáo, log hệ thống, file phân tích và dữ liệu phục vụ nhiều phòng ban khác nhau.

Sau một yêu cầu mới về tuân thủ dữ liệu hoặc data residency, doanh nghiệp cần tái tạo các bucket tương ứng tại một Region khác, ví dụ us-east-2, để phục vụ chiến lược mở rộng hạ tầng hoặc dự phòng khu vực.

Nếu làm thủ công, đội ngũ Cloud phải kiểm tra từng bucket một:

Bucket này có lifecycle rule nào?
Bucket kia có bật encryption không?
Bucket policy đang cho phép role nào truy cập?
Bucket có bật access logging hay Object Lock không?
Bucket có tag nào phục vụ phân bổ chi phí không?

Với vài bucket thì việc này có thể chấp nhận được. Nhưng với hàng trăm hoặc hàng nghìn bucket, cách làm thủ công gần như không khả thi.

Khi sử dụng giải pháp AWS Step Functions và Lambda, doanh nghiệp chỉ cần cung cấp thông tin bucket nguồn và Region đích. Workflow sẽ tự động tạo bucket đích, sao chép cấu hình, ghi log và lưu lại kết quả kiểm toán.

Điều này giúp giảm rất nhiều thời gian vận hành, hạn chế lỗi cấu hình thủ công và tạo ra một quy trình có thể lặp lại nhiều lần.

VÌ SAO KHÔNG CHỈ DÙNG INFRASTRUCTURE AS CODE?

Một điểm rất hay trong bài viết của AWS là họ không xem giải pháp này thay thế hoàn toàn cho Infrastructure as Code.

Nếu doanh nghiệp đang xây dựng hệ thống mới từ đầu, cách tốt nhất vẫn là sử dụng AWS CloudFormation hoặc AWS CDK để định nghĩa bucket và toàn bộ cấu hình dưới dạng template. Cách làm này giúp hạ tầng có tính nhất quán, dễ version control và dễ triển khai lại.

Tuy nhiên, thực tế doanh nghiệp không phải lúc nào cũng có một hệ thống “sạch” ngay từ đầu. Nhiều bucket đã tồn tại nhiều năm, được tạo bởi nhiều nhóm khác nhau, không còn script gốc hoặc không được quản lý bằng IaC.

Trong trường hợp đó, việc sao chép cấu hình từ bucket đang chạy thực tế sẽ hữu ích hơn. Giải pháp này phù hợp với các bucket legacy, các môi trường đã vận hành lâu năm hoặc các bài toán cần replicate nhanh cấu hình hiện có sang Region mới.

Nói cách khác:

IaC phù hợp cho hệ thống mới, được thiết kế chuẩn ngay từ đầu.
Workflow replicate cấu hình phù hợp cho hệ thống đã tồn tại, cần sao chép trạng thái thực tế hiện tại.
NHỮNG ĐIỂM CẦN LƯU Ý TRƯỚC KHI TRIỂN KHAI

Mặc dù giải pháp này rất hữu ích, vẫn có một số điểm quan trọng cần chú ý.

Thứ nhất, AWS KMS key là tài nguyên phụ thuộc Region. Nếu source bucket sử dụng SSE-KMS, destination Region cần có KMS key phù hợp để Lambda có thể áp dụng lại cấu hình encryption. Không thể chỉ sao chép máy móc key từ Region nguồn sang Region đích.

Thứ hai, nếu source bucket sử dụng Object Lock và bạn muốn bật Object Lock cho destination bucket, bucket đích cần được tạo với Object Lock ngay từ đầu. Đây là một thiết lập đặc biệt của S3 và không thể bật tùy ý sau khi bucket đã được tạo trong mọi trường hợp.

Thứ ba, bucket policy được sao chép dưới dạng JSON gốc. Điều này có nghĩa là nếu policy trong source bucket có Resource ARN trỏ trực tiếp đến bucket nguồn, sau khi sao chép sang bucket đích thì policy vẫn có thể đang tham chiếu đến ARN cũ. Vì vậy, đội ngũ vận hành cần kiểm tra và cập nhật lại bucket policy nếu có các ARN gắn chặt với bucket nguồn.

Thứ tư, workflow không tự động rollback nếu một cấu hình nào đó bị lỗi trong quá trình sao chép. Các cấu hình được áp dụng theo thứ tự, nếu một bước thất bại thì workflow dừng lại và đánh dấu FAILED trong DynamoDB. Khi đó, người vận hành cần kiểm tra CloudWatch Logs, xử lý nguyên nhân lỗi rồi chạy lại workflow.

Thứ năm, với các bucket có cấu hình rất lớn, ví dụ policy dài hoặc nhiều lifecycle rule phức tạp, Lambda timeout hoặc memory có thể cần được điều chỉnh để phù hợp hơn với workload thực tế.

ĐIỀU MÌNH HỌC ĐƯỢC SAU BÀI VIẾT NÀY

Sau khi đọc bài viết này, mình nhận ra một điều rất thực tế: quản lý S3 ở quy mô doanh nghiệp không chỉ đơn giản là tạo bucket và upload object. Phần cấu hình cấp bucket mới là nơi chứa rất nhiều yếu tố quan trọng liên quan đến bảo mật, tuân thủ, chi phí và vận hành.

S3 Cross-Region Replication có thể giúp đồng bộ dữ liệu object giữa các Region, nhưng cấu hình bucket vẫn là một bài toán riêng. Nếu không có quy trình tự động hóa, việc tái tạo cấu hình thủ công rất dễ gây sai lệch giữa môi trường nguồn và môi trường đích.

Điểm mình thấy hay trong kiến trúc này là AWS sử dụng Step Functions để biến một chuỗi thao tác phức tạp thành một workflow rõ ràng, có trạng thái, có kiểm tra lỗi và có audit trail. Lambda xử lý phần logic sao chép, DynamoDB lưu lịch sử, CloudWatch hỗ trợ quan sát và debug. Đây là một cách kết hợp rất thực tế giữa các dịch vụ serverless trên AWS.

Theo mình, bài này rất phù hợp với những ai đang học theo hướng Cloud Engineer, DevOps Engineer hoặc Solutions Architect, vì nó cho thấy cách AWS giải quyết một vấn đề vận hành thực tế: không chỉ triển khai tài nguyên mới, mà còn chuẩn hóa và tự động hóa những tài nguyên đã tồn tại trong hệ thống.

KẾT LUẬN

Giải pháp sao chép cấu hình Amazon S3 bucket giữa các Region bằng AWS Step Functions và AWS Lambda là một mô hình rất thực tế cho các doanh nghiệp đang vận hành nhiều bucket lâu năm trên AWS.

Thay vì phải mở từng bucket và sao chép thủ công từng cấu hình, workflow này giúp tự động tạo bucket đích, áp dụng lại các thiết lập quan trọng, ghi log chi tiết và lưu lại lịch sử thực thi để phục vụ kiểm toán.

Giải pháp này không thay thế hoàn toàn Infrastructure as Code, nhưng bổ sung rất tốt cho các tình huống doanh nghiệp cần xử lý bucket legacy, migrate sang Region mới, đáp ứng yêu cầu tuân thủ dữ liệu hoặc chuẩn hóa cấu hình ở quy mô lớn.

Thông qua bài viết này, mình thấy rõ một bài học quan trọng trong thiết kế hệ thống Cloud: tự động hóa không chỉ dành cho việc deploy ứng dụng, mà còn cần thiết trong cả quản trị cấu hình, bảo mật và vận hành hạ tầng lâu dài.

Một hệ thống Cloud tốt không chỉ là hệ thống chạy được, mà phải là hệ thống có thể lặp lại, kiểm soát được, giám sát được và giảm thiểu tối đa lỗi thao tác thủ công.

![alt text](/my-hugo-site/images/3-Blogpost/khoa.png)

Link tài liệu:
https://aws.amazon.com/.../replicate-amazon-s3-bucket.../...