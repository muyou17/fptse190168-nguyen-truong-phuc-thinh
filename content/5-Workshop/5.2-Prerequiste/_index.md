---
title : "Prerequiste"
date :  2025-12-07

 
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

#### IAM permissions
Add the following IAM permission policy to your user account to deploy and cleanup this workshop.Since I am performing this workshop for the first time, I will grant full access to the selected permissions.
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

