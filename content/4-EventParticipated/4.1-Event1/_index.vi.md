---
title: "Event 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---


# Event Report: FCAJ Community Day Workshop


## I. Thông tin sự kiện

**Tên sự kiện:** FCAJ Community Day Workshop
**Thời gian:** 09:00 AM – 12:00 PM, Thứ Bảy, ngày 23/05/2026
**Địa điểm:** Tầng 26, Bitexco Financial Tower, 02 Hải Triều, Phường Bến Nghé, Quận 1, TP. Hồ Chí Minh
**Vai trò tham gia:** Người tham dự
**Người thực hiện báo cáo:** Khoa – Sinh viên năm cuối ngành Công nghệ phần mềm, HUTECH

---

## II. Tổng quan sự kiện

Trong quá trình tham gia chương trình **First Cloud AI Journey**, tôi có cơ hội tham dự sự kiện **FCAJ Community Day Workshop**. Đây là một buổi chia sẻ kỹ thuật có giá trị, tập trung vào các chủ đề liên quan đến **AI trong môi trường production**, **Amazon Q**, **CloudFront**, **bảo mật hệ thống**, **multi-agent architecture**, và các bài học thực tế từ quá trình xây dựng sản phẩm công nghệ.

Sự kiện giúp tôi mở rộng góc nhìn về cách các hệ thống AI và cloud được triển khai trong môi trường doanh nghiệp. Nếu ở trường học, nhiều dự án chỉ cần chạy đúng chức năng cơ bản, thì qua workshop này tôi nhận ra rằng hệ thống thực tế cần quan tâm nhiều hơn đến bảo mật, chi phí, khả năng mở rộng, độ ổn định, chất lượng dữ liệu đầu vào và cách kiểm soát rủi ro khi ứng dụng AI.

---

## III. Mục tiêu của sự kiện

Các mục tiêu chính của sự kiện bao gồm:

* Chia sẻ các best practices khi thiết kế và triển khai ứng dụng hiện đại trên nền tảng AWS Cloud.
* Giới thiệu cách đưa AI vào môi trường production một cách an toàn và hiệu quả.
* Trình bày các phương pháp tối ưu context khi sử dụng Large Language Models.
* Giới thiệu Amazon Q và các ứng dụng AI Agent trong doanh nghiệp.
* Phân tích cách tối ưu hiệu năng và bảo mật với Amazon CloudFront.
* Chia sẻ kinh nghiệm phát triển sản phẩm trong thời gian ngắn thông qua case study hackathon.
* Giải thích các yếu tố gây ra tính không xác định trong mô hình ngôn ngữ lớn.
* Trình bày kiến trúc multi-agent trong bài toán chấm điểm tín dụng doanh nghiệp.

---

## IV. Danh sách diễn giả

Các diễn giả tham gia sự kiện gồm:

* **Ms. Vy Lam** – Senior Business Systems Analyst, VPBank.
* **Ms. Thao Nguyen** – GenAI Engineer, VIB.
* **Ms. Mai Nguyen** – GenAI Engineer, VIB.
* **Ms. Uyen Le** – GenAI Engineer, VIB.
* **Mr. Tinh Truong** – Platform Engineer, GoTymeX.
* **Mr. Thinh Nguyen** – DevOps Engineer, FCAJ.
* **Mr. Duc Dao** – Solutions Architect, Cloud Kinetics.
* **Mr. Pham Ng. Hai Anh** – Cloud Consultant, G-AsiaPacific Vietnam.

---

# V. Nội dung kỹ thuật chính

---

## 1. Context Is Everything – Bringing AI to Production

Phần trình bày của **Mr. Tinh Truong** tập trung vào một vấn đề rất quan trọng khi đưa AI vào môi trường production: **Context Engineering**. Thay vì chỉ quan tâm đến việc chọn mô hình mạnh hơn, diễn giả nhấn mạnh rằng chất lượng đầu ra của AI phụ thuộc rất nhiều vào cách cung cấp ngữ cảnh.

Một context tốt cần có các yếu tố chính:

* **Goal:** mục tiêu rõ ràng của tác vụ.
* **Situation:** bối cảnh thực tế mà hệ thống đang xử lý.
* **Constraints:** các giới hạn kỹ thuật hoặc nghiệp vụ.
* **Evidence:** dữ liệu, tài liệu hoặc mã nguồn liên quan.

Diễn giả cũng chỉ ra một số lỗi phổ biến khi sử dụng AI trong doanh nghiệp, ví dụ như đưa quá nhiều tài liệu thô vào prompt, lặp lại các kiến thức mà mô hình đã biết, hoặc không cung cấp ràng buộc kỹ thuật cụ thể. Những lỗi này có thể làm mô hình trả lời chung chung, sai ngữ cảnh hoặc tạo ra kết quả không dùng được trong hệ thống thực tế.

Qua phần này, tôi hiểu rằng muốn sử dụng AI hiệu quả trong dự án thật, người dùng cần biết cách thiết kế context, chọn đúng dữ liệu liên quan và đưa ra yêu cầu rõ ràng thay vì chỉ viết prompt ngắn và mong AI tự hiểu toàn bộ vấn đề.

---

## 2. Friendly AI Assistant with Amazon Q

Phần trình bày của **Mr. Pham Ng. Hai Anh** giới thiệu về **Amazon Q** và cách AI Agent có thể hỗ trợ nhân sự trong doanh nghiệp, đặc biệt là những người không chuyên sâu về kỹ thuật.

Amazon Q có thể kết nối với nhiều nguồn dữ liệu khác nhau như Amazon S3, cơ sở dữ liệu nội bộ và các công cụ doanh nghiệp. Nhờ đó, hệ thống có thể tạo ra một AI Assistant giúp người dùng tra cứu thông tin, tổng hợp nội dung và tự động hóa một số công việc hằng ngày.

Một ví dụ thực tế được trình bày là **PM Assistant**, hỗ trợ Project Manager trong việc ghi nhận nội dung cuộc họp, tạo biên bản họp, soạn email gửi stakeholder và cập nhật lịch trình công việc.

Điểm tôi ấn tượng là Amazon Q không chỉ đơn thuần là chatbot trả lời câu hỏi. Khi được cấu hình với dữ liệu, quyền truy cập và guardrails phù hợp, nó có thể trở thành một trợ lý doanh nghiệp an toàn, có kiểm soát và hữu ích trong quy trình vận hành.

---

## 3. Maximizing Performance and Security with CloudFront

Phần trình bày của **Mr. Thinh Nguyen** tập trung vào cách sử dụng **Amazon CloudFront** để tăng hiệu năng, giảm độ trễ và bảo vệ hệ thống trước các rủi ro như traffic spike hoặc DDoS.

Nội dung chính bao gồm:

* Sử dụng mạng lưới edge locations của CloudFront để đưa nội dung đến gần người dùng hơn.
* Kết hợp CloudFront với AWS WAF, AWS Shield, Route 53 và S3 để tăng cường bảo mật.
* Che giấu origin server để hạn chế việc lộ IP thật của hệ thống.
* Tối ưu giao thức như HTTP/3, QUIC/UDP để cải thiện tốc độ truyền tải.
* Sử dụng CloudFront Functions hoặc Lambda@Edge để xử lý logic ở tầng edge.

Qua phần này, tôi hiểu rõ hơn rằng CDN không chỉ dùng để tăng tốc website. Trong hệ thống production, CloudFront còn là một lớp bảo vệ quan trọng, giúp giảm tải origin, chống tấn công ở tầng biên và tối ưu chi phí vận hành.

---

## 4. UTMorpho – From Concept to Product in 36 Hours

Phần trình bày từ đội ngũ VIB Engineering Team chia sẻ case study về sản phẩm **UTMorpho** tại LotusHacks 2026. Đây là một ví dụ thực tế về việc phát triển sản phẩm trong thời gian rất ngắn, chỉ trong 36 giờ.

Ý tưởng của UTMorpho là cho phép người dùng scan bản phác thảo giao diện từ giấy hoặc iPad, sau đó sử dụng AI để tạo ra mã nguồn frontend có thể sử dụng được. Đây là một ứng dụng rất thực tế, giải quyết vấn đề lặp lại trong quá trình chuyển đổi wireframe thành giao diện.

Một số khó khăn được chia sẻ gồm:

* AI tạo ra mã nguồn quá dài hoặc không tối ưu.
* Giới hạn API token khi thử nghiệm liên tục.
* Áp lực thời gian trong giai đoạn hackathon.
* Cần thu hẹp phạm vi để tạo ra sản phẩm chạy được thay vì cố làm quá nhiều tính năng.

Bài học tôi rút ra là khi làm sản phẩm trong thời gian ngắn, việc xác định phạm vi rõ ràng quan trọng hơn việc thêm nhiều chức năng. Một sản phẩm nhỏ nhưng chạy ổn định sẽ tốt hơn một ý tưởng lớn nhưng không hoàn thiện.

---

## 5. Navigating Non-Determinism in Large Language Models

Phần trình bày của **Mr. Duc Dao** giải thích vì sao các mô hình ngôn ngữ lớn không phải lúc nào cũng cho kết quả hoàn toàn giống nhau, ngay cả khi đặt `temperature = 0`.

Một số nguyên nhân chính gồm:

* Tính toán số thực song song trên GPU có thể tạo ra sai số rất nhỏ.
* Thứ tự xử lý trong GPU cluster có thể thay đổi.
* Request batching của nhà cung cấp API có thể làm môi trường thực thi khác nhau.
* Các sai số nhỏ trong logits có thể dẫn đến việc chọn token khác nhau.

Diễn giả cũng đề xuất một số hướng giảm rủi ro như dùng majority voting, schema nghiêm ngặt, JSON mode, function calling hoặc regex constraints.

Phần này giúp tôi hiểu sâu hơn rằng khi xây dựng ứng dụng AI production, không nên giả định mô hình luôn deterministic tuyệt đối. Thay vào đó, cần thiết kế thêm cơ chế kiểm tra, ràng buộc đầu ra và xử lý sai lệch.

---

## 6. Enterprise-Grade Multi-Agent Systems in Corporate Credit Scoring

Phần trình bày của **Ms. Vy Lam** từ VPBank là một nội dung rất ấn tượng về cách áp dụng **Multi-Agent AI System** vào bài toán đánh giá tín dụng doanh nghiệp.

Thay vì sử dụng một AI Agent duy nhất, hệ thống được thiết kế như một hội đồng tín dụng ảo, bao gồm nhiều agent chuyên biệt:

* Manager Agent.
* Financial Analyst Agent.
* Market Analyst Agent.
* Team Evaluator Agent.
* Risk Assessor Agent.

Mỗi agent đảm nhiệm một góc nhìn riêng và có thể kiểm tra chéo kết quả của nhau. Cách thiết kế này giúp tăng tính minh bạch, giảm rủi ro sai lệch và tạo audit trail cho quá trình đánh giá.

Diễn giả cũng nhấn mạnh các yếu tố quan trọng khi đưa AI vào lĩnh vực tài chính như IAM, KMS, secret rotation, data governance, PII masking, network isolation, observability, human-in-the-loop và các tiêu chuẩn tuân thủ như SOC 2, GDPR, PCI DSS.

Qua phần này, tôi hiểu rằng AI trong doanh nghiệp không thể chỉ dừng ở demo. Để đưa vào production, hệ thống cần có kiến trúc bảo mật, giám sát, kiểm soát dữ liệu và quy trình kiểm định rất nghiêm ngặt.

---

# VI. Bài học rút ra

Sau khi tham dự FCAJ Community Day Workshop, tôi rút ra một số bài học quan trọng:

* AI không chỉ phụ thuộc vào model, mà còn phụ thuộc rất nhiều vào context.
* Context Engineering là kỹ năng quan trọng khi xây dựng ứng dụng AI production.
* Amazon Q có thể hỗ trợ mạnh mẽ trong tự động hóa công việc doanh nghiệp.
* CloudFront không chỉ là CDN mà còn là lớp bảo mật và tối ưu hiệu năng quan trọng.
* Khi phát triển sản phẩm, cần kiểm soát phạm vi để tránh làm quá rộng nhưng không hoàn thiện.
* LLM không hoàn toàn deterministic, vì vậy cần có cơ chế kiểm soát đầu ra.
* Multi-Agent Architecture phù hợp với các bài toán phức tạp cần nhiều góc nhìn chuyên môn.
* Bảo mật, governance và observability là các yếu tố bắt buộc khi đưa AI vào hệ thống thật.

---

# VII. Ứng dụng vào học tập và dự án

Những kiến thức từ workshop có thể được áp dụng vào quá trình học tập và đồ án của tôi như sau:

* Áp dụng Context Engineering khi xây dựng chatbot AI.
* Thiết kế prompt có mục tiêu, bối cảnh, ràng buộc và dữ liệu tham chiếu rõ ràng.
* Sử dụng kiến trúc có lớp bảo mật để bảo vệ dữ liệu người dùng.
* Tìm hiểu thêm Amazon Bedrock, Amazon Q và các AI Agent framework.
* Kết hợp CloudFront, WAF và S3 để bảo vệ và tối ưu website.
* Thiết kế hệ thống chatbot theo hướng có kiểm soát đầu ra, tránh hallucination.
* Nghiên cứu Multi-Agent Architecture cho các bài toán tư vấn hoặc phân tích phức tạp.
* Áp dụng tư duy production-ready thay vì chỉ dừng ở mức demo chạy được.

---

# VIII. Hình ảnh minh chứng sự kiện

![FCAJ Community Day Workshop](/my-hugo-site/images/Event/image.png)

---

# IX. Kết luận

FCAJ Community Day Workshop là một sự kiện rất hữu ích đối với tôi trong quá trình học tập về cloud và AI. Thông qua các phần chia sẻ từ nhiều diễn giả có kinh nghiệm, tôi có thêm góc nhìn thực tế về cách xây dựng hệ thống AI và cloud trong môi trường doanh nghiệp.

Điều quan trọng nhất tôi học được là một hệ thống production không chỉ cần chạy được, mà còn phải an toàn, dễ quan sát, tối ưu chi phí, có khả năng mở rộng và kiểm soát được rủi ro. Những kiến thức về Context Engineering, Amazon Q, CloudFront, LLM non-determinism và Multi-Agent Architecture sẽ là nền tảng quan trọng để tôi áp dụng vào các dự án thực tế trong thời gian tới.


