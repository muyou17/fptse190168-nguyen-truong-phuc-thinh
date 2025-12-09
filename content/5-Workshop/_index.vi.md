---
title: "Workshop"
date: 2025-12-07


weight: 5
chapter: false
pre: " <b> 5. </b> "
---
{{% notice warning %}}
⚠️ **Lưu ý:** Các thông tin dưới đây chỉ nhằm mục đích tham khảo, vui lòng **không sao chép nguyên văn** cho bài báo cáo của bạn kể cả warning này.
{{% /notice %}}

# Hệ thống chuyển đổi Văn bản sang Giọng nói tự động sử dụng Serverless

#### Tổng quan

**Kiến trúc hướng sự kiện (Event-Driven Architecture)** cho phép bạn xây dựng các ứng dụng tự động phản hồi lại các thay đổi trạng thái, chẳng hạn như khi tải lên một tập tin, mà không cần cung cấp hay quản lý máy chủ.

Trong bài thực hành này, bạn sẽ học cách tạo, cấu hình và kiểm thử một quy trình Serverless có khả năng tự động chuyển đổi các file văn bản thành âm thanh giọng nói sống động bằng cách sử dụng các Dịch vụ được quản lý của AWS (AWS Managed Services).

Bạn sẽ tận dụng hai cơ chế chính để xử lý dữ liệu bất đồng bộ:
+ **S3 Event Notifications** - Cấu hình Amazon S3 đóng vai trò là nguồn sự kiện. Nó sẽ tự động kích hoạt một hàm tính toán bất cứ khi nào có một đối tượng mới được tải lên một thư mục đầu vào cụ thể.
+ **Managed AI Services** - Tận dụng Amazon Polly để tổng hợp giọng nói từ văn bản. Điều này cho phép bạn thêm các khả năng học sâu (deep learning) vào ứng dụng của mình thông qua các lệnh gọi API đơn giản mà không cần chuyên môn về khoa học dữ liệu.

#### Nội dung

1. [Tổng quan Workshop](5.1-Workshop-overview)
2. [Các bước chuẩn bị](5.2-Prerequiste/)
3. [Xây dựng Logic xử lý (Lambda)](5.3-S3/)
4. [Cấu hình Lưu trữ (S3)](5.4-lambda/)
5. [Kiểm tra kết quả](5.5-testing/)
6. [Dọn dẹp tài nguyên](5.6-Cleanup/)