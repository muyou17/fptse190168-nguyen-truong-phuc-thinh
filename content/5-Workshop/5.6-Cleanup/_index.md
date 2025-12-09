---
title : "Clean up"
date : 2025-12-07


weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---
Congratulations on completing this workshop! 
In this workshop, you learned architecture patterns for building Serverless Event-driven applications on AWS.

By configuring S3 Event Notifications, you enabled an automated workflow where compute resources (AWS Lambda) react instantly to data ingestion without the need for manual intervention or server management.

By integrating Amazon Polly, you successfully leveraged Managed AI Services to transform text into lifelike speech, demonstrating how to add complex machine learning capabilities to your application with minimal code.

#### Clean up
1. Delete S3 buckets
+ Open S3 console
+ Open s3-demo-bucket
+ Choose 2 folders ```input``` and ```output```
+ Click **delete** and confirm 
![delete s3](/images/5-Workshop/5.6-Cleanup/delete-folder.jpg)
+ Then choose the bucket we created for the lab, click and confirm empty. Click delete and confirm delete.
![delete s3](/images/5-Workshop/5.6-Cleanup/delete-s3.jpg)


2. Delete Lambda function
+ Open the lambda console 
+ Find **workshop1** and click **action**
+ Choose **Delete** and confirm 

![delete lambda](/images/5-Workshop/5.6-Cleanup/delete-lambda.jpg)

3. Delete IAM role
+ Open IAM console
+ Select **Roles** from the menu on the left.
+ Find the role **PollyLambdaRole**.
+ Select the role and click **Delete**.
![delete s3](/images/5-Workshop/5.6-Cleanup/delete-IAM.jpg)