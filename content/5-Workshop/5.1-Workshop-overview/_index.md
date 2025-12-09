---
title: "Workshop Overview"
date: 2025-12-07
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

# INTRODUCTION

## Introduction to S3 Event Notifications

- **S3 Event Notifications** is a feature that allows Amazon S3 to automatically send notifications when specific events occur in your Bucket (for example: when a new file is uploaded, deleted, or copied).
- S3 can send notifications to various destinations such as AWS Lambda, Amazon SNS, or Amazon SQS.
- In this workshop, we use the `PutObject` event to automatically trigger a Lambda function as soon as a text file is uploaded to S3.

## Introduction to Amazon Polly

- **Amazon Polly** is a Text-to-Speech service that uses AWS's advanced Deep Learning technology.
- Polly supports over 60 voices in more than 20 languages, including Vietnamese, with natural human-like audio quality.
- This service is fully managed, so you don't need to worry about managing infrastructure or scalability.

## Workshop Overview

In this workshop, you will build a **Serverless Text-to-Speech Converter** application. The system operates fully automatically based on an Event-driven Architecture model:

- **Amazon S3 (Input Bucket)**: Stores input text files (`.txt`) uploaded by users.
- **S3 Event Notifications**: Detects new file upload events and triggers AWS Lambda.
- **AWS Lambda**: The central processor that orchestrates data flow between S3 and Polly. Lambda reads the text file, calls the Polly API for conversion, and saves the result.
- **Amazon Polly**: The AI service that converts text into audio with natural voice.
- **Amazon S3 (Output Bucket)**: Stores output audio files (`.mp3`) after processing is complete.

The entire process runs automatically without manual intervention, helping save time and operational costs.

## System Architecture

The model below describes the detailed architecture and data flow of the Text-to-Speech Converter system:

![Architecture Diagram](/images/5-Workshop/5.1-Workshop-overview/diagram1.jpg)


**Workflow:**

1. **Upload file**: User uploads a text file (`.txt`) to **Amazon S3 Input Bucket**.

2. **Event trigger**: S3 detects the `PutObject` event (new file), automatically sends an event notification to trigger **AWS Lambda Function**.

3. **Lambda processing**: Lambda function is triggered and performs:
   - Reads the text file content from S3 Input Bucket
   - Sends the text content to **Amazon Polly** API along with parameters (voice ID, output format)

4. **Polly conversion**: Amazon Polly receives the request, processes the text, and returns an audio stream to Lambda.

5. **Save result**: Lambda receives the audio stream from Polly and saves it as an `.mp3` file to **Amazon S3 Output Bucket**.

6. **Complete**: User can download the MP3 file from S3 Output Bucket for use.

## Benefits of Serverless Architecture

- **No server management**: AWS automatically handles scaling, patching, and high availability.
- **Cost optimization**: Pay only when processing requests (pay-per-use model).
- **Auto-scaling**: The system can handle from a few requests to millions of requests without additional configuration.
- **Fast deployment**: Focus on application logic instead of managing infrastructure.