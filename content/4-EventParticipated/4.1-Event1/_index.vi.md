---
title : "Event 1"
date :  "2025-09-09" 
weight : 1 
chapter : false
pre : " <b> 4.1 </b> "
---

# Báo cáo tổng hợp: “Hội thảo Dịch vụ AWS AI/ML & Generative AI”

## Mục tiêu Sự kiện
* Cung cấp cái nhìn tổng quan về bức tranh AI/ML tại Việt Nam.
* Minh họa quy trình máy học (Machine Learning) toàn diện sử dụng Amazon SageMaker.
* Giới thiệu các khả năng của Generative AI với Amazon Bedrock (Mô hình nền tảng, Agents, Guardrails).
* Chia sẻ các kỹ thuật về Kỹ thuật tạo câu lệnh (Prompt Engineering) và Tạo nội dung tăng cường truy xuất (RAG).

## Diễn giả
* **Các Kiến trúc sư giải pháp (Solutions Architects)**
* **Các Chuyên gia về AI/ML**

## Điểm nhấn Chính

### Tổng quan Dịch vụ AWS AI/ML (SageMaker)
* **Nền tảng Toàn diện:** Bao phủ toàn bộ vòng đời từ chuẩn bị dữ liệu đến triển khai.
    * **Chuẩn bị dữ liệu:** Các chiến lược gán nhãn và làm sạch dữ liệu.
    * **Huấn luyện & Tinh chỉnh:** Tối ưu hóa mô hình về hiệu năng và chi phí.
    * **Triển khai:** Đưa mô hình lên môi trường production (thực tế) một cách hiệu quả.
* **MLOps Tích hợp:** Nêu bật khả năng tự động hóa và chuẩn hóa các đường ống (pipeline) ML.
* **SageMaker Studio:** Demo trực tiếp giao diện thống nhất để xây dựng, huấn luyện và triển khai mô hình.

### Generative AI với Amazon Bedrock
* **Mô hình Nền tảng (FMs):** Hướng dẫn so sánh và lựa chọn các mô hình hàng đầu:
    * **Claude:** Khả năng suy luận cao.
    * **Llama:** Mã nguồn mở và hiệu quả.
    * **Titan:** Tích hợp nguyên bản (native) với AWS.
* **Prompt Engineering (Kỹ thuật Prompt):**
    * **Chain-of-Thought (Chuỗi suy luận):** Chia nhỏ các tác vụ suy luận phức tạp.
    * **Few-shot learning (Học từ vài mẫu):** Sử dụng các ví dụ mẫu để cải thiện kết quả đầu ra.
* **Kiến trúc Nâng cao:**
    * **RAG (Retrieval-Augmented Generation):** Tích hợp Knowledge Bases (Cơ sở tri thức) để căn cứ câu trả lời dựa trên dữ liệu doanh nghiệp.
    * **Bedrock Agents:** Tạo quy trình làm việc đa bước và tích hợp với các công cụ bên ngoài.
    * **Guardrails (Bộ lọc an toàn):** Đảm bảo an toàn và lọc nội dung.

## Bài học Chính (Key Takeaways)

### Tư duy Thiết kế
* **Lựa chọn Mô hình:** Chọn Mô hình nền tảng phù hợp (ví dụ: Claude vs. Titan) dựa trên yêu cầu cụ thể của trường hợp sử dụng (tốc độ vs. khả năng suy luận).
* **An toàn là trên hết:** Việc triển khai Guardrails là rất quan trọng đối với ứng dụng AI có trách nhiệm trong môi trường doanh nghiệp.

### Kiến trúc Kỹ thuật
* **Ưu tiên RAG hơn Fine-tuning:** Đối với hầu hết các trường hợp sử dụng kiến thức nội bộ, RAG mang lại giải pháp linh hoạt và tiết kiệm chi phí hơn so với việc tinh chỉnh mô hình.
* **Quy trình làm việc Agentic:** Chuyển từ chatbot thụ động sang các Agent chủ động có thể thực thi tác vụ là biên giới tiếp theo của GenAI.

### Chiến lược Hiện đại hóa
* **Áp dụng MLOps:** Chuyển từ huấn luyện mô hình thủ công sang các pipeline tự động (MLOps) là điều cần thiết để mở rộng quy mô.
* **Bối cảnh Địa phương:** Hiểu rõ các xu hướng AI/ML cụ thể tại Việt Nam giúp đối chiếu tiêu chuẩn cho các dự án trong nước.

## Ứng dụng vào Công việc

* **Thử nghiệm SageMaker:** Đánh giá quy trình ML hiện tại và xác định cơ hội chuyển đổi sang **Amazon SageMaker** để quản lý vòng đời tốt hơn.
* **Xây dựng Bot Kiến thức:** Tạo nguyên mẫu sử dụng Amazon Bedrock và **RAG** để tra cứu tài liệu nội bộ hoặc hướng dẫn kỹ thuật.
* **Cải thiện Prompt:** Áp dụng ngay kỹ thuật Chain-of-Thought và **Few-shot** để nâng cao độ chính xác của các tương tác AI hiện tại.
* **Triển khai Guardrails:** Cấu hình bộ lọc nội dung trên Bedrock để đảm bảo an toàn thương hiệu cho các ứng dụng thử nghiệm.

## Trải nghiệm Sự kiện

Tham dự hội thảo **“AWS AI/ML Services & Generative AI”** đã cung cấp một lộ trình thực tế để áp dụng các dịch vụ thông minh. Những trải nghiệm chính bao gồm:

### Học hỏi từ chuyên gia
* Hiểu rõ hơn về bức tranh AI/ML tại Việt Nam, nắm bắt các cơ hội và thách thức tại địa phương.
* Đào sâu kiến thức kỹ thuật về sự khác biệt giữa các Mô hình nền tảng lớn (**Claude, Llama, Titan**).

### Tiếp cận kỹ thuật thực tế
* Phần hướng dẫn SageMaker Studio đã minh họa cách thống nhất chuỗi công cụ ML rời rạc vào một giao diện quản lý duy nhất.
* Demo trực tiếp về Bedrock đã cho thấy việc triển khai thực tế một chatbot GenAI, giải mã sự phức tạp của RAG và Agents.

### Kết nối và thảo luận
* Hoạt động ice-breaker và các phiên networking cho phép trao đổi ý tưởng với đồng nghiệp về những thách thức thực tế khi triển khai GenAI.
* Các cuộc thảo luận củng cố tầm quan trọng của Prompt Engineering như một kỹ năng quan trọng cho phát triển hiện đại.

### Bài học rút ra
* **RAG** là chìa khóa để làm cho LLM hữu ích với dữ liệu đặc thù của doanh nghiệp mà không tốn chi phí huấn luyện cao.
* **Guardrails** không phải là tùy chọn; chúng là một lớp nền tảng của ngăn xếp (stack) Generative AI.
* Hiệu quả trong ML đến từ MLOps tích hợp thay vì các thử nghiệm khoa học dữ liệu riêng lẻ.

### Ảnh sự kiện
![Photo](/images/aws1.jpg)