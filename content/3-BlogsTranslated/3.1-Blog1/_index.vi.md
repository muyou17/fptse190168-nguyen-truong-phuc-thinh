---
title: "Blog 1"
date: "2025-09-08"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---


# Thí nghiệm ML nhanh chóng cho doanh nghiệp với Amazon SageMaker AI và Comet

*Bài đăng này được viết cùng với Sarah Ostermeier từ Comet.*

Khi các tổ chức doanh nghiệp mở rộng các sáng kiến học máy (ML) từ giai đoạn chứng minh ý tưởng sang sản xuất, độ phức tạp trong việc quản lý các thí nghiệm, theo dõi dòng dõi mô hình, và quản lý khả năng tái tạo tăng lên theo cấp số nhân. Điều này chủ yếu là do các nhà khoa học dữ liệu và kỹ sư ML liên tục khám phá các kết hợp khác nhau của siêu tham số, kiến trúc mô hình, và phiên bản tập dữ liệu, tạo ra lượng lớn siêu dữ liệu cần được theo dõi để đảm bảo khả năng tái tạo và tuân thủ quy định. Khi việc phát triển mô hình ML mở rộng qua nhiều đội nhóm và yêu cầu quy định ngày càng nghiêm ngặt, việc theo dõi thí nghiệm trở nên phức tạp hơn bao giờ hết. Với các quy định AI ngày càng tăng, [đặc biệt là ở EU](https://artificialintelligenceact.eu/), các tổ chức hiện yêu cầu các bản ghi kiểm toán chi tiết về dữ liệu huấn luyện mô hình, kỳ vọng hiệu suất, và quy trình phát triển, khiến việc theo dõi thí nghiệm trở thành một nhu cầu kinh doanh thiết yếu, không chỉ là một thực hành tốt nhất.

[Amazon SageMaker AI](https://aws.amazon.com/sagemaker-ai/) cung cấp cơ sở hạ tầng được quản lý mà các doanh nghiệp cần để mở rộng khối lượng công việc ML, xử lý việc cung cấp tính toán, huấn luyện phân tán, và triển khai mà không cần gánh nặng cơ sở hạ tầng. Tuy nhiên, các đội nhóm vẫn cần khả năng theo dõi thí nghiệm mạnh mẽ, so sánh mô hình, và khả năng cộng tác vượt xa việc ghi log cơ bản.

[Comet](https://www.comet.com/site/) là một nền tảng quản lý thí nghiệm ML toàn diện, tự động theo dõi, so sánh, và tối ưu hóa các thí nghiệm ML trong suốt vòng đời mô hình. Nó cung cấp cho các nhà khoa học dữ liệu và kỹ sư ML các công cụ mạnh mẽ để theo dõi thí nghiệm, giám sát mô hình, tối ưu hóa siêu tham số, và phát triển mô hình cộng tác. Nó cũng cung cấp [Opik](https://www.comet.com/site/products/opik/), nền tảng mã nguồn mở của Comet để quan sát và phát triển LLM.

Comet có sẵn trong SageMaker AI dưới dạng một [Ứng dụng AI Đối tác](https://docs.aws.amazon.com/sagemaker/latest/dg/partner-apps.html), như một khả năng quản lý thí nghiệm được quản lý hoàn toàn, với bảo mật cấp doanh nghiệp, tích hợp quy trình làm việc liền mạch, và quy trình mua sắm đơn giản thông qua [AWS Marketplace](https://aws.amazon.com/marketplace/pp/prodview-etdyy6ne2onzg).

Sự kết hợp này đáp ứng nhu cầu của một quy trình làm việc ML doanh nghiệp từ đầu đến cuối, trong đó SageMaker AI xử lý cơ sở hạ tầng và tính toán, còn Comet cung cấp khả năng quản lý thí nghiệm, đăng ký mô hình, và giám sát sản xuất mà các đội nhóm yêu cầu để tuân thủ quy định và hiệu quả vận hành. Trong bài đăng này, chúng tôi thể hiện một quy trình phát hiện gian lận hoàn chỉnh bằng cách sử dụng SageMaker AI với Comet, thể hiện khả năng tái tạo và ghi log sẵn sàng kiểm toán mà các doanh nghiệp cần hiện nay.

## Comet sẵn sàng cho doanh nghiệp trên SageMaker AI

Trước khi tiến hành các hướng dẫn cài đặt, các tổ chức phải xác định [mô hình vận hành](https://docs.aws.amazon.com/whitepapers/latest/sagemaker-studio-admin-best-practices/operating-model.html) của mình và dựa trên đó, quyết định cách cài đặt Comet. Chúng tôi khuyên bạn nên triển khai Comet bằng cách sử dụng mô hình vận hành liên kết. Trong kiến trúc này, Comet được quản lý tập trung và lưu trữ trong một tài khoản dịch vụ chia sẻ, và mỗi đội ngũ khoa học dữ liệu duy trì các môi trường hoàn toàn tự trị. Mỗi mô hình vận hành đi kèm với những lợi ích và hạn chế riêng. Để biết thêm thông tin, hãy tham khảo Thực hành tốt nhất cho [Quản trị Amazon SageMaker Studio](https://docs.aws.amazon.com/whitepapers/latest/sagemaker-studio-admin-best-practices/sagemaker-studio-admin-best-practices.html).

Hãy cùng đi sâu vào việc cài đặt Comet trong SageMaker AI. Các doanh nghiệp lớn thường có các vai trò sau:
- **Quản trị viên** – Chịu trách nhiệm thiết lập các dịch vụ cơ sở hạ tầng chung và môi trường cho các đội nhóm sử dụng trường hợp.
- **Người dùng** – Các nhà thực hành ML từ các đội nhóm sử dụng trường hợp, sử dụng các môi trường do đội nền tảng thiết lập để giải quyết các vấn đề kinh doanh của họ.

Trong các phần sau, chúng ta sẽ đi qua hành trình của từng vai trò.

Comet hoạt động tốt với cả SageMaker AI và Amazon SageMaker. SageMaker AI cung cấp môi trường phát triển tích hợp (IDE) Amazon SageMaker Studio, và SageMaker cung cấp IDE Amazon SageMaker Unified Studio. Trong bài đăng này, chúng tôi sử dụng SageMaker Studio.

### Hành trình của quản trị viên

Trong kịch bản này, quản trị viên nhận được yêu cầu từ một đội nhóm làm việc trên trường hợp sử dụng phát hiện gian lận để cung cấp một môi trường ML với thiết lập huấn luyện và thí nghiệm được quản lý hoàn toàn. Hành trình của quản trị viên bao gồm các bước sau:

1. Thực hiện các điều kiện tiên quyết để [thiết lập Ứng dụng AI Đối tác](https://docs.aws.amazon.com/sagemaker/latest/dg/partner-app-onboard.html). Điều này thiết lập quyền cho quản trị viên, cho phép Comet đảm nhận [vai trò thực thi SageMaker AI](https://docs.aws.amazon.com/sagemaker/latest/dg/sagemaker-roles.html) thay mặt cho người dùng và các đặc quyền bổ sung để quản lý đăng ký Comet thông qua AWS Marketplace.
2. Trên bảng điều khiển SageMaker AI, trong mục **Ứng dụng và IDE** trong bảng điều hướng, chọn **Ứng dụng AI Đối tác**, sau đó chọn **Xem chi tiết** cho Comet.
![](/images/3-BlogsTranslated/blog-1-1.png)
Các chi tiết được hiển thị, bao gồm mô hình định giá hợp đồng cho Comet và chi phí ước tính của cấp độ cơ sở hạ tầng.
![](/images/3-BlogsTranslated/blog-1-2.png)
Comet cung cấp các tùy chọn đăng ký khác nhau từ hợp đồng 1 tháng đến 36 tháng. Với hợp đồng này, người dùng có thể truy cập Comet trong SageMaker. Dựa trên số lượng người dùng, quản trị viên có thể xem xét và phân tích kích thước phiên bản phù hợp cho máy chủ bảng điều khiển Comet. Comet hỗ trợ từ 5–500 người dùng chạy hơn 100 công việc thí nghiệm.
3. Chọn **Đi đến Marketplace để đăng ký** để được chuyển hướng đến danh sách Comet trên AWS Marketplace.
4. Chọn **Xem tùy chọn mua hàng**.

Trong biểu mẫu đăng ký, cung cấp các chi tiết cần thiết.

Khi đăng ký hoàn tất, quản trị viên có thể bắt đầu cấu hình Comet.

5. Trong khi triển khai Comet, thêm trưởng nhóm dự án của đội nhóm trường hợp sử dụng phát hiện gian lận làm quản trị viên để quản lý các hoạt động quản trị cho bảng điều khiển Comet.

Việc triển khai máy chủ Comet mất vài phút. Để biết thêm chi tiết về bước này, hãy tham khảo [Partner AI App provisioning](#).

6. Thiết lập một miền SageMaker AI theo các bước trong [Use custom setup for Amazon SageMaker AI](#). Theo thực hành tốt nhất, cung cấp một URL miền đã ký trước cho thành viên đội nhóm trường hợp sử dụng để truy cập trực tiếp vào giao diện người dùng Comet mà không cần đăng nhập vào bảng điều khiển SageMaker.
7. Thêm các thành viên đội nhóm vào miền này và kích hoạt quyền truy cập vào Comet trong khi cấu hình miền.

Bây giờ miền SageMaker AI đã sẵn sàng để người dùng đăng nhập và bắt đầu làm việc trên trường hợp sử dụng phát hiện gian lận.

### Hành trình của người dùng

Bây giờ, hãy khám phá hành trình của một nhà thực hành ML từ trường hợp sử dụng phát hiện gian lận. Người dùng hoàn thành các bước sau:

1. Đăng nhập vào miền SageMaker AI thông qua URL đã ký trước.

Bạn sẽ được chuyển hướng đến IDE SageMaker Studio. Tên người dùng và vai trò thực thi AWS Identity and Access Management (IAM) của bạn được quản trị viên cấu hình sẵn.

2. Tạo một Không gian JupyterLab theo hướng dẫn người dùng JupyterLab.
3. Bạn có thể bắt đầu làm việc trên trường hợp sử dụng phát hiện gian lận bằng cách khởi tạo một sổ Jupyter.

Quản trị viên cũng đã thiết lập quyền truy cập cần thiết vào dữ liệu thông qua một thùng Amazon Simple Storage Service (Amazon S3).

4. Để truy cập API Comet, cài đặt thư viện `comet_ml` và cấu hình các biến môi trường cần thiết như mô tả trong [Set up the Amazon SageMaker Partner AI Apps SDKs](#).
5. Để truy cập giao diện người dùng Comet, chọn Ứng dụng AI Đối tác trong bảng điều hướng SageMaker Studio và chọn Mở cho Comet.

Bây giờ, hãy cùng đi qua việc triển khai trường hợp sử dụng.

## Tổng quan về giải pháp

Trường hợp sử dụng này làm nổi bật các thách thức doanh nghiệp phổ biến: làm việc với các tập dữ liệu không cân bằng (trong ví dụ này, chỉ 0,17% giao dịch là gian lận), yêu cầu nhiều lần lặp thí nghiệm, và duy trì khả năng tái tạo hoàn toàn để tuân thủ quy định. Để theo dõi, hãy tham khảo tài liệu Comet và Hướng dẫn Bắt đầu Nhanh để biết thêm chi tiết về cài đặt và API.

Đối với trường hợp sử dụng này, chúng tôi sử dụng tập dữ liệu Phát hiện Gian lận Thẻ Tín dụng. Tập dữ liệu chứa các giao dịch thẻ tín dụng với nhãn nhị phân đại diện cho giao dịch gian lận (1) hoặc hợp pháp (0). Trong các phần sau, chúng tôi đi qua một số phần quan trọng của việc triển khai. Toàn bộ mã của việc triển khai có sẵn trong kho GitHub.

### Điều kiện tiên quyết

Là điều kiện tiên quyết, cấu hình các thư viện nhập cần thiết và các biến môi trường cho tích hợp Comet và SageMaker:

```python
# Comet ML để theo dõi thí nghiệm
import comet_ml
from comet_ml import Experiment, API, Artifact
from comet_ml.integration.sagemaker import log_sagemaker_training_job_v1
AWS_PARTNER_APP_AUTH=true
AWS_PARTNER_APP_ARN=<Your_AWS_PARTNER_APP_ARN>
COMET_API_KEY=<Your_Comet_API_Key>  # Từ Trang Chi tiết, nhấp vào Mở Comet. Ở góc trên bên phải, nhấp vào người dùng -> Khóa API
# Cấu hình Comet ML
COMET_WORKSPACE = '<your-comet-workspace-name>'
COMET_PROJECT_NAME = '<your-comet-project-name>'
```

### Chuẩn bị tập dữ liệu

Một trong những tính năng doanh nghiệp chính của Comet là phiên bản hóa tập dữ liệu tự động và theo dõi dòng dõi. Khả năng này cung cấp khả năng kiểm toán đầy đủ về dữ liệu nào được sử dụng để huấn luyện mỗi mô hình, điều này rất quan trọng để tuân thủ quy định và khả năng tái tạo. Bắt đầu bằng cách tải tập dữ liệu:

```css
# Tạo một Artifact Comet để theo dõi tập dữ liệu gốc
dataset_artifact = Artifact(
    name="fraud-dataset",
    artifact_type="dataset",
    aliases=["raw"]
)
# Thêm tệp tập dữ liệu gốc vào artifact
dataset_artifact.add_remote(s3_data_path, metadata={
    "dataset_stage": "raw",
    "dataset_split": "not_split",
    "preprocessing": "none"
})
```

### Bắt đầu một thí nghiệm Comet

Với artifact tập dữ liệu đã được tạo, bạn có thể bắt đầu theo dõi quy trình làm việc ML. Việc tạo một thí nghiệm Comet tự động bắt đầu ghi lại mã, thư viện đã cài đặt, siêu dữ liệu hệ thống, và các thông tin ngữ cảnh khác trong nền. Bạn có thể ghi lại artifact tập dữ liệu đã tạo trước đó trong thí nghiệm. Xem mã sau:

```java
# Tạo một thí nghiệm Comet mới
experiment_1 = comet_ml.Experiment(
    project_name=COMET_PROJECT_NAME,
    workspace=COMET_WORKSPACE,
)
# Ghi lại artifact tập dữ liệu vào thí nghiệm này để theo dõi dòng dõi
experiment_1.log_artifact(dataset_artifact)
```

### Tiền xử lý dữ liệu

Các bước tiếp theo là các bước tiền xử lý tiêu chuẩn, bao gồm loại bỏ các bản sao, bỏ các cột không cần thiết, chia thành các tập huấn luyện/xác thực/kiểm tra, và chuẩn hóa các đặc trưng bằng cách sử dụng `StandardScaler` của scikit-learn. Chúng tôi gói mã xử lý trong `preprocess.py` và chạy nó như một công việc Xử lý SageMaker. Xem mã sau:

```css
# Chạy công việc xử lý SageMaker
processor = SKLearnProcessor(
    framework_version='1.0-1',
    role=sagemaker.get_execution_role(),
    instance_count=1,
    instance_type='ml.t3.medium'
)
processor.run(
    code='preprocess.py',
    inputs=[ProcessingInput(source=s3_data_path, destination='/opt/ml/processing/input')],
    outputs=[ProcessingOutput(source='/opt/ml/processing/output', destination=f's3://{bucket_name}/{processed_data_prefix}')]
)
```

Sau khi bạn gửi công việc xử lý, SageMaker AI khởi động các phiên bản tính toán, xử lý và phân tích dữ liệu đầu vào, và giải phóng tài nguyên sau khi hoàn thành. Đầu ra của công việc xử lý được lưu trữ trong thùng S3 đã chỉ định.

Tiếp theo, tạo một phiên bản mới của artifact tập dữ liệu để theo dõi dữ liệu đã được xử lý. Comet tự động phiên bản hóa các artifact có cùng tên, duy trì dòng dõi hoàn chỉnh từ dữ liệu gốc đến dữ liệu đã được xử lý.

```css
# Tạo một phiên bản cập nhật của Artifact 'fraud-dataset' cho dữ liệu đã được xử lý
preprocessed_dataset_artifact = Artifact(
    name="fraud-dataset",
    artifact_type="dataset",
    aliases=["preprocessed"],
    metadata={
        "description": "Tập dữ liệu phát hiện gian lận thẻ tín dụng",
        "fraud_percentage": f"{fraud_percentage:.3f}%",
        "dataset_stage": "preprocessed",
        "preprocessing": "StandardScaler + chia train/val/test",
    }
)
# Thêm các tệp tập dữ liệu huấn luyện, xác thực, và kiểm tra làm tài sản từ xa
preprocessed_dataset_artifact.add_remote(
    uri=f's3://{bucket_name}/{processed_data_prefix}',
    logical_path='split_data'
)
# Ghi lại tập dữ liệu đã cập nhật vào thí nghiệm để theo dõi các cập nhật
experiment_1.log_artifact(preprocessed_dataset_artifact)
```

### Quy trình thí nghiệm Comet và SageMaker AI

Các nhà khoa học dữ liệu thích thí nghiệm nhanh; do đó, chúng tôi tổ chức quy trình làm việc thành các hàm tiện ích có thể tái sử dụng, có thể được gọi nhiều lần với các siêu tham số khác nhau trong khi duy trì ghi log và đánh giá nhất quán qua tất cả các lần chạy. Trong phần này, chúng tôi giới thiệu các hàm tiện ích cùng với một đoạn mã ngắn bên trong hàm:

- **train()** – Khởi tạo một công việc huấn luyện mô hình SageMaker bằng thuật toán XGBoost tích hợp sẵn của SageMaker:

```css
# Tạo bộ ước lượng SageMaker
estimator = Estimator(
    image_uri=xgboost_image,
    role=execution_role,
    instance_count=1,
    instance_type='ml.m5.large',
    output_path=model_output_path,
    sagemaker_session=sagemaker_session_obj,
    hyperparameters=hyperparameters_dict,
    max_run=1800  # Thời gian huấn luyện tối đa tính bằng giây
)
# Bắt đầu huấn luyện
estimator.fit({
    'train': train_channel,
    'validation': val_channel
})
```

- **log_training_job()** – Ghi lại siêu dữ liệu và số liệu huấn luyện và liên kết tài sản mô hình với thí nghiệm để đảm bảo khả năng truy xuất nguồn gốc hoàn chỉnh:

```code
# Ghi lại công việc huấn luyện SageMaker vào Comet
log_sagemaker_training_job_v1(
    estimator=training_estimator,
    experiment=api_experiment
)
```

- **log_model_to_comet()** – Liên kết các artifact mô hình với Comet, ghi lại siêu dữ liệu huấn luyện, và liên kết tài sản mô hình với thí nghiệm để đảm bảo khả năng truy xuất nguồn gốc hoàn chỉnh:

```code
experiment.log_remote_model(
    model_name=model_name,
    uri=model_artifact_path,
    metadata=metadata
)
```

- **deploy_and_evaluate_model()** – Thực hiện triển khai và đánh giá mô hình, và ghi log số liệu:

```code
# Triển khai đến điểm cuối
predictor = estimator.deploy(
    initial_instance_count=1,
    instance_type="ml.m5.xlarge")
# Ghi lại số liệu và hình ảnh hóa vào Comet
experiment.log_metrics(metrics)
experiment.log_confusion_matrix(matrix=cm, labels=['Normal', 'Fraud'])
# Ghi lại đường cong ROC
fpr, tpr, _ = roc_curve(y_test, y_pred_prob_as_np_array)
experiment.log_curve("roc_curve", x=fpr, y=tpr)
```

Mã dự đoán và đánh giá hoàn chỉnh có sẵn trong kho GitHub.

### Chạy các thí nghiệm

Bây giờ bạn có thể chạy nhiều thí nghiệm bằng cách gọi các hàm tiện ích với các cấu hình khác nhau và so sánh các thí nghiệm để tìm ra cài đặt tối ưu nhất cho trường hợp sử dụng phát hiện gian lận.

Đối với thí nghiệm đầu tiên, chúng tôi thiết lập một đường cơ sở bằng cách sử dụng các siêu tham số XGBoost tiêu chuẩn:

```css
# Xác định siêu tham số cho thí nghiệm đầu tiên
hyperparameters_v1 = {
    'objective': 'binary:logistic',  # Phân loại nhị phân
    'num_round': 100,               # Số vòng tăng cường
    'eval_metric': 'auc',           # Số liệu đánh giá
    'learning_rate': 0.15,          # Tỷ lệ học
    'booster': 'gbtree'             # Thuật toán tăng cường
}
# Huấn luyện mô hình
estimator_1 = train(
    model_output_path=f"s3://{bucket_name}/{model_output_prefix}/1",
    execution_role=role,
    sagemaker_session_obj=sagemaker_session,
    hyperparameters_dict=hyperparameters_v1,
    train_channel_loc=train_channel_location,
    val_channel_loc=validation_channel_location
)
# Ghi lại công việc huấn luyện và artifact mô hình
log_training_job(experiment_key=experiment_1.get_key(), training_estimator=estimator_1)
log_model_to_comet(experiment=experiment_1,
                   model_name="fraud-detection-xgb-v1",
                   model_artifact_path=estimator_1.model_data,
                   metadata=metadata)
# Triển khai và đánh giá
deploy_and_evaluate_model(experiment=experiment_1,
                         estimator=estimator_1,
                         X_test_scaled=X_test_scaled,
                         y_test=y_test
                         )
```

Trong khi chạy một thí nghiệm Comet từ một sổ Jupyter, chúng ta cần kết thúc thí nghiệm để đảm bảo mọi thứ được ghi lại và lưu trữ trong máy chủ Comet. Xem mã sau: `experiment_1.end()`

Khi thí nghiệm cơ sở hoàn tất, bạn có thể chạy các thí nghiệm bổ sung với các siêu tham số khác nhau. Kiểm tra sổ để xem chi tiết của cả hai thí nghiệm.

Khi thí nghiệm thứ hai hoàn tất, điều hướng đến giao diện người dùng Comet để so sánh hai lần chạy thí nghiệm này.

### Xem các thí nghiệm Comet trong giao diện người dùng

Để truy cập giao diện người dùng, bạn có thể tìm URL trong IDE SageMaker Studio hoặc bằng cách thực thi mã được cung cấp trong sổ: `experiment_2.url`

Ảnh chụp màn hình sau đây cho thấy giao diện người dùng thí nghiệm Comet. Các chi tiết thí nghiệm chỉ nhằm mục đích minh họa và không đại diện cho một thí nghiệm phát hiện gian lận thực tế.

Điều này kết thúc thí nghiệm phát hiện gian lận.

### Dọn dẹp

Đối với phần thí nghiệm, cơ sở hạ tầng xử lý và huấn luyện SageMaker mang tính tạm thời và tự động tắt khi công việc hoàn tất. Tuy nhiên, bạn vẫn phải dọn dẹp thủ công một vài tài nguyên để tránh chi phí không cần thiết:

1. Tắt Không gian JupyterLab SageMaker sau khi sử dụng. Để biết hướng dẫn, hãy tham khảo [Idle shutdown](#).
2. Đăng ký Comet gia hạn dựa trên hợp đồng đã chọn. Hủy hợp đồng khi không còn yêu cầu gia hạn đăng ký Comet.

## Lợi ích của tích hợp SageMaker và Comet

Sau khi đã thể hiện quy trình làm việc kỹ thuật, hãy xem xét các lợi ích rộng lớn hơn mà tích hợp này mang lại.

### Phát triển mô hình được đơn giản hóa

Sự kết hợp giữa Comet và SageMaker giảm thiểu chi phí thủ công khi chạy các thí nghiệm ML. Trong khi SageMaker xử lý việc cung cấp và mở rộng cơ sở hạ tầng, việc ghi log tự động của Comet ghi lại các siêu tham số, số liệu, mã, thư viện đã cài đặt, và hiệu suất hệ thống từ các công việc huấn luyện của bạn mà không cần cấu hình bổ sung. Điều này giúp các đội nhóm tập trung vào phát triển mô hình thay vì quản lý sổ sách thí nghiệm. Khả năng hình ảnh hóa của Comet vượt xa các biểu đồ số liệu cơ bản. Các biểu đồ tích hợp cho phép so sánh thí nghiệm nhanh chóng, và các bảng Python tùy chỉnh hỗ trợ các công cụ phân tích đặc thù cho lĩnh vực để gỡ lỗi hành vi mô hình, tối ưu hóa siêu tham số, hoặc tạo các hình ảnh hóa chuyên biệt mà các công cụ tiêu chuẩn không thể cung cấp.

### Hợp tác và quản trị doanh nghiệp

Đối với các đội nhóm doanh nghiệp, sự kết hợp này tạo ra một nền tảng trưởng thành để mở rộng các dự án ML trong các môi trường được quy định. SageMaker cung cấp các môi trường ML nhất quán, an toàn, và Comet cho phép cộng tác liền mạch với việc theo dõi dòng dõi artifact và mô hình hoàn chỉnh. Điều này giúp tránh các sai lầm tốn kém xảy ra khi các đội nhóm không thể tái tạo lại các kết quả trước đó.

### Tích hợp vòng đời ML hoàn chỉnh

Không giống như các giải pháp điểm chỉ giải quyết việc huấn luyện hoặc giám sát, Comet kết hợp với SageMaker hỗ trợ vòng đời ML hoàn chỉnh của bạn. Các mô hình có thể được đăng ký trong sổ đăng ký mô hình của Comet với việc theo dõi phiên bản đầy đủ và quản trị. SageMaker xử lý triển khai mô hình, và Comet duy trì dòng dõi và quy trình phê duyệt để thăng cấp mô hình. Khả năng giám sát sản xuất của Comet theo dõi hiệu suất mô hình và sự trôi dạt dữ liệu sau khi triển khai, tạo ra một vòng lặp khép kín nơi các thông tin chi tiết từ sản xuất thông báo cho vòng thí nghiệm SageMaker tiếp theo của bạn.

## Kết luận

Trong bài đăng này, chúng tôi đã thể hiện cách sử dụng SageMaker và Comet cùng nhau để khởi tạo các môi trường ML được quản lý hoàn toàn với khả năng tái tạo và theo dõi thí nghiệm.

Để nâng cao quy trình làm việc SageMaker của bạn với khả năng quản lý thí nghiệm toàn diện, triển khai Comet trực tiếp trong môi trường SageMaker của bạn thông qua AWS Marketplace, và chia sẻ phản hồi của bạn trong phần bình luận.

Để biết thêm thông tin về các dịch vụ và tính năng được thảo luận trong bài đăng này, hãy tham khảo các tài nguyên sau:

- [Thiết lập Ứng dụng AI Đối tác](https://docs.aws.amazon.com/sagemaker/latest/dg/partner-app-onboard.html)
- [Khởi động nhanh với Comet](https://www.comet.com/docs/v2/guides/quickstart/)
- [Sổ trên GitHub](https://github.com/aws-samples/generative-ai-on-amazon-sagemaker/tree/main/workshops/partner-ai-apps-with-sagemakerai/comet/Fraud%20Detection)
- [Tài liệu Comet](https://www.comet.com/docs/v2/)
- [Nền tảng mã nguồn mở Opik để quan sát LLM](https://github.com/comet-ml/opik)

## Về các tác giả

**Vikesh Pandey** là một Kiến trúc sư Giải pháp Chuyên gia GenAI/ML tại AWS, giúp các tổ chức tài chính lớn áp dụng và mở rộng khối lượng công việc GenAI và ML. Ông là tác giả của cuốn sách “Generative AI for financial services.” Ông có hơn 15 năm kinh nghiệm xây dựng các ứng dụng cấp doanh nghiệp trên GenAI/ML và các công nghệ liên quan. Trong thời gian rảnh rỗi, ông chơi một môn thể thao không tên với con trai mình, nằm giữa bóng đá và bóng bầu dục.

**Naufal Mir** là một Kiến trúc sư Giải pháp Chuyên gia GenAI/ML tại AWS. Ông tập trung vào việc giúp khách hàng xây dựng, huấn luyện, triển khai và di chuyển khối lượng công việc học máy sang SageMaker. Trước đây, ông làm việc tại các tổ chức tài chính phát triển và vận hành các hệ thống quy mô lớn. Ngoài công việc, ông thích chạy bộ và đạp xe đường dài.

**Sarah Ostermeier** là Quản lý Tiếp thị Sản phẩm Kỹ thuật tại Comet. Bà chuyên đưa các sản phẩm dành cho nhà phát triển GenAI và ML của Comet đến với các kỹ sư cần chúng thông qua nội dung kỹ thuật, tài nguyên giáo dục, và thông điệp sản phẩm. Trước đây, bà đã làm việc với tư cách là kỹ sư ML, nhà khoa học dữ liệu, và quản lý thành công khách hàng, giúp khách hàng triển khai và mở rộng các giải pháp AI. Ngoài công việc, bà thích đi du lịch ngoài lề, viết về AI, và đọc khoa học viễn tưởng.
