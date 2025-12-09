---
title : "Tạo Lambda"
date :  2025-12-07

 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

#### Tạo hàm Lambda

1. Truy cập vào **Lambda management console**
2. Tại giao diện Dashboard, chọn **Create function**

![Create lambda](/images/5-Workshop/5.3-S3-vpc/create-lambda.jpg)

3. Trong giao diện **Create Lambda function**
+ **Name the lambda**: chọn một tên chưa trùng lặp (gợi ý: số bài lab và tên của bạn)

![Lambda name](/images/5-Workshop/5.3-S3-vpc/lambda-name.jpg)

+ **Runtime**: chọn python 3.13 hoặc mới nhất
![Runtime](/images/5-Workshop/5.3-S3-vpc/runtime.jpg)
+ Mở rộng phần **Change default execution role**
+ Tại mục Execution role, chọn Use an existing role và chọn **PollyLambdaRole**
![executionrole](/images/5-Workshop/5.3-S3-vpc/executionrole.jpg)
+ Kéo xuống dưới và nhấn **Create function** (hoặc Create lambda)
+ Tạo thành công Lambda function.

![Success](/images/5-Workshop/5.3-S3-vpc/lambda-success.jpg)

#### Tạo code Lambda
+ Trong bài thực hành này, chúng ta sẽ sử dụng code để chuyển đổi một thư mục văn bản thành thư mục giọng nói (Text-to-Speech) bằng dịch vụ Amazon Polly.
+ Kéo xuống phần **Code source**.
+ Xóa tất cả code hiện có trong file ```lambda_function.py```. 
+ Chúng ta sẽ sử dụng đoạn code như sau:
```python
import json
import boto3
import os

s3 = boto3.client('s3')
polly = boto3.client('polly')

def lambda_handler(event, context):
    try:
        # 1. Lấy thông tin file vừa upload
        record = event['Records'][0]
        bucket_name = record['s3']['bucket']['name']
        object_key = record['s3']['object']['key'] # VD: input/hello.txt
        
        print(f"Processing file: {object_key}")
        
        # 2. Đọc nội dung file text
        file_obj = s3.get_object(Bucket=bucket_name, Key=object_key)
        text_content = file_obj['Body'].read().decode('utf-8')
        
        # 3. Gọi Polly để chuyển đổi văn bản sang giọng nói (Giọng: Joanna)
        response = polly.synthesize_speech(
            Text=text_content,
            OutputFormat='mp3',
            VoiceId='Joanna'
        )
        
        # 4. Lưu file MP3 sang thư mục output
        # Đổi tên từ input/abc.txt thành output/abc.mp3
        new_key = object_key.replace("input/", "output/").replace(".txt", ".mp3")
        
        if "AudioStream" in response:
            with response["AudioStream"] as stream:
                s3.put_object(
                    Bucket=bucket_name,
                    Key=new_key,
                    Body=stream.read(),
                    ContentType='audio/mpeg'
                )
        
        return "Done!"
    except Exception as e:
        print(e)
        raise e
```
+ Nhấn nút Deploy.
![deploy](/images/5-Workshop/5.3-S3-vpc/deploy.jpg)
#### Tạo Trigger
+ Ngay phía trên phần code, nhấn nút **+ Add trigger**.
![addtrigger](/images/5-Workshop/5.3-S3-vpc/add-trigger.jpg)
+ Source: Chọn **S3**.
+ Bucket: Chọn bucket **s3-demo-text**.
![addtrigger](/images/5-Workshop/5.3-S3-vpc/create-trigger.jpg)
+ Event types: Chọn **All object create events**.
![addtrigger](/images/5-Workshop/5.3-S3-vpc/event-types.jpg)
+ Prefix: Nhập ``input/``
+ ⚠️ Lưu ý: Bạn bắt buộc phải nhập ``input/``. Nếu để trống, Lambda sẽ kích hoạt ngay cả khi file MP3 được tạo ra -> Gây ra vòng lặp vô tận -> Tăng chi phí.
+ Suffix: Nhập ``.txt``
![addtrigger](/images/5-Workshop/5.3-S3-vpc/prefix-suffix.jpg)
+ Tích vào ô **"I acknowledge..."** -> Nhấn **Add**.
![addtrigger](/images/5-Workshop/5.3-S3-vpc/add-success.jpg)