---
title: "Workshop"
date: 2025-12-07


weight: 5
chapter: false
pre: " <b> 5. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

# Automated Text-to-Speech Converter using Serverless

#### Overview

**Event-Driven Architecture** allows you to build applications that automatically respond to changes in state, such as a file upload, without provisioning or managing servers.

In this lab, you will learn how to create, configure, and test a Serverless pipeline that automatically converts text files into lifelike speech audio using AWS Managed Services.

You will utilize two key mechanisms to process data asynchronously:
+ **S3 Event Notifications** - Configure Amazon S3 to act as an event source. It will automatically trigger a compute function whenever a new object is uploaded to a specific input folder.
+ **Managed AI Services** - Leverage Amazon Polly to synthesize speech from text. This allows you to add deep learning capabilities to your application through simple API calls, without needing data science expertise.

#### Content

1. [Workshop overview](5.1-Workshop-overview)
2. [Prerequisite](5.2-Prerequiste/)
3. [Build Core Logic (Lambda)](5.3-S3/)
4. [Configure Storage (S3)](5.4-lambda/)
5. [Testing Result](5.5-testing/)
6. [Clean up](5.6-Cleanup/)