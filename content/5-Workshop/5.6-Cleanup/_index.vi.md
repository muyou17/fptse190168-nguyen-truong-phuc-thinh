---
title : "Dọn dẹp tài nguyên"
date : 2025-12-07


weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---
Chúc mừng bạn đã hoàn thành workshop này!
Trong workshop này, bạn đã tìm hiểu các mô hình kiến trúc để xây dựng ứng dụng Serverless hướng sự kiện (Event-driven) trên AWS.

Bằng cách cấu hình S3 Event Notifications, bạn đã kích hoạt một quy trình tự động hóa nơi tài nguyên tính toán (AWS Lambda) phản ứng tức thì với việc nạp dữ liệu mà không cần can thiệp thủ công hay quản lý máy chủ.

Bằng cách tích hợp Amazon Polly, bạn đã tận dụng thành công các Dịch vụ AI được quản lý (Managed AI Services) để chuyển đổi văn bản thành giọng nói sống động, chứng minh cách thêm các khả năng máy học phức tạp vào ứng dụng của mình với lượng code tối thiểu.

#### Dọn dẹp tài nguyên
1. Xóa S3 buckets
+ Mở giao diện S3 console
+ Mở bucket s3-demo-bucket
+ Chọn 2 thư mục ```input``` và ```output```
+ Nhấn **Delete** và xác nhận
![delete s3](/images/5-Workshop/5.6-Cleanup/delete-folder.jpg)
+ Sau đó chọn bucket mà chúng ta đã tạo cho bài lab, nhấn Empty (làm rỗng) và xác nhận. Nhấn Delete (xóa) và xác nhận xóa.
![delete s3](/images/5-Workshop/5.6-Cleanup/delete-s3.jpg)


2. Xóa Lambda function
+ Mở giao diện Lambda console 
+ Tìm function **workshop1** và nhấn **Actions**
+ Chọn **Delete** và xác nhận 

![delete lambda](/images/5-Workshop/5.6-Cleanup/delete-lambda.jpg)

3. Xóa IAM role
+ Mở giao diện IAM console
+ Chọn **Roles** từ menu bên trái.
+ Tìm role **PollyLambdaRole**.
+ Chọn role đó và nhấn **Delete**.
![delete s3](/images/5-Workshop/5.6-Cleanup/delete-IAM.jpg)