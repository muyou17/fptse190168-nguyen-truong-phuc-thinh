---
title: "Bản đề xuất"
date: "2025-09-09"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---


[Bản Word](/Proposal.docx)


# MealPlan
## Nền tảng Bán Nguyên liệu Thực phẩm Cá nhân hóa
### 1. Tóm tắt điều hành  

Nền tảng Bán Nguyên liệu Thực phẩm Cá nhân hóa tập trung vào việc cho phép mua sắm nhanh hơn và hiệu quả hơn. Người dùng đăng ký tài khoản để truy cập cơ sở dữ liệu công thức đa dạng, nhận đề xuất bữa ăn dựa trên AI dựa trên lịch sử mua hàng và đặt hàng giao tận nhà. Tận dụng cơ sở hạ tầng đám mây AWS, nền tảng đảm bảo khả năng mở rộng linh hoạt, hiệu suất cao và quản lý an toàn.

### 2. Tuyên bố vấn đề  
*Vấn đề hiện tại*  
Khách hàng thường mất nhiều thời gian tìm kiếm những bữa ăn phù hợp với nhu cầu hàng ngày. Mặc dù nhiều nền tảng cung cấp gợi ý về bữa ăn hoặc thực đơn, nhưng hầu hết đều không hỗ trợ mua các món ăn hoàn chỉnh, chế biến sẵn, buộc người dùng phải tự tìm kiếm nhà hàng hoặc nhà cung cấp những bữa ăn đó.

*Giải pháp*  
Nền tảng sử dụng Spring Boot để xây dựng một backend ổn định với các API REST cho tài khoản người dùng, công thức nấu ăn, giỏ hàng và đơn hàng. Frontend được xây dựng bằng React và cung cấp các đề xuất bữa ăn dựa trên AI dễ sử dụng. Dữ liệu được lưu trữ trong AWS RDS (PostgreSQL), trong khi hình ảnh và tệp tĩnh được lưu trữ trên Amazon S3. Backend chạy trên Amazon EC2 bên trong một VPC an toàn và Route 53 được sử dụng để quản lý tên miền.

*Lợi ích và hoàn vốn đầu tư*  
Giải pháp này thiết lập một nền tảng toàn diện cho công ty khởi nghiệp về dinh dưỡng để mở rộng dịch vụ, đồng thời thu thập dữ liệu người dùng cho các hệ thống khuyến nghị nâng cao. Chi phí là 119,51 đô la Mỹ/tháng và 1.434,12 đô la Mỹ/12 tháng. Quá trình phát triển tận dụng các khuôn khổ mã nguồn mở, không phát sinh thêm chi phí phần cứng.

### 3. Kiến trúc giải pháp  

Trang web được host trên EC2. Dữ liệu được lưu trữ bằng EC2 instance. Hình ảnh được lưu trên S3. Code sẽ dược đẩy lên github nhằm quản lý và tự động đẩy code lên s3 để CodeDeploy sẽ thực hiện deploy lên server. Cloudfront được sử dụng nhằm cải thiện tốc tải. Cognito dùng để quản lý danh tính người dùng. CloudTrail được dùng để giám sát và lữu trữ lịch sử hoạt động. CloudWatch dùng để giám sát và quản lý hiệu suất, tình trạng hoạt động của các tài nguyên và ứng dụng trên AWS. IAM dùng để cấp quyền cho các service. SecretManager được dùng nhằm quản lý các thông tin nhạy cảm.

![AWS Architecture](/images/2-Proposal/AWS_Architecture.jpeg)

*Dịch vụ AWS sử dụng*  
- *WAF*: Bảo vệ ứng dụng web khỏi các tấn công mạng
- *AWS CloudFront*: Tăng tốc độ tải trang web.  
- *AWS EC2*: Deploy sản phẩm, NAT instance, Database.
- *AWS VPC*: là mạng ảo.  
- *AWS S3*: Lưu trữ code, file log, hình ảnh.
- *CodeDeploy*: Deploy code lên EC2.
- *GitLab*: chứa source code và push code lên s3.
- *Amazon Cognito*: Quản lý quyền truy cập cho người dùng trang web.  
- *IAM*: Tạo user và role.
- *Secret Manager*: Chứa các thông tin quan trọng.
- *CloudTrail*: Giám sát và lữu trữ lịch sử hoạt động.
- *CloudWatch*: Giám sát và quản lý hiệu suất, tình trạng hoạt động của các tài nguyên và ứng dụng trên AWS

### 4. Triển khai kỹ thuật  
*Các giai đoạn triển khai*  
Các giai đoạn triển khai Dự án này gồm hai phần: phát triển back-end Spring Boot và front-end React, triển khai website lên AWS bằng các dịch vụ AWS, mỗi phần gồm 4 giai đoạn.

1. Xây dựng Lý thuyết và Vẽ Kiến trúc: Thu thập các yêu cầu cho ứng dụng web, thiết kế kiến ​​trúc hệ thống (Spring Boot REST API + React front-end) và định nghĩa lược đồ cơ sở dữ liệu (Tháng 1).

2. Phát triển, Kiểm thử: Triển khai back-end Spring Boot với các REST API (xác thực, quản lý người dùng, CRUD món ăn/công thức, giỏ hàng, v.v.) và xây dựng front-end React (UI/UX, định tuyến, biểu mẫu, quản lý trạng thái). Thực hiện các bài kiểm tra đơn vị cho các dịch vụ back-end, kiểm tra tích hợp cho các điểm cuối API và kiểm tra front-end (Thư viện Kiểm tra Jest/React). (Tháng 1–2)

3. Tính toán Giá và Kiểm tra Tính khả thi: Sử dụng Công cụ Tính giá AWS để ước tính chi phí cho EC2 (lưu trữ back-end), RDS (cơ sở dữ liệu), S3 (tệp và hình ảnh tĩnh), VPC (mạng), Route53 (tên miền) sau đó điều chỉnh nếu cần (Tháng 2).

4. Tích hợp AWS: Tích hợp các dịch vụ AWS vào ứng dụng. Triển khai trang web trên EC2, lưu trữ hình ảnh trên S3, cấu hình RDS cho cơ sở dữ liệu, sử dụng VPC để quản lý mạng, Route53 cho miền và thiết lập các pipeline CI/CD (GitHub Actions hoặc AWS CodePipeline). Thực hiện kiểm thử dàn dựng trước khi phát hành chính thức. (Tháng 3)

*Yêu cầu kỹ thuật*  
1. Back-end (Spring Boot): REST API để xác thực, quản lý người dùng, CRUD món ăn/công thức nấu ăn, giỏ hàng và xử lý đơn hàng. Bao gồm bảo mật (JWT, Spring Security).
2. Front-end (React): Ứng dụng web đáp ứng với UI/UX thân thiện với người dùng, tích hợp API với back-end Spring Boot.
3. Cơ sở dữ liệu (EC2): Cơ sở dữ liệu quan hệ (MySQL/PostgreSQL) để lưu trữ người dùng, công thức nấu ăn, giỏ hàng và dữ liệu đơn hàng được host trên EC2.
4. Lưu trữ (AWS S3): Được sử dụng để lưu trữ hình ảnh do người dùng tải lên 
5. Lưu trữ & Mạng (AWS EC2 & AWS VPC): Ứng dụng được triển khai trên các phiên bản EC2.
6, CI/CD (GitHub Actions hoặc AWS CodePipeline): Quy trình xây dựng, triển khai tự động cho cả back-end và front-end.
Xác thực & Bảo mật: Xác thực JWT và cấu hình HTTPS; AWS Cognito tùy chọn để quản lý quyền truy cập của người dùng.

### 5. Lộ trình & Mốc triển khai  
- Tháng 1: Xây dựng lý thuyết và vẽ kiến ​​trúc (backend Spring Boot + thiết kế frontend React, lược đồ cơ sở dữ liệu). Bắt đầu phát triển ban đầu backend và frontend.

- Tháng 2: Tiếp tục phát triển backend và frontend, thực hiện kiểm thử đơn vị và tích hợp. Sử dụng Công cụ tính giá AWS để đánh giá chi phí lưu trữ và tinh chỉnh kiến ​​trúc nhằm đảm bảo hiệu quả chi phí.

- Tháng 3: Tích hợp các dịch vụ AWS, cấu hình các pipeline CI/CD, tiến hành kiểm thử dàn dựng và triển khai website lên môi trường production.

- Sau khi ra mắt: Tối đa 3 tháng để bảo trì, tối ưu hóa và cải tiến tính năng

### 6. Ước tính ngân sách  
Có thể xem chi phí trên [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=0a74d43130ed1b5856ad5b775a746e30d39583db)  

### **Dịch Vụ AWS (AWS Services)**
- AWS WAF: $11.6/tháng 
- Application Load Balancer (ALB): $18.63/tháng 
- Amazon EC2 Application: $19.27/tháng
- Amazon EC2 Data Tier: $9.64/tháng 
- Amazon EC2 Nat Instances: $19.27/tháng
- Amazon S3: $3.72/tháng 
- AWS CodeDeploy: 0$
- AWS Secrets Manager: $0.4/tháng 
- Amazon Cognito: $14.25/tháng
- Amazon CloudWatch: $4.91/tháng
- AWS CloudTrail: $1.77/tháng
- VPC Endpoints: $16.05/tháng
Total: $119.51/tháng, $1434.12/12 tháng

### 7. Đánh giá rủi ro  
*Ma trận rủi ro*  
⦁	Mất kết nối mạng: Ảnh hưởng trung bình / Xác suất trung bình.
⦁	Hỏng/Treo EC2 / ALB / AZ: Ảnh hưởng cao / Xác suất thấp–trung bình.
⦁	Rò rỉ bí mật: Ảnh hưởng cao / Xác suất thấp–trung bình.
⦁	Vượt chi phí: Ảnh hưởng trung bình / Xác suất trung bình.

*Chiến lược giảm thiểu*  
⦁	Tính khả dụng: Multi-AZ + Auto Scaling + ALB; health checks; caching bằng CloudFront; Route 53 failover.
Sử dụng caching của CloudFront để giảm sự phụ thuộc vào dịch vụ backend.
⦁	Bảo mật: IAM theo nguyên tắc ít quyền nhất (least-privilege); Secrets Manager với cơ chế xoay vòng (rotation) và ghi nhật ký truy cập; bảo vệ bằng WAF/Shield.
⦁	Quản lý chi phí: AWS Budgets & Cost Explorer; điều chỉnh kích thước phù hợp (rightsizing); sử dụng Reserved hoặc Spot instances khi phù hợp.
 

*Kế hoạch dự phòng*  
⦁	Tự động rollback thông qua CodeDeploy.
⦁	Dự phòng thủ công: GitLab runner → prebuilt AMIs hoặc ASGs

### 8. Kết quả kỳ vọng  
⦁	CI/CD hoàn toàn tự động (GitLab → CodePipeline → CodeBuild → CodeDeploy) giảm lỗi thủ công.
⦁	Bảo mật nhiều lớp (IAM, WAF, Secrets Manager) với audit qua CloudTrail.
