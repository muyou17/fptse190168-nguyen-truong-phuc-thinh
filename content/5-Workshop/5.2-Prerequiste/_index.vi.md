---
title : "Các bước chuẩn bị"
date :  2025-12-07

 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

#### IAM permissions
Gắn IAM permission policy sau vào tài khoản aws user của bạn để triển khai và dọn dẹp tài nguyên trong workshop này. Do tôi đang thực hiện workshop này lần đầu nên tôi sẽ gắn full quyền cho các quyền tôi đã chọn
```
{
    {
	"Version": "2012-10-17",
	"Statement": [
		{
			"Sid": "VisualEditor0",
			"Effect": "Allow",
			"Action": [
				"polly:*",
				"s3:*",
				"logs:*"
			],
			"Resource": "*"
		}
	]
}

}
```

