---
title : "Create Lambda"
date :  2025-12-07

 
weight : 2
chapter : false
pre : " <b> 5.3.2 </b> "
---

#### Create Lambda function

1. Navigate to **Lambda management console**
2. In the Dashboard console, choose **Create function**

![Create lambda](/images/5-Workshop/5.3-S3-vpc/create-lambda.jpg)

3. In **the Create Lambda function console**
+ **Name the lambda**: choose a name that hasn't been given to any bucket globally (hint: lab number and your name)

![Lambda name](/images/5-Workshop/5.3-S3-vpc/lambda-name.jpg)

+ **Runtime**: choose python 3.13 or latest
![Runtime](/images/5-Workshop/5.3-S3-vpc/runtime.jpg)
+ Expand the **Change default execution role** section
+ In Execution role, choose Use an existing role and choose **PollyLambdaRole**
![executionrole](/images/5-Workshop/5.3-S3-vpc/executionrole.jpg)
+ Scroll down and choose **Create lambda**
+ Successfully create S3 bucket.

![Success](/images/5-Workshop/5.3-S3-vpc/lambda-success.jpg)

#### Create code Lambda
+ In this workshop, we will use the code to convert a text folder into a voice folder (Text-to-Speech) using Amazon Polly service.
+ Scroll down to the **Code source** section.
+ Delete all existing code in the ```lambda_function.py``` file. 
+ We will use the code as follows:
```python
import json
import boto3
import os

s3 = boto3.client('s3')
polly = boto3.client('polly')

def lambda_handler(event, context):
    try:
        # 1. Get uploaded file info
        record = event['Records'][0]
        bucket_name = record['s3']['bucket']['name']
        object_key = record['s3']['object']['key'] # Ex: input/hello.txt
        
        print(f"Processing file: {object_key}")
        
        # 2. Read text file content
        file_obj = s3.get_object(Bucket=bucket_name, Key=object_key)
        text_content = file_obj['Body'].read().decode('utf-8')
        
        # 3. Call Polly to convert text to speech (Voice: Joanna)
        response = polly.synthesize_speech(
            Text=text_content,
            OutputFormat='mp3',
            VoiceId='Joanna'
        )
        
        # 4. Save MP3 file to output folder
        # Change name from input/abc.txt to output/abc.mp3
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
+ Click the **Deploy** button.
![deploy](/images/5-Workshop/5.3-S3-vpc/deploy.jpg)
#### Create Trigger
+ Right above the code section, click the **+ Add trigger** button.
![addtrigger](/images/5-Workshop/5.3-S3-vpc/add-trigger.jpg)
+ Source: Select **S3**.
+ Bucket: Select the bucket **s3-demo-text**.
![addtrigger](/images/5-Workshop/5.3-S3-vpc/create-trigger.jpg)
+ Event types: Select **All object create events**.
![addtrigger](/images/5-Workshop/5.3-S3-vpc/event-types.jpg)
+ Prefix: Enter ```input/```
+ ⚠️ Note: You must enter ``input/``. If left blank, Lambda will trigger even when the MP3 file is created -> Causing an infinite loop -> Increasing costs.
+ Suffix: Enter ```.txt```
![addtrigger](/images/5-Workshop/5.3-S3-vpc/prefix-suffix.jpg)
+ Check the box **"I acknowledge..."** -> Click **Add**.
![addtrigger](/images/5-Workshop/5.3-S3-vpc/add-success.jpg)