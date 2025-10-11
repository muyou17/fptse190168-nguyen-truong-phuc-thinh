---
title: "Bản đề xuất"
date: "2025-09-09"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---


# Nền tảng Bán Nguyên Liệu Thực Phẩm Cá Nhân Hóa

### 1. Tóm tắt điều hành
Nền tảng Bán Nguyên Liệu Thực Phẩm Cá Nhân Hóa tập trung vào việc giúp khách hàng mua sắm nguyên liệu nhanh chóng và hiệu quả bằng cách cung cấp nguyên liệu tươi kèm công thức nấu ăn được cá nhân hóa. Người dùng đăng ký tài khoản để khám phá kho công thức đa dạng, nhận gợi ý món ăn từ AI dựa trên lịch sử mua hàng và sở thích, đồng thời đặt mua nguyên liệu giao tận nơi. Hệ thống cho phép tùy chỉnh khẩu phần và tính toán calo/macro chính xác để tăng tiện lợi. Nền tảng tận dụng hạ tầng đám mây AWS để đảm bảo khả năng mở rộng linh hoạt, hiệu suất cao và quản lý an toàn.

### 2. Tuyên bố vấn đề
*Vấn đề hiện tại*
Khách hàng gặp khó khăn trong việc tìm mua nguyên liệu nấu ăn một cách hiệu quả, thường mất nhiều thời gian để tìm nguồn cung phù hợp với công thức. Các nền tảng hiện tại như MealPlan VN chỉ cung cấp gợi ý thực đơn và công thức nấu ăn, nhưng thiếu tích hợp bán nguyên liệu trực tiếp, khiến người dùng phải tự tìm nguồn cung cấp, tốn thời gian và dễ dẫn đến sai sót trong khẩu phần.

*Giải pháp*
Nền tảng sử dụng Spring Boot để xây dựng backend REST API xử lý đăng ký người dùng, quản lý công thức, giỏ hàng và đơn hàng; React cho frontend giao diện thân thiện với gợi ý AI dựa trên lịch sử mua hàng. Dữ liệu được lưu trữ trên AWS RDS (PostgreSQL), hình ảnh và tài nguyên tĩnh trên Amazon S3, backend triển khai trên Amazon EC2 trong VPC an toàn, và Route 53 quản lý domain. Người dùng có thể tùy chỉnh khẩu phần, nhận gợi ý món ăn cá nhân hóa, và đặt mua nguyên liệu giao tận nơi. Các tính năng chính bao gồm kho công thức đa dạng, tính toán calo chính xác, tích hợp CI/CD qua GitHub Actions hoặc AWS CodePipeline, và chi phí vận hành thấp.

*Lợi ích và hoàn vốn đầu tư (ROI)*
Giải pháp tạo nền tảng toàn diện để startup dinh dưỡng đơn giản hóa mua sắm nguyên liệu, đồng thời thu thập dữ liệu người dùng cho hệ thống khuyến nghị nâng cao. Nền tảng giảm bớt việc mua sắm rời rạc thông qua hệ thống tích hợp, tiết kiệm thời gian cho người dùng và tăng doanh thu từ bán nguyên liệu. Chi phí hàng tháng ước tính 21.13 USD (theo AWS Pricing Calculator), tổng cộng khoảng 253.56 USD cho 12 tháng. Tất cả công nghệ phát triển dựa trên framework mã nguồn mở, không phát sinh chi phí phần cứng thêm. Thời gian hoàn vốn 6–12 tháng nhờ tiết kiệm thời gian cho người dùng và doanh thu ổn định từ đơn hàng.

### 3. Kiến trúc giải pháp
Nền tảng áp dụng kiến trúc AWS Serverless để quản lý dữ liệu từ 5 trạm dựa trên Raspberry Pi, có thể mở rộng lên 15 trạm. Dữ liệu được tiếp nhận qua AWS IoT Core, lưu trữ trong S3 data lake và xử lý bởi AWS Glue Crawlers và ETL jobs để chuyển đổi và tải vào một S3 bucket khác cho mục đích phân tích. Lambda và API Gateway xử lý bổ sung, trong khi Amplify với Next.js cung cấp bảng điều khiển được bảo mật bởi Cognito.
Nền tảng tận dụng kiến trúc AWS dựa trên VPC để lưu trữ một ứng dụng web có khả năng mở rộng. Các thành phần front-end chạy trong subnet công khai và kết nối một cách an toàn với các dịch vụ back-end trong private subnet. Lưu lượng từ internet chảy qua Internet Gateway (IGW) đến các instance front-end, nơi tương tác với các instance back-end để xử lý business logic và dữ liệu. Cơ sở dữ liệu nằm trong private subnet để tăng cường bảo mật và cách ly. Route 53 quản lý định tuyến DNS, trong khi bucket S3 cung cấp lưu trữ ứng dụng có thể truy cập qua các VPC endpoint. IAM kiểm soát quyền truy cập, và các pipeline CI/CD được điều phối bằng AWS CodeBuild và CodePipeline để tự động hóa triển khai.

![Architecture](/images/2-Proposal/architecture.png)

*Dịch vụ AWS sử dụng*

- **Amazon VPC**: Cung cấp mạng riêng biệt với subnet công khai và riêng tư trong một Availability Zone.
- **Amazon EC2**: Chạy máy chủ ứng dụng cho cả front-end và back-end.
- **Amazon RDS**: Dịch vụ cơ sở dữ liệu quan hệ được quản lý trong subnet riêng tư.
- **Amazon S3**: Lưu trữ dữ liệu ứng dụng và sao lưu.
- **VPC Endpoint**: Cho phép EC2 truy cập S3 nội bộ mà không cần đi qua Internet.
- **Amazon Route 53**: Quản lý tên miền và định tuyến truy cập từ bên ngoài.
- **AWS CodeBuild & CodePipeline**: Tự động hóa quy trình build và triển khai ứng dụng từ GitHub lên EC2.
- **AWS IAM**: Quản lý quyền truy cập và phân quyền bảo mật cho người dùng và dịch vụ.

*Thiết kế thành phần*

- **Mạng (Networking)**:
  Kiến trúc hoạt động trong một VPC duy nhất, bao gồm một public subnet và hai private subnet.

- **Lớp giao diện (Front-End Layer)**:
  EC2 trong private subnet xử lý lưu lượng truy cập từ internet thông qua IGW và được quản lý bằng Route 53.

- **Lớp ứng dụng (Back-End Layer)**:
  EC2 trong private subnet xử lý các yêu cầu từ front-end và giao tiếp với cơ sở dữ liệu.

- **Lớp dữ liệu (Data Layer)**:
  RDS nằm trong private subnet để lưu trữ dữ liệu lâu dài và bảo mật.

- **Lớp lưu trữ (Storage Layer)**:
  Dữ liệu và tệp ứng dụng được lưu trong Amazon S3, truy cập qua VPC Endpoint.

- **Quy trình CI/CD**:
  CodePipeline và CodeBuild tự động lấy mã nguồn từ GitHub, build và triển khai bản cập nhật lên các EC2 tương ứng.

- **Bảo mật & Quyền truy cập**:
  IAM quản lý quyền và vai trò, đảm bảo chỉ các dịch vụ và người dùng được phép mới có thể truy cập tài nguyên.

### 4. Triển khai kỹ thuật
*Các giai đoạn triển khai*
Dự án có hai phần: phát triển back-end Spring Boot và front-end React, và triển khai website lên AWS sử dụng các dịch vụ AWS, mỗi phần theo 4 giai đoạn.
- **Xây dựng giả thuyết và vẽ kiến trúc**: Thu thập yêu cầu cho ứng dụng web, thiết kế kiến trúc hệ thống (REST API Spring Boot + front-end React), và định nghĩa schema cơ sở dữ liệu (Tháng 1).
- **Phát triển, kiểm tra**: Triển khai back-end Spring Boot với REST API (xác thực, quản lý người dùng, CRUD meal/recipe, giỏ hàng, v.v.), và xây dựng front-end React (UI/UX, routing, forms, quản lý trạng thái). Thực hiện unit test cho dịch vụ back-end, integration test cho API endpoint, và front-end test (Jest/React Testing Library). (Tháng 1–2)
- **Tính toán chi phí và kiểm tra tính khả thi**: Sử dụng AWS Pricing Calculator để ước tính chi phí cho EC2 (lưu trữ back-end), RDS (cơ sở dữ liệu), S3 (tệp tĩnh và hình ảnh), VPC (mạng), Route53 (domain), sau đó điều chỉnh nếu cần (Tháng 2).
- **Tích hợp AWS**: Tích hợp các dịch vụ AWS vào ứng dụng. Triển khai website trên EC2, lưu trữ hình ảnh trên S3, cấu hình RDS cho cơ sở dữ liệu, sử dụng VPC để quản lý mạng, Route53 cho domain, và thiết lập pipeline CI/CD (GitHub Actions hoặc AWS CodePipeline). Thực hiện staging test trước khi phát hành sản xuất cuối cùng. (Tháng 3)

*Yêu cầu kỹ thuật*
- **Back-end (Spring Boot):** REST API cho xác thực, quản lý người dùng, CRUD meal/recipe, giỏ hàng, và xử lý đơn hàng. Bao gồm bảo mật (JWT, Spring Security), xác thực, và tích hợp với AWS RDS.
- **Front-end (React):** Ứng dụng web responsive với UI/UX thân thiện, routing (React Router), quản lý trạng thái (Redux hoặc Context API), và tích hợp API với back-end Spring Boot.
- **Cơ sở dữ liệu (AWS RDS):** Cơ sở dữ liệu quan hệ (MySQL/PostgreSQL) để lưu trữ người dùng, công thức, giỏ hàng, và dữ liệu đơn hàng.
- **Lưu trữ (AWS S3):** Sử dụng để lưu trữ tệp build React tĩnh và hình ảnh do người dùng tải lên (ví dụ: hình ảnh công thức, ảnh hồ sơ).
- **Host & Mạng (AWS EC2 & AWS VPC):** Back-end Spring Boot được triển khai trên các instance EC2, được bảo mật và cách ly trong VPC để kiểm soát mạng và truy cập.
- **Quản lý Domain (AWS Route 53):** Cấu hình domain tùy chỉnh và quản lý DNS cho website.
- **CI/CD (GitHub Actions or AWS CodePipeline):** Pipeline tự động xây dựng, kiểm thử và triển khai cho cả back-end và front-end.
- **Công cụ kiểm tra:** JUnit/Mockito cho unit test và back-end integration, Jest/React Testing Library cho front-end test, Postman cho API test.
- **Xác thực & Bảo mật:** Xác thực JWT và cấu hình HTTPS; tùy chọn AWS Cognito để quản lý truy cập người dùng.

## 5. Lộ trình & Mốc triển khai

### Project Timeline

- **Trước dự án (Tháng 1):** Thành lập nhóm, phân công vai trò (back-end, front-end, triển khai AWS), thu thập yêu cầu, và soạn thảo kiến trúc hệ thống ban đầu.
- **Thực hiện dự án (Tháng 1–3):**
  - **Tháng 1:** Xây dựng giả thuyết và vẽ kiến trúc (back-end Spring Boot + thiết kế front-end React, schema cơ sở dữ liệu). Bắt đầu phát triển ban đầu cho back-end và front-end.
  - **Tháng 2:** Tiếp tục phát triển back-end và front-end, thực hiện unit và integration test. Sử dụng AWS Pricing Calculator để đánh giá chi phí lưu trữ và tinh chỉnh kiến trúc để tối ưu hóa chi phí.
  - **Tháng 3:** Tích hợp các dịch vụ AWS (EC2, RDS, S3, VPC, Route 53), cấu hình pipeline CI/CD, thực hiện kiểm thử staging, và triển khai website lên sản xuất.
- **Sau phát hành:** Lên đến 3 tháng cho bảo trì, tối ưu hóa, và nâng cấp tính năng (ví dụ: mở rộng back-end, cải thiện UI/UX, thêm tính năng mới).

## 6. Ước tính Ngân sách

- Dịch vụ AWS:
  - VPC: $0.00/tháng (không tốn phí cho subnet, route table, security group)
  - EC2: ≈$7.60/tháng (shared instance, Linux, sử dụng liên tục, t4g.micro, 360 giờ, 20 GB gp3)
  - Route 53: $1.10/tháng (1 hosted zone, dưới 1 triệu truy vấn DNS, 1 DNS Firewall)
  - S3 Standard: ≈$0.33/tháng (10 GB, ~5000 requests, outbound data transfer)
  - RDS: ≈$12.00/tháng (db.t4.micro, dung lượng 20 GB, PostgreSQL, 360 giờ, không có chi tiết hiệu năng)
  - CodeBuild: ≈$0.10/tháng (3 lần build trong tháng, thời lượng 10 phút, arm1.small, Linux)
  - CodePipeline: $0.00/tháng (40 free action, free tier)

Total: ≈$21.13/tháng (không bao gồm truyền dữ liệu ra ngoài), ≈253.56/12 tháng

## 7. Đánh giá rủi ro

*Ma trận rủi ro*
- EC2 và RDS downtime: Tác động trung bình, xác xuất trung bình
- Nghẽn cổ chai cơ sở dữ liệu: Tác động trung bình, trong giờ cao điểm
- Vượt ngân sách: Tác động cao, xác suất rất thấp
- Mối đe dọa bảo mật: Tác động cao, xác suất thấp

*Chiến lược giảm thiểu*
- Kích hoạt Auto Scaling cho các instance EC2.
- Thiết lập AWS Budget Alerts để giám sát và kiểm soát chi tiêu.
- Triển khai vai trò IAM, Security Group, và AWS WAF để tăng cường bảo mật.

### Kế hoạch dự phòng
- Phục hồi & Chuyển tiếp: Thăng cấp read replica RDS hoặc khôi phục snapshot; triển khai lại EC2 từ AMI.

## 8. Kết quả kỳ vọng

*Cải tiến kỹ thuật*
- Pipeline CI/CD + IaC để triển khai và rollback đáng tin cậy.
- Giám sát và cảnh báo với CloudWatch để theo dõi sức khỏe hệ thống.

*Giá trị dài hạn*
- Thu thập dữ liệu sở thích người dùng để xây dựng hệ thống khuyến nghị cho gợi ý món ăn cá nhân hóa.
- Mở rộng cơ sở hạ tầng để hỗ trợ hàng nghìn người dùng.
- Tiềm năng tích hợp với các dịch vụ giao hàng.
