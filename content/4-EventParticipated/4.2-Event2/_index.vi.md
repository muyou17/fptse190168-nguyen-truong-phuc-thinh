---
title : "Event 2"
date :  "2025-09-09" 
weight : 2 
chapter : false
pre : " <b> 4.2 </b> "
---


# Báo cáo tổng hợp: “DevOps trên AWS”

## Mục tiêu Sự kiện
* Hiểu các nguyên tắc cốt lõi của văn hóa DevOps và các chỉ số hiệu suất chính (DORA, MTTR).
* Nắm vững chuỗi công cụ CI/CD của AWS để tự động hóa việc xây dựng (build), kiểm thử (test) và triển khai (deployment).
* Tìm hiểu các khái niệm Cơ sở hạ tầng dưới dạng Mã (IaC) sử dụng CloudFormation và AWS CDK.
* Khám phá các chiến lược container hóa sử dụng ECR, ECS, EKS và App Runner.
* Triển khai khả năng quan sát toàn diện (full-stack observability) và giám sát với CloudWatch và X-Ray.

## Diễn giả
* **Các Chuyên gia DevOps của AWS**
* **Các Kiến trúc sư Giải pháp Cấp cao**

## Điểm nhấn Chính

### Văn hóa & Tư duy DevOps
* **Tóm tắt:** Tích hợp với các khái niệm AI/ML từ các phiên trước.
* **Các chỉ số quan trọng:** Tập trung vào Tần suất triển khai, Thời gian thực hiện thay đổi, Thời gian trung bình để khôi phục (MTTR) và Tỷ lệ lỗi khi thay đổi (các chỉ số DORA).
* **Chuyển đổi văn hóa:** Chuyển từ các nhóm làm việc riêng lẻ (siloed) sang mô hình chia sẻ trách nhiệm.

### Đường ống CI/CD trên AWS
* **Kiểm soát nguồn (Source Control):** Sử dụng AWS CodeCommit và triển khai các chiến lược Git như GitFlow và Trunk-based.
* **Xây dựng & Kiểm thử:** Cấu hình AWS CodeBuild để kiểm thử và biên dịch tự động.
* **Triển khai:** Sử dụng AWS CodeDeploy để thực hiện các chiến lược triển khai an toàn:
* **Blue/Green:** Giảm thiểu thời gian ngừng hoạt động và rủi ro.
* **Canary:** Triển khai dần dần cho một nhóm nhỏ người dùng.
* **Rolling:** Cập nhật các instance theo từng bước.
* **Điều phối:** Kết nối tất cả các khâu với AWS CodePipeline

### Cơ sở hạ tầng dưới dạng Mã (IaC)
* **AWS CloudFormation:** Định nghĩa hạ tầng bằng template, stack và quản lý sự sai lệch cấu hình (drift).
* **AWS CDK (Cloud Development Kit):** Sử dụng ngôn ngữ lập trình quen thuộc để định nghĩa tài nguyên đám mây dưới dạng các cấu trúc mã (constructs).
* **So sánh:** Lựa chọn giữa template khai báo (CloudFormation) và mã mệnh lệnh (CDK) dựa trên kỹ năng của đội ngũ.

### Dịch vụ Container & Khả năng quan sát
* **Quản lý Container:** Lưu trữ image trong Amazon ECR với các chính sách vòng đời.
* **Điều phối:** Lựa chọn giữa Amazon ECS (đơn giản, native AWS) và **Amazon EKS** (chuẩn Kubernetes), hoặc AWS App Runner để triển khai đơn giản kiểu PaaS.
* **Giám sát:** Sử dụng Amazon CloudWatch cho các chỉ số/cảnh báo và AWS X-Ray cho truy vết phân tán để xác định điểm nghẽn hiệu năng.

## Bài học Chính (Key Takeaways)

### Chiến lược DevOps
* **Tự động hóa là ưu tiên:** Triển khai thủ công dễ gặp lỗi; mọi thứ từ hạ tầng đến triển khai mã nguồn cần được tự động hóa.
* **Đo lường:** Bạn không thể cải thiện những gì bạn không đo lường. Sử dụng chỉ số DORA để theo dõi tốc độ và sự ổn định.
* **Dịch chuyển sang trái (Shift Left):** Tích hợp kiểm thử và bảo mật sớm trong pipeline CI/CD, thay vì để ở cuối quy trình.

### Kiến trúc Kỹ thuật
* **Hạ tầng bất biến (Immutable Infrastructure):** Coi máy chủ là tài nguyên dùng một lần; thay thế chúng thay vì vá lỗi tại chỗ.
* **Container hóa:** Tách biệt ứng dụng khỏi hệ điều hành bên dưới để đảm bảo tính nhất quán giữa các môi trường (Dev, Test, Prod).
* **Khả năng quan sát:** Vượt ra ngoài việc giám sát "bật/tắt" đơn giản để có thông tin sâu sắc bằng cách sử dụng truy vết phân tán.

### Ứng dụng vào Công việc
* **Triển khai CI/CD:** Thiết lập CodePipeline cho các dự án hiện tại để tự động hóa quy trình build/deploy cho ứng dụng Spring Boot/React.
* **Áp dụng IaC:** Bắt đầu định nghĩa tài nguyên AWS (database, S3 bucket, Cognito User Pools) bằng AWS CDK thay vì dùng console.
* **Container hóa:** Dockerize các microservices hiện có và đẩy image lên ECR.
* **Nâng cao giám sát:** Thêm X-Ray vào các dịch vụ backend để trực quan hóa độ trễ API và hiệu năng truy vấn cơ sở dữ liệu.

## Trải nghiệm Sự kiện

Tham dự hội thảo “DevOps on AWS” đã cung cấp một lộ trình thực tế để tự động hóa vòng đời phân phối phần mềm. Nó thu hẹp khoảng cách giữa việc viết code và chạy nó một cách đáng tin cậy trên môi trường production (thực tế). Các trải nghiệm chính bao gồm:

### Học hỏi từ chuyên gia
* Làm rõ "Ma trận thuật ngữ" của các công cụ AWS (CodeCommit, CodeBuild, CodeDeploy, CodePipeline) và cách chúng tích hợp.
* Hiểu giá trị chiến lược của chỉ số DORA trong việc chứng minh hiệu quả đầu tư DevOps với các bên liên quan.

### Tiếp cận kỹ thuật thực tế
* **CI/CD Walkthrough:** Bản demo pipeline đầy đủ cho thấy chính xác cách thay đổi code kích hoạt build và deploy tự động.
* **IaC Implementation:** Xem AWS CDK hoạt động là một điểm nhấn—viết hạ tầng bằng Java/TypeScript trực quan hơn nhiều cho lập trình viên so với viết template JSON/YAML.
* **Deployment Strategies:** Trực quan hóa Blue/Green deployments đã chứng minh cách phát hành bản cập nhật mà không có thời gian chết (zero downtime).

### Kết nối và thảo luận
* Thảo luận về sự đánh đổi giữa ECS và EKS với đồng nghiệp, nhận ra rằng đối với nhiều dự án, ECS hoặc App Runner cung cấp con đường nhanh hơn để ra production với ít chi phí vận hành hơn.
* Trao đổi ý tưởng về cách xử lý database migrations (di chuyển dữ liệu) trong một pipeline CI/CD.

### Bài học rút ra
* **Phát hiện sai lệch (Drift Detection)** trong CloudFormation là rất quan trọng để duy trì tính toàn vẹn của hạ tầng.
* **Khả năng quan sát (Observability)** không phải là tùy chọn đối với microservices; nếu không có X-Ray, việc gỡ lỗi kiến trúc phân tán gần như là không thể.
* Một nền tảng DevOps vững chắc giúp giảm đáng kể thời gian trung bình để khôi phục (MTTR), cho phép các team đổi mới nhanh hơn mà ít sợ làm hỏng hệ thống production.

### Ảnh sự kiện
![Photo](/images/aws2.jpg)
