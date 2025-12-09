---
title: "Tổng quan Workshop"
date: 2025-12-07
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# GIỚI THIỆU

## Giới thiệu về S3 Event Notifications

- **S3 Event Notifications** là tính năng cho phép Amazon S3 tự động gửi thông báo khi có các sự kiện cụ thể xảy ra trong Bucket (ví dụ: khi một file mới được upload, xóa hoặc sao chép).
- S3 có thể gửi thông báo đến nhiều đích đến khác nhau như AWS Lambda, Amazon SNS, hoặc Amazon SQS.
- Trong workshop này, chúng ta sử dụng sự kiện `PutObject` để tự động kích hoạt Lambda function ngay khi file văn bản được upload lên S3.

## Giới thiệu về Amazon Polly

- **Amazon Polly** là dịch vụ chuyển văn bản thành giọng nói (Text-to-Speech) sử dụng công nghệ Deep Learning tiên tiến của AWS.
- Polly hỗ trợ hơn 60 giọng nói trong hơn 20 ngôn ngữ, bao gồm cả tiếng Việt, với chất lượng âm thanh tự nhiên giống con người.
- Dịch vụ này hoàn toàn được quản lý (fully managed), bạn không cần lo lắng về việc quản lý hạ tầng hay khả năng mở rộng.

## Tổng quan về workshop

Trong workshop này, bạn sẽ xây dựng một ứng dụng **Serverless Text-to-Speech Converter** (Chuyển đổi văn bản thành giọng nói). Hệ thống hoạt động hoàn toàn tự động dựa trên mô hình Event-driven Architecture:

- **Amazon S3 (Input Bucket)**: Lưu trữ file văn bản đầu vào (`.txt`) do người dùng upload.
- **S3 Event Notifications**: Phát hiện sự kiện upload file mới và kích hoạt AWS Lambda.
- **AWS Lambda**: Bộ xử lý trung tâm, đóng vai trò điều phối luồng dữ liệu giữa S3 và Polly. Lambda đọc file văn bản, gọi Polly API để chuyển đổi, và lưu kết quả.
- **Amazon Polly**: Dịch vụ AI thực hiện chuyển đổi văn bản thành âm thanh với giọng nói tự nhiên.
- **Amazon S3 (Output Bucket)**: Lưu trữ file âm thanh đầu ra (`.mp3`) sau khi xử lý hoàn tất.

Toàn bộ quy trình diễn ra tự động mà không cần can thiệp thủ công, giúp tiết kiệm thời gian và chi phí vận hành.

## Kiến trúc hệ thống

Mô hình dưới đây mô tả chi tiết kiến trúc và luồng dữ liệu (Data Flow) của hệ thống Text-to-Speech Converter:

![Architecture Diagram](/images/5-Workshop/5.1-Workshop-overview/diagram1.jpg)


**Luồng hoạt động (Workflow):**

1. **Upload file**: Người dùng tải file văn bản (`.txt`) lên **Amazon S3 Input Bucket**.

2. **Event trigger**: S3 phát hiện sự kiện `PutObject` (có file mới), tự động gửi event notification kích hoạt **AWS Lambda Function**.

3. **Lambda xử lý**: Lambda function được trigger và thực hiện:
   - Đọc nội dung file văn bản từ S3 Input Bucket
   - Gửi nội dung văn bản đến **Amazon Polly** API cùng với các tham số (voice ID, output format)

4. **Polly chuyển đổi**: Amazon Polly nhận request, xử lý văn bản và trả về audio stream (dữ liệu âm thanh) cho Lambda.

5. **Lưu kết quả**: Lambda nhận audio stream từ Polly và lưu dưới dạng file `.mp3` vào **Amazon S3 Output Bucket**.

6. **Hoàn tất**: Người dùng có thể tải xuống file MP3 từ S3 Output Bucket để sử dụng.

## Lợi ích của kiến trúc Serverless

- **Không cần quản lý server**: AWS tự động xử lý scaling, patching và high availability.
- **Chi phí tối ưu**: Chỉ trả tiền khi có request xử lý (pay-per-use model).
- **Tự động mở rộng**: Hệ thống có thể xử lý từ vài requests đến hàng triệu requests mà không cần cấu hình thêm.
- **Triển khai nhanh**: Tập trung vào logic ứng dụng thay vì quản lý infrastructure.