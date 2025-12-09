---
title : "Event 2"
date : "2025-09-09"
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---


# Summary Report: “DevOps on AWS”

## Event Objectives
* Understand the core principles of DevOps culture and key performance metrics (DORA, MTTR).
* Master the AWS CI/CD toolchain for automating build, test, and deployment.
* Learn Infrastructure as Code (IaC) concepts using CloudFormation and AWS CDK.
* Explore containerization strategies using ECR, ECS, EKS, and App Runner.
* Implement full-stack observability and monitoring with CloudWatch and X-Ray.

## Speakers
* **AWS DevOps Specialists**
* **Senior Solutions Architects**

## Key Highlights

### DevOps Culture & Mindset
* **Recap:** Integration with AI/ML concepts from previous sessions.
* **Metrics Matter:** Focus on Deployment Frequency, Lead Time for Changes, Mean Time to Restore (MTTR), and Change Failure Rate (DORA metrics).
* **Cultural Shift:** Moving from siloed teams to shared responsibility.

### AWS CI/CD Pipeline
* **Source Control:** Utilizing **AWS CodeCommit** and implementing Git strategies like GitFlow and Trunk-based development.
* **Build & Test:** Configuring **AWS CodeBuild** for automated testing and compilation.
* **Deployment:** Using **AWS CodeDeploy** to implement safe deployment strategies:
  * **Blue/Green:** Reduces downtime and risk.
  * **Canary:** Gradual rollout to a small subset of users.
  * **Rolling:** Update instances incrementally.
* **Orchestration:** Tying it all together with **AWS CodePipeline**.

### Infrastructure as Code (IaC)
* **AWS CloudFormation:** Defining infrastructure using templates, stacks, and managing configuration drift.
* **AWS CDK (Cloud Development Kit):** Using familiar programming languages to define cloud resources as code constructs.
* **Comparison:** Choosing between declarative templates (CloudFormation) vs. imperative code (CDK) based on team skills.

### Container Services & Observability
* **Container Management:** Storing images in **Amazon ECR** with lifecycle policies.
* **Orchestration:** Choosing between **Amazon ECS** (simpler, AWS-native) and **Amazon EKS** (Kubernetes standard), or **AWS App Runner** for simplified PaaS-like deployment.
* **Monitoring:** Using **Amazon CloudWatch** for metrics/alarms and **AWS X-Ray** for distributed tracing to identify performance bottlenecks.

## Key Takeaways

### DevOps Strategy
* **Automation First:** Manual deployments are error-prone; everything from infrastructure to code deployment should be automated.
* **Measurement:** You cannot improve what you do not measure. Use DORA metrics to track velocity and stability.
* **Shift Left:** Integrate testing and security early in the CI/CD pipeline, not at the end.

### Technical Architecture
* **Immutable Infrastructure:** Treat servers as disposable resources; replace them rather than patching them in place.
* **Containerization:** Decouple applications from the underlying OS to ensure consistency across environments (Dev, Test, Prod).
* **Observability:** Moving beyond simple "up/down" monitoring to deep insights using distributed tracing.

### Applying to Work
* **Implement CI/CD:** Set up a CodePipeline for current projects to automate the build/deploy process for Spring Boot/React applications.
* **Adopt IaC:** Start defining AWS resources (databases, S3 buckets, Cognito User Pools) using **AWS CDK** instead of the console.
* **Containerize:** Dockerize existing microservices and push images to ECR.
* **Enhance Monitoring:** Add X-Ray instrumentation to backend services to visualize API latency and database query performance.

## Event Experience

Attending the **“DevOps on AWS”** workshop provided a practical roadmap for automating the software delivery lifecycle. It bridged the gap between writing code and running it reliably in production. Key experiences included:

### Learning from experts
* Gained clarity on the "Alphabet Soup" of AWS tools (CodeCommit, CodeBuild, CodeDeploy, CodePipeline) and how they integrate.
* Understood the strategic value of **DORA metrics** in justifying DevOps investments to business stakeholders.

### Hands-on technical exposure
* **CI/CD Walkthrough:** The demo of a full pipeline showed exactly how code changes trigger builds and deployments automatically.
* **IaC Implementation:** Seeing **AWS CDK** in action was a highlight—writing infrastructure in Java/TypeScript is much more intuitive for developers than writing JSON/YAML templates.
* **Deployment Strategies:** Visualizing **Blue/Green deployments** demonstrated how to release updates with zero downtime.

### Networking and discussions
* Discussed the trade-offs between **ECS and EKS** with peers, realizing that for many projects, ECS or App Runner provides a faster path to production with less overhead.
* Exchanged ideas on how to handle **database migrations** within a CI/CD pipeline.

### Lessons learned
* **Drift Detection** in CloudFormation is critical for maintaining infrastructure integrity.
* **Observability** is not optional for microservices; without X-Ray, debugging distributed architectures is nearly impossible.
* A solid DevOps foundation significantly reduces **Mean Time to Recovery (MTTR)**, allowing teams to innovate faster with less fear of breaking production.

### Event photo
![Photo](/images/aws2.jpg)