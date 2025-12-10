---
title: "Blog 3"
date: "2025-10-10"
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---


# Bắt đầu với các instance Amazon EC2 bare metal dành cho Amazon RDS for Oracle và Amazon RDS Custom for Oracle
*bởi Sameer Malik và Nitin Saxena | vào 04/09/2025 | trong Amazon EC2 Bare Metal, Amazon RDS, Amazon RDS Custom, Intermediate (200), RDS for Oracle, Technical How-to | [Link cố định](https://docs.google.com/document/d/1uAenejCFaIhvpFgfbhwSIifjAIbHR-FZBaYxgCItNXU/edit?usp=sharing)*

Amazon Relational Database Service (Amazon RDS) for Oracle là một cơ sở dữ liệu thương mại được quản lý hoàn chỉnh, giúp bạn dễ dàng thiết lập, vận hành và mở rộng các triển khai Oracle trên đám mây. Amazon RDS Custom for Oracle cho phép bạn – với vai trò quản trị viên cơ sở dữ liệu – truy cập và tùy chỉnh môi trường Oracle của bạn cũng như hệ điều hành.

Trong bài viết này, chúng tôi khám phá hỗ trợ các instance bare metal trên Amazon EC2 bare metal Instances cho Amazon RDS for Oracle và RDS Custom for Oracle.

Các instance bare metal EC2 được thiết kế để cung cấp cho ứng dụng của bạn quyền truy cập trực tiếp vào bộ xử lý và bộ nhớ của máy chủ nền tảng. Các instance EC2 bare metal cung cấp dung lượng dành riêng cho sử dụng dưới mô hình thuê không chia sẻ (non-shared tenancy) và mang lại mức độ cô lập (isolation) cao hơn so với các instance ảo hóa (virtualized instance) chia sẻ.
Những instance bare metal này cũng có thể mang lại lợi ích về bản quyền, cho phép bạn sử dụng các giấy phép phần mềm đủ điều kiện theo mô hình Bring Your Own License (BYOL) từ các nhà cung cấp như Microsoft và Oracle. Để biết thêm chi tiết về lợi ích bản quyền khi sử dụng instance bare metal EC2, bạn có thể xem bài viết Hiệu quả cấp phép Oracle với Dedicated Host từ đối tác cấp giấy phép AWS House of Brick.

Thêm nữa, các instance EC2 bare metal trên Amazon RDS for Oracle và RDS Custom for Oracle được cung cấp với giá tính toán thấp hơn 25% so với các instance ảo hóa có kích thước tương đương, giúp cho khối lượng công việc Oracle trên RDS bare metal trở nên kinh tế hơn.

## Những trường hợp sử dụng phổ biến cho EC2 bare metal

Dưới đây là các trường hợp sử dụng thường thấy cho các instance EC2 bare metal:
* Hỗ trợ các workload có yêu cầu bản quyền hạn chế – Điều này thường là động lực chính, đặc biệt đối với các phần mềm doanh nghiệp truyền thống như Oracle Database, SQL Server, SAP, hoặc Windows Server, trong đó bản quyền có thể thuận lợi hơn khi dùng lõi vật lý hoặc phần cứng chuyên dụng.
* Tính toán hiệu năng cao (High-Performance Computing – HPC) và các mô phỏng khoa học – Các workload này cần hiệu năng cao nhất, truy cập trực tiếp các tính năng phần cứng chuyên biệt hoặc giao tiếp thấp độ trễ giữa các node.
* Ứng dụng legacy – Những ứng dụng này không được hỗ trợ trong môi trường ảo hóa hoặc yêu cầu truy cập phần cứng cụ thể.
* Cô lập và bảo mật – Instance bare metal cung cấp mức độ cô lập cao hơn so với các instance ảo hóa được chia sẻ, điều này có thể hữu ích cho các workload nhạy cảm hoặc khi cần tuân thủ quy định bảo mật và tuân thủ. Nó cũng là lựa chọn khi tổ chức muốn chạy các ứng dụng trên cơ sở hạ tầng single-tenant trong môi trường không ảo hóa để đáp ứng các yêu cầu tuân thủ cụ thể.

## Lợi ích khi sử dụng instance RDS bare metal cho Amazon RDS for Oracle và RDS Custom for Oracle

Các instance RDS bare metal mang lại các lợi ích sau:
* Lợi ích bản quyền – Vì instance RDS bare metal cung cấp dung lượng hoàn toàn dành riêng cho sử dụng dưới mô hình thuê không chia sẻ, việc cấp phép có thể trở nên gần giống triển khai on-premise, nơi khách hàng (tùy theo hợp đồng với Oracle) có thể áp dụng bản quyền core-based truyền thống và yếu tố lõi Oracle (Oracle processor core factor).
* Chi phí tính toán thấp hơn – Các instance RDS bare metal cho Amazon RDS for Oracle và RDS Custom for Oracle có chi phí tính toán thấp hơn 25% so với các instance ảo hóa tương đương. Ví dụ, một instance bare metal m6i.metal có chi phí thấp hơn 25% so với một instance ảo hóa m6i.32xlarge.
* Hợp nhất cơ sở dữ liệu hiệu quả – Với lợi ích bản quyền và chi phí tính toán thấp hơn từ instance bare metal trên Amazon RDS for Oracle và RDS Custom for Oracle, bạn có thể hợp nhất workload cơ sở dữ liệu hiệu quả hơn bằng các phương pháp hợp nhất cơ sở dữ liệu hiện được hỗ trợ như hợp nhất theo sơ đồ và các tùy chọn Oracle multi-tenant.
* Hiệu năng nâng cao cho workload cụ thể – Khách hàng chạy workload Oracle Database trong môi trường ảo hóa và sử dụng hơn một nửa số lõi của máy chủ vật lý có thể tận hưởng hiệu suất máy chủ vật lý đầy đủ (bao gồm CPU, bộ nhớ, IOPS, throughput) mà không phải trả thêm chi phí bản quyền.

## Tạo một instance RDS bare metal cho Oracle bằng AWS Management Console

Trong phần này, chúng tôi hướng dẫn cách tạo một instance cơ sở dữ liệu RDS cho Oracle với loại instance bare metal qua AWS Management Console. Để biết chi tiết và điều kiện tiên quyết, xem Tạo một instance Oracle DB.
1. Đăng nhập vào console Amazon RDS.
2. Ở góc trên bên phải của console Amazon RDS, chọn AWS Region mà bạn muốn tạo instance DB.
3. Trong bảng điều hướng, chọn Databases.
4. Chọn Create database.
5. Chọn Standard create.
6. Với Engine type, chọn Oracle.
7. Với Database management type, chọn Amazon RDS.
8. Với Edition, chọn Oracle Enterprise Edition.
9. Với License model, giữ mặc định là Bring Your Own License (BYOL).
10. Với Engine version, chọn phiên bản mà bạn muốn.
11. Trong phần Templates, chọn Production.
12. Với DB instance identifier, nhập tên cho instance DB của bạn.
13. Trong phần Credentials Settings, cung cấp tên người dùng quản trị, chọn Self-managed cho Credentials management, và nhập mật khẩu.
14. Với Encryption key, chọn khóa mã hóa của bạn.
15. Trong phần Instance configuration, với DB instance class, chọn instance bare metal (ví dụ: db.m6i.metal).
16. Hoàn thành các bước còn lại của việc tạo instance và nhấn Create database.

Sau khi bạn tạo instance DB, bạn có thể xác nhận trạng thái của nó trong console Amazon RDS.

## Tạo instance RDS for Oracle bare metal qua AWS CLI

Bạn cũng có thể tạo một instance bare metal sử dụng AWS Command Line Interface (AWS CLI), như ví dụ dưới đây. Hãy đảm bảo AWS CLI đã được cấu hình với thông tin xác thực và Region mặc định cần thiết:

```bash
aws rds create-db-instance \
        --db-instance-identifier metaldb \
        --db-instance-class db.m6i.metal \
        --engine oracle-ee \
        --allocated-storage 100 \
        --master-username <username> \
        --master-user-password <YourStrongPassword> \
        --engine-version 19.0.0.0.ru-2025-04.spb-1.r1 \
        --license-model license-included \
        --publicly-accessible false \
        --storage-encrypted \
        --vpc-security-group-ids sg-xxxxxxxxxxxxxxxxx \
        --db-subnet-group-name my-db-subnet-group
```

## Dọn dẹp

Để tránh bị tính phí do instance RDS đang chạy, bạn nên xóa instance RDS đã provision ở ví dụ trên. Bạn có thể xóa instance DB bằng AWS Management Console, AWS CLI, hoặc API RDS. Để biết chi tiết cách xóa, hãy xem Xoá một instance DB .

## Kết luận

Với các instance Amazon EC2 bare metal dành cho Amazon RDS for Oracle và RDS Custom for Oracle, bạn có thể tận dụng linh hoạt bản quyền bổ sung và tùy chọn chi phí thấp hơn để vận hành khối lượng công việc Oracle trên RDS một cách hiệu quả. Để biết thêm chi tiết về lần ra mắt này, xem Có gì mới.

## Về các tác giả

### Sameer Malik

Sameer đã làm việc với các cơ sở dữ liệu quan hệ (relational database) hơn 23 năm và hiện đang giữ vị trí Principal Database Solution Architect tại AWS, tập trung vào RDS và Aurora. Ông đã hỗ trợ nhiều khách hàng trong việc di cư và hiện đại hóa workload cơ sở dữ liệu của họ lên AWS.

### Nitin Saxena

Nitin là Software Development Manager trong nhóm RDS DBS Managed Commercial Engines tại Amazon Web Services. Ông tập trung vào các dịch vụ như RDS Oracle và RDS Custom for Oracle. Ông thích thiết kế và phát triển các tính năng mới trên RDS Oracle và RDS Custom để giải quyết các vấn đề của khách hàng.
