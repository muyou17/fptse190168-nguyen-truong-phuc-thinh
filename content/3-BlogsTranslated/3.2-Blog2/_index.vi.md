---
title: "Blog 2"
date: "2025-10-09"
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Giảm chi phí Amazon ElastiCache của bạn lên đến 60% với Valkey và CUDOS
*bởi Chris Gillespie, Brenno Passanha, và Yuriy Prykhodko | vào 04/09/2025 | trong Amazon ElastiCache, Cloud Cost Optimization, Intermediate (200), Technical How-to | [Link cố định](https://docs.google.com/document/d/1suPwHOCkhGvLkEFdOxGAjkcEHFrdCOrxE_tu7Y2OhZA/edit?usp=sharing)*

Trong bài viết này, chúng tôi sẽ hướng dẫn bạn cách tiết kiệm chi phí trên Amazon ElastiCache bằng cách nâng cấp công cụ cụm (cluster engine) của bạn lên ElastiCache cho Valkey. Nếu bạn hiện đang sử dụng ElastiCache cho Redis OSS, bạn có thể đạt được mức tiết kiệm chi phí lên đến 60% bằng cách nâng cấp lên Valkey.

Chúng tôi cũng sẽ giới thiệu cho bạn CUDOS dashboard, một phần của Cloud Intelligence Dashboards (CID) framework mã nguồn mở, để bạn có thể xem mức tiết kiệm chi phí khi chuyển từ ElastiCache Redis sang Valkey trong tổ chức của mình, theo dõi tiến độ đạt được các mục tiêu này và ưu tiên các nỗ lực nâng cấp.

## Amazon ElastiCache cho Valkey

Valkey là một kho dữ liệu key-value mã nguồn mở, hiệu năng cao, được điều hành bởi Linux Foundation và được hỗ trợ bởi hơn 50 công ty bao gồm Ericsson, Google Cloud, Aiven, Oracle, Percona, ByteDance và Amazon Web Services (AWS). Valkey là một bản thay thế tương thích hoàn toàn (drop-in replacement) cho Redis OSS và đã được cộng đồng đón nhận nhanh chóng kể từ khi dự án được khởi xướng. Valkey hiện khả dụng như một dịch vụ được quản lý thông qua ElastiCache.

Với ElastiCache cho Valkey, bạn có thể chọn giữa các cụm tự thiết kế (node-based) hoặc triển khai bộ nhớ đệm serverless. ElastiCache cho Valkey Serverless có giá thấp hơn 33% so với các engine được hỗ trợ khác, và cụm tự thiết kế (node-based) có giá thấp hơn 20%. Bạn có thể chuyển sang các mức giá thấp hơn này thông qua quá trình nâng cấp tại chỗ (in-place), không downtime, từ cụm ElastiCache hiện có sử dụng Redis OSS sang Valkey. Các Reservation ElastiCache for Redis OSS hiện có của bạn vẫn tiếp tục được áp dụng.

## Nâng cấp lên ElastiCache cho Valkey

Khi ElastiCache thực hiện quá trình nâng cấp từ Redis OSS lên Valkey, nó sẽ tuân theo cùng một quy trình như một nâng cấp major version của engine Redis OSS hiện có. Nếu cụm của bạn là cụm tự thiết kế, bạn nên tuân thủ các best practice khi nâng cấp.

Khi bắt đầu nâng cấp, bạn có thể chọn phiên bản chính (major version) của Valkey. Valkey khả dụng từ phiên bản 7.2, bao gồm các thay đổi từ Redis OSS cho đến phiên bản 7.2.4. Nếu cụm của bạn đang chạy Redis OSS ở phiên bản cũ hơn 7.2.4, bạn nên xem xét các thay đổi có thể ảnh hưởng đến workload của bạn.

Nâng cấp lên phiên bản 8.1 sẽ mang lại những lợi ích bổ sung như tăng tốc độ mở rộng, Bloom filters, cập nhật có điều kiện, và nhiều cải tiến khác. Phiên bản 8.0 đã bao gồm cải thiện về sử dụng bộ nhớ so với Valkey 7.2 (và do đó là so với tất cả các phiên bản Redis OSS), với cải thiện xa hơn hơn nữa vào phiên bản 8.1. Biểu đồ dưới đây minh họa sự cải thiện về mức sử dụng bộ nhớ đối với một workload mẫu. Phân tích chi tiết hơn có thể được tìm thấy trong bài viết Năm đầu của Valkey: Những đổi mới mã nguồn mở và ElastiCache phiên bản 8.1.

Cải thiện 40% về hiệu suất sử dụng bộ nhớ có thể cho phép bạn giảm kích thước loại node trong một cụm thiết kế thủ công. Điều này sẽ tiết kiệm chi phí thêm khoảng 50% cho cùng một khối lượng công việc. Tổng mức tiết kiệm chi phí trong trường hợp này sẽ là 60% (`1 - (0.8 * 0.5)`).

Bạn có thể bắt đầu nâng cấp ElastiCache Redis OSS sang Valkey thông qua AWS Management Console, AWS Command Line Interface (AWS CLI), AWS API, hoặc AWS CloudFormation.
* Console – Với các cụm tự thiết kế hoặc serverless, hãy làm theo hướng dẫn tại Nâng cấp từ ElastiCache cho Redis OSS lên ElastiCache cho Valkey trong Bắt đầu với Amazon ElastiCache cho Valkey.
* AWS CLI – Dùng lệnh  `modify-cache-cluster` hoặc `modify-serverless-cache`. Thiết lập engine thành valkey và chỉ định phiên bản. Ví dụ, để nâng cấp một cụm tự thiết kế lên Valkey phiên bản 8.0, dùng lệnh:
```bash
aws elasticache modify-cache-cluster \
  --cache-cluster-id my-cluster \
  --engine valkey \
  --engine-version 8.0
```

Với cụm serverless, dùng:
```bash
aws elasticache modify-serverless-cache-instance \
  --cache-cluster-id my-serverless-cluster \
  --engine valkey \
  --engine-version 8.0
```
* API – Dùng API ModifyCacheCluster hoặc ModifyServerlessCacheInstance. Thiết lập tham số Engine thành valkey và EngineVersion thành phiên bản chính mong muốn.
* CloudFormation – Cập nhật các thuộc tính CacheClusterEngine và CacheClusterEngineVersion trong tài nguyên AWS::ElastiCache::CacheCluster hoặc AWS::ElastiCache::GlobalReplicationGroup.

Trong quá trình nâng cấp, trạng thái cụm sẽ hiển thị là modifying. Sau khi hoàn tất, trạng thái sẽ chuyển về available. Quá trình nâng cấp tự động xử lý việc di chuyển dữ liệu, và cụm của bạn vẫn khả dụng suốt quá trình.

Việc lập kế hoạch và ưu tiên di chuyển, hiểu rõ tiềm năng tiết kiệm trên từng tài nguyên, và theo dõi hiệu suất ở quy mô lớn đòi hỏi khả năng quan sát tổng thể. Để đơn giản hóa những công việc này, AWS cung cấp CUDOS dashboard, giúp bạn có cái nhìn chi tiết về cơ hội tiết kiệm chi phí trên toàn bộ tài nguyên ElastiCache của mình.

## Cloud Intelligent Dashboards framework

CUDOS dashboard là một phần của Cloud Intelligence Dashboards (CID) framework mã nguồn mở. Bạn có thể triển khai khung này trong tài khoản AWS của mình bằng cách sử dụng mẫu cơ sở hạ tầng dưới dạng mã (Infrastructure as Code – IaC) được cung cấp sẵn. Framework này giúp bạn tăng cường trách nhiệm tài chính, tối ưu hoá chi phí và nâng cao hiệu quả vận hành trên toàn bộ tổ chức AWS của bạn. CUDOS dashboard cung cấp các thông tin chi tiết và có thể hành động, cho phép bạn ra quyết định dựa trên dữ liệu nhằm đạt được hiệu quả chi phí cao hơn trong cơ sở hạ tầng AWS của mình.

## Thông tin nâng cấp ElastiCache trong CUDOS

CUDOS phiên bản 5.6 giới thiệu một phần ElastiCache toàn diện, giúp bạn chuyển đổi quy trình di trú Redis sang Valkey thành một chiến lược tối ưu hóa dựa trên dữ liệu. Dashboard cung cấp các tính năng sau:
* Theo dõi quá trình áp dụng bằng hình ảnh – Theo dõi tiến trình áp dụng của bạn với các biểu đồ rõ ràng thể hiện tỷ lệ giữa Redis OSS, Valkey tự thiết kế (self-designed) và Valkey serverless theo thời gian.
* Đo lường mức tiết kiệm – Xem hiệu quả chi phí đã đạt được trong quá khứ từ các cụm đã nâng cấp lên Valkey.
* Khuyến nghị chi tiết – Phân tích từng cụm riêng biệt, hiển thị chi phí hiện tại của Redis OSS, chi phí ước tính khi dùng Valkey, và tiềm năng tiết kiệm chính xác cho từng tài khoản AWS và cụm ElastiCache.

Bảng phân tích chi phí chi tiết trong dashboard hiển thị chính xác cụm nào mang lại tiềm năng tiết kiệm cao nhất, với chi phí hiện tại trên Redis OSS, chi phí ước tính nếu nâng cấp lên Valkey, và khoản tiết kiệm hàng tháng dự kiến. Bạn có thể lọc các cơ hội tiết kiệm này dựa trên hệ thống phân loại nội bộ của doanh nghiệp, chẳng hạn như thẻ phân bổ chi phí, đơn vị tổ chức, danh mục chi phí, hoặc các chiều dữ liệu khác phù hợp với quy trình ra quyết định của tổ chức bạn.

## Bắt đầu với CUDOS

Để bắt đầu với CUDOS, bạn có thể khám phá phần ElastiCache trong interactive demo dashboard. Hãy làm theo hướng dẫn triển khai để thiết lập CUDOS trong tổ chức của bạn.

Nếu bạn đã sử dụng CUDOS, hãy làm theo hướng dẫn cập nhật để nâng cấp lên phiên bản 5.6. Bạn cũng có thể dùng hướng dẫn thêm phân loại tổ chức để thêm khả năng lọc trực quan theo thẻ (tags), danh mục chi phí, và các chiều dữ liệu khác trong hệ thống phân loại của tổ chức.

Bạn có thể truy cập trang Cloud Intelligence Dashboards để tìm hiểu thêm về các dashboard khác giúp bạn phân tích vận hành.

## Tổng kết

ElastiCache for Valkey là một giải pháp thay thế tương thích hoàn toàn (drop-in replacement) cho ElastiCache for Redis OSS. Bằng cách nâng cấp các cụm ElastiCache for Redis OSS hiện có sang ElastiCache for Valkey, bạn sẽ:
* Giảm 33% chi phí đối với cụm serverless, hoặc 20% chi phí đối với cụm tự thiết kế (node-based).
* Tiếp tục sử dụng các gói đặt chỗ (Reservations) hiện tại của ElastiCache for Redis OSS cho các cụm đã nâng cấp.
* Thực hiện quy trình nâng cấp tại chỗ không downtime, đảm bảo gián đoạn ứng dụng tối thiểu.
* Cải thiện hiệu suất sử dụng bộ nhớ ElastiCache, với khả năng giảm kích thước node trong cụm tự thiết kế, mang lại tiết kiệm chi phí bổ sung lên đến 50%.

Để tính toán và theo dõi các khoản tiết kiệm hiệu quả này trên toàn tổ chức, cũng như ưu tiên kế hoạch di trú, bạn có thể sử dụng CUDOS dashboard, nơi cung cấp phần chuyên biệt về di trú ElastiCache Redis sang Valkey.

Tại AWS, chúng tôi tin rằng Valkey đại diện cho tương lai của các hệ thống lưu trữ dữ liệu trong bộ nhớ. Để biết hướng dẫn từng bước về cách bắt đầu với ElastiCache for Valkey, hãy xem Bắt đầu với Amazon ElastiCache for Valkey hoặc truy cập tài liệu Amazon ElastiCache.

## Về các tác giả

### Chris Gillespie

Chris là Senior Solutions Architect tại Vương quốc Anh. Anh dành phần lớn thời gian làm việc với các khách hàng “sinh ra trên đám mây”. Ngoài công việc, Chris dành thời gian cho gia đình và rèn luyện sức khỏe.

### Brenno Passanha

Brenno là Senior Technical Account Manager. Anh là thành viên của Cloud Operations Technical Field Community, chuyên tập trung vào Cloud Financial Management. Ngoài giờ làm việc, Brenno thích dành thời gian chăm sóc con cái, đi du lịch khắp thế giới và tạo nên những kỷ niệm qua những trải nghiệm mới.

### Yuriy Prykhodko

Yuriy là Principal Technical Account Manager làm việc tại Luxembourg. Anh giúp khách hàng xây dựng các hệ thống trên AWS có độ tin cậy cao và chi phí tối ưu. Yuriy cũng là chuyên gia FinOps và là tác giả kiêm trưởng nhóm phát triển Cloud Intelligence Dashboards. Trong thời gian rảnh, anh thích chơi bóng rổ và đi du lịch vòng quanh thế giới cùng gia đình và bạn bè.
