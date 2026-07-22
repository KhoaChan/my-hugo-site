---
title: "Worklog Tuần 8"
date: 2026-06-05
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---


## I. Tóm tắt tổng quan

Sau khi hoàn thành thiết kế kiến trúc tổng thể của hệ thống ở tuần trước, trong tuần 8 tôi tiếp tục chuyển sang giai đoạn hoàn thiện thiết kế kỹ thuật trước khi bắt đầu các hoạt động triển khai đầu tiên của đề tài thực tập. Mục tiêu chính của tuần này là rà soát lại toàn bộ kiến trúc, phân tích chi tiết hơn từng thành phần trong hệ thống và xác định cách từng module có thể được triển khai trong quá trình phát triển.

Trong quá trình nghiên cứu, tôi tiếp tục tham khảo AWS Documentation, AWS Prescriptive Guidance và một số mô hình kiến trúc Video-on-Demand trên AWS để đánh giá liệu kiến trúc đã lựa chọn có phù hợp hay không. Thông qua quá trình tự rà soát và phân tích, tôi điều chỉnh một số thành phần nhằm cải thiện khả năng mở rộng, giảm sự phụ thuộc giữa các dịch vụ và giúp quá trình triển khai thực tế hơn.

Đồng thời, tôi bắt đầu thiết kế mô hình dữ liệu cho Amazon DynamoDB, xây dựng danh sách các API nghiệp vụ dự kiến triển khai thông qua Amazon API Gateway và AWS Lambda, đồng thời xác định luồng dữ liệu giữa các thành phần trong hệ thống.

Kết thúc tuần này, tôi đã hoàn thành các tài liệu thiết kế kỹ thuật cơ bản, tạo nền tảng để chuyển sang giai đoạn triển khai hệ thống trong tuần tiếp theo.

---

## II. Mục tiêu chiến lược trong tuần

Sau khi đã xác định kiến trúc tổng thể, mục tiêu của tuần 8 là cụ thể hóa các thành phần kỹ thuật nhằm chuẩn bị cho quá trình phát triển hệ thống.

Các mục tiêu chính bao gồm:

- Rà soát và hoàn thiện kiến trúc tổng thể dựa trên quá trình nghiên cứu và phân tích.
- Thiết kế mô hình dữ liệu cho Amazon DynamoDB.
- Xác định các API nghiệp vụ và phương thức giao tiếp giữa Frontend và Backend.
- Chuẩn bị cấu trúc dự án và chia hệ thống thành các module chức năng.
- Xây dựng kế hoạch triển khai chi tiết cho từng giai đoạn phát triển.
- Đảm bảo các quyết định thiết kế đáp ứng yêu cầu về khả năng mở rộng, hiệu năng và chi phí vận hành.

Thông qua các mục tiêu trên, tôi hướng đến việc tạo ra một nền tảng kỹ thuật thống nhất trước khi bắt đầu các hoạt động phát triển đầu tiên của hệ thống.

---

## III. Nhật ký hoạt động & Lộ trình phân bổ chi tiết (Từ 09/06/2026 đến 15/06/2026)

| Thời gian | Danh mục hoạt động | Chi tiết các tác vụ thực hiện | Kết quả/Minh chứng đạt được |
| --- | --- | --- | --- |
| Ngày 1 (09/06) | Rà soát kiến trúc | Rà soát Architecture Draft V1, kiểm tra luồng dữ liệu, vai trò của từng dịch vụ AWS và mối quan hệ giữa các thành phần trong hệ thống. | Hoàn thành phiên bản kiến trúc đã được cải thiện để phục vụ giai đoạn thiết kế kỹ thuật. |
| Ngày 2 (10/06) | Thiết kế cơ sở dữ liệu | Thiết kế mô hình dữ liệu trên Amazon DynamoDB, xác định Partition Key, Sort Key và các trường metadata cần thiết cho bản ghi video. | Hoàn thành thiết kế dữ liệu ban đầu cho hệ thống. |
| Ngày 3 (11/06) | Thiết kế API | Xác định các API cần triển khai, bao gồm HTTP method, cấu trúc Request/Response và luồng xác thực dự kiến thông qua Amazon Cognito. | Hoàn thành danh sách API cho các chức năng chính của hệ thống. |
| Ngày 4 (12/06) | Thiết kế quy trình xử lý | Phân tích các luồng Video Upload, Video Processing và Video Streaming; xác định dữ liệu trao đổi giữa Amazon S3, SQS, Lambda, MediaConvert và DynamoDB. | Hoàn thành luồng xử lý nghiệp vụ của hệ thống. |
| Ngày 5 (13/06) | Chuẩn bị môi trường phát triển | Thiết lập cấu trúc dự án, tổ chức thư mục mã nguồn và chuẩn bị môi trường phát triển cho cả Backend và Frontend. | Hoàn thành cấu trúc dự án ban đầu. |
| Ngày 6 (14/06) | Lập kế hoạch triển khai | Chia hệ thống thành các module chức năng như Authentication, Video Upload, Video Processing và Video Streaming; xây dựng kế hoạch phát triển theo từng giai đoạn. | Hoàn thành roadmap triển khai chi tiết cho các tuần tiếp theo. |
| Ngày 7 (15/06) | Rà soát và tổng kết thiết kế | Rà soát toàn bộ tài liệu kỹ thuật, kiểm tra tính thống nhất của kiến trúc, API, mô hình dữ liệu và định hướng triển khai. | Hoàn thành tài liệu thiết kế kỹ thuật và chuẩn bị cho giai đoạn lập trình. |

---

## IV. Thực thi kỹ thuật chuyên sâu & Phân tích chi tiết

Sau khi hoàn thành thiết kế kiến trúc tổng thể ở tuần trước, tôi tiếp tục tập trung vào việc hoàn thiện các thành phần kỹ thuật ở mức thiết kế để chuẩn bị cho giai đoạn triển khai. Mục tiêu của tuần này chưa phải là phát triển các chức năng cụ thể, mà là chuyển đổi kiến trúc tổng quan thành các tài liệu kỹ thuật có thể sử dụng trực tiếp trong quá trình lập trình.

Trong quá trình nghiên cứu, tôi tiếp tục tham khảo tài liệu chính thức của AWS, AWS Well-Architected Framework và một số mô hình Video-on-Demand mã nguồn mở để đánh giá mức độ phù hợp của kiến trúc. Bằng cách so sánh nhiều hướng triển khai khác nhau, tôi điều chỉnh một số thành phần nhằm đơn giản hóa luồng xử lý, giảm sự phụ thuộc giữa các dịch vụ và đảm bảo khả năng mở rộng trong tương lai.

Một trong những nhiệm vụ quan trọng của tuần này là thiết kế mô hình dữ liệu cho hệ thống. Sau khi phân tích yêu cầu nghiệp vụ, tôi tiếp tục lựa chọn Amazon DynamoDB làm cơ sở dữ liệu chính để lưu trữ metadata của video. Các trường dữ liệu dự kiến bao gồm Video ID, Video Name, Upload Time, Processing Status, Owner, Category và đường dẫn đến các file video đã xử lý.

Bên cạnh đó, tôi bước đầu nghiên cứu việc sử dụng Global Secondary Indexes (GSI) để hỗ trợ truy vấn theo trạng thái xử lý hoặc danh mục video. Việc thiết kế mô hình dữ liệu dựa trên access pattern ngay từ đầu giúp giảm nhu cầu thay đổi cấu trúc bảng khi hệ thống mở rộng.

Đồng thời, tôi xác định danh sách các API sẽ được triển khai trong giai đoạn đầu của đề tài. Các API được chia thành nhiều nhóm, bao gồm Authentication, Video Management và Video Streaming. Với mỗi API, tôi xác định HTTP method, dữ liệu đầu vào, dữ liệu phản hồi và cơ chế xác thực dự kiến thông qua Amazon Cognito.

Đối với workflow xử lý video, tôi tiếp tục phân tích luồng dữ liệu giữa Amazon S3, Amazon SQS, AWS Lambda và AWS Elemental MediaConvert. Thay vì để Backend trực tiếp xử lý toàn bộ quá trình upload và chuyển đổi video, tôi giữ định hướng xử lý bất đồng bộ nhằm giảm tải cho API và cải thiện khả năng mở rộng của hệ thống.

Ngoài việc thiết kế mô hình dữ liệu và API, tôi cũng bắt đầu xây dựng cấu trúc thư mục dự án để tạo sự thống nhất trong quá trình triển khai. Backend và Frontend được tách thành hai phần độc lập, giúp việc phát triển và kiểm thử từng module trở nên dễ dàng hơn. Tôi cũng xác định rõ ranh giới giữa các module như Authentication, Video Upload, Video Processing và Video Streaming để chuẩn bị cho giai đoạn triển khai.

Kết thúc tuần này, mặc dù chưa bắt đầu lập trình các chức năng cụ thể, tôi đã hoàn thành phần lớn các tài liệu thiết kế kỹ thuật cần thiết. Đây là nền tảng quan trọng giúp giảm các thay đổi lớn trong quá trình phát triển và hỗ trợ triển khai hệ thống ở các tuần tiếp theo.

---

## V. Thách thức hạ tầng, Nhật ký xử lý lỗi & Góc nhìn chuyên môn

Trong quá trình hoàn thiện thiết kế kỹ thuật, tôi nhận ra rằng xây dựng một hệ thống Serverless không chỉ đơn giản là lựa chọn các dịch vụ AWS. Việc này còn yêu cầu xác định rõ ranh giới trách nhiệm giữa từng thành phần. Nếu không có một thiết kế thống nhất ngay từ đầu, việc phát triển các module độc lập có thể dẫn đến sự không tương thích giữa Backend, Database và workflow xử lý event.

Một trong những khó khăn tôi tập trung phân tích là cách tổ chức dữ liệu trong Amazon DynamoDB. Khác với cơ sở dữ liệu quan hệ, DynamoDB yêu cầu thiết kế dựa trên các mẫu truy vấn thay vì chỉ chuẩn hóa dữ liệu. Điều này khiến tôi phải dành thời gian xác định Partition Key, Sort Key, các thuộc tính cần đánh index và chiến lược mở rộng dữ liệu trong tương lai.

Ngoài ra, việc xây dựng danh sách API cũng đặt ra một số câu hỏi liên quan đến tính nhất quán của giao diện lập trình. Tôi quyết định sử dụng hướng RESTful API và xác định quy ước đặt tên endpoint, cấu trúc JSON Request/Response và HTTP status code nhằm tạo sự rõ ràng trước khi bắt đầu lập trình.

Bên cạnh các vấn đề về thiết kế, tôi cũng chú trọng đến khả năng mở rộng trong tương lai của hệ thống. Thay vì chỉ thiết kế cho các chức năng hiện tại, tôi cố gắng xây dựng kiến trúc có thể hỗ trợ thêm các tính năng như Video Recommendation, Comment hoặc Playlist mà không cần thay đổi lớn đối với các thành phần hiện có.

Thông qua quá trình nghiên cứu và phân tích, tôi nhận thấy rằng việc đầu tư thời gian vào giai đoạn thiết kế có thể giảm đáng kể rủi ro kỹ thuật trong quá trình triển khai và tạo nền tảng ổn định cho việc phát triển các chức năng của hệ thống.

---

## VI. Đánh giá và Chiêm nghiệm chuyên môn

Tuần 8 giúp tôi hiểu rõ hơn vai trò của giai đoạn thiết kế trong quá trình phát triển một hệ thống Cloud. Mặc dù tôi chưa trực tiếp xây dựng các chức năng nghiệp vụ, việc phân tích dữ liệu, thiết kế API và xác định luồng xử lý giữa các dịch vụ AWS đã giúp tôi hình dung rõ hơn cách các thành phần sẽ phối hợp với nhau trong quá trình triển khai.

Thông qua việc tham khảo tài liệu chính thức của AWS và các dự án thực tế, tôi nhận ra rằng một kiến trúc tốt không chỉ đáp ứng yêu cầu hiện tại mà còn cần hỗ trợ việc mở rộng và bảo trì trong tương lai. Những quyết định được đưa ra trong giai đoạn thiết kế sẽ ảnh hưởng trực tiếp đến hiệu năng hệ thống, chi phí vận hành và khả năng phát triển về sau.

Ngoài ra, tuần làm việc này còn giúp tôi cải thiện kỹ năng phân tích yêu cầu, thiết kế giải pháp và tổ chức tài liệu kỹ thuật. Việc thiết lập các quy ước kỹ thuật trước khi lập trình là cần thiết để giảm các thay đổi lớn khi đề tài bước vào giai đoạn phát triển.

Nhìn chung, mặc dù chưa xây dựng chức năng cụ thể trong tuần này, tôi đã tạo được một nền tảng thiết kế kỹ thuật tương đối hoàn chỉnh và sẵn sàng chuyển sang triển khai các thành phần đầu tiên của hệ thống.

---

## VII. Kế hoạch chiến lược & Lộ trình tối ưu cho tuần tiếp theo

Trong tuần tiếp theo, tôi dự kiến chính thức bắt đầu giai đoạn triển khai dựa trên các tài liệu kỹ thuật đã xây dựng trong hai tuần vừa qua.

Trọng tâm chính của tuần tiếp theo sẽ là chuẩn bị hạ tầng AWS phục vụ phát triển, bao gồm cấu hình các dịch vụ cơ bản như Amazon Cognito, Amazon S3, Amazon DynamoDB, Amazon API Gateway và AWS Lambda. Đồng thời, tôi sẽ xây dựng các module nền tảng đầu tiên của hệ thống như xác thực người dùng, kết nối cơ sở dữ liệu và các API cơ bản phục vụ quản lý video.

Song song với việc phát triển Backend, tôi cũng sẽ kiểm thử từng thành phần ngay sau khi hoàn thành để đảm bảo tính ổn định trước khi tích hợp với các module khác. Việc triển khai hệ thống theo từng bước nhỏ sẽ giúp tôi dễ phát hiện vấn đề hơn và điều chỉnh kiến trúc khi cần thiết mà không ảnh hưởng đến toàn bộ hệ thống.

Mục tiêu của tuần tiếp theo là hoàn thành nền tảng kỹ thuật ban đầu của hệ thống, tạo cơ sở để phát triển workflow Video Upload và Video Processing Pipeline trong các giai đoạn tiếp theo của đề tài.