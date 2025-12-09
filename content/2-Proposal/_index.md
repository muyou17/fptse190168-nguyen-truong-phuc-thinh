---
title: "Proposal"
date: "2025-10-02"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Personalized Food Ingredient Sales Platform
## Ingredient Shopping with AI-Powered Recipe Recommendations for Smarter Home Cooking
### 1. Executive Summary
The Personalized Food Ingredient Sales Platform focuses on enabling faster and more efficient ingredient shopping by providing fresh ingredients alongside customized recipes. Users register accounts to access a diverse recipe database, receive AI-driven meal suggestions based on purchase history and preferences, and order ingredients with doorstep delivery. The system allows portion customization and precise calorie/macro calculations for convenience. Leveraging AWS cloud infrastructure, the platform ensures flexible scalability, high performance, and secure management.

### 2. Problem Statement
#### What’s the Problem?
Customers face challenges in efficiently sourcing ingredients for meal preparation, often spending significant time searching for supplies that match recipes. Existing platforms offer recipe and menu suggestions but lack integrated ingredient purchasing, requiring users to source supplies independently, which is time-consuming and prone to errors in portioning.

#### The Solution
The platform employs Spring Boot for a robust REST API back-end handling user registration, recipe management, shopping cart, and order processing, with React delivering a user-friendly front-end featuring AI-driven meal recommendations. Data is stored in Amazon EC2 (PostgreSQL), images and static assets in Amazon S3, with front-end, back-end, AI model deployed on another Amazon EC2 instance within a secure VPC, and Route 53 managing the domain. Users can customize portions, receive personalized recipe suggestions, and order ingredients for delivery. Key features include a diverse recipe library, accurate calorie calculations, and relatively low operational costs.

#### Benefits and Return on Investment
The solution establishes a comprehensive platform for the nutrition startup to expand services while collecting user data for enhanced recommendation systems. It eliminates manual calorie calculations and fragmented shopping through an integrated system, simplifying nutrition planning and driving revenue from ingredient sales. Estimated monthly costs are $21.13 (per AWS Pricing Calculator), totaling approximately $253.56 for 12 months. Development leverages open-source frameworks, incurring no additional hardware costs. ROI is expected within 6–12 months through user time savings and consistent order revenue.

### 3. Solution Architecture
The platform leverages a secure, scalable, and automated AWS architecture to host and deploy web applications following CI/CD best practices. It integrates infrastructure automation, high availability, and global content delivery using AWS services. Source code is managed in GitLab and automatically deployed to private EC2 instances via CodePipeline and CodeDeploy. Security is enforced at multiple layers through IAM, WAF, and Secrets Manager, while CloudFront, Elastic Load Balancing, and EC2 Auto Scaling ensure performance and resilience. Monitoring and logging are managed through CloudWatch and CloudTrail.

![Architecture](/images/2-Proposal/architecture.png)

#### AWS Services Used
- **Amazon Route 53:** Manages DNS and routes user traffic to CloudFront.
- **AWS WAF:** Protects the application from common web exploits and DDoS attacks.
- **Amazon CloudFront:** Delivers cached content globally for lower latency.
- **Elastic Load Balancing (ALB):** Distributes traffic across EC2 instances in multiple Availability Zones.
- **Amazon EC2 (Auto Scaling):** Hosts application and database workloads with dynamic scaling for performance and cost optimization.
- **Amazon S3:** Stores static assets, media files, and deployment artifacts.
- **AWS CodePipeline:** Automates build, test, and deployment workflows.
- **AWS CodeBuild:** Builds and tests source code before deployment.
- **AWS CodeDeploy:** Automates deployment to EC2 instances.
- **AWS IAM:** Controls permissions and roles securely across AWS services.
- **AWS Secrets Manager:** Stores and manages sensitive credentials securely.
- **Amazon Cognito:** Manages user authentication and access control.
- **Amazon CloudWatch:** Monitors system and application performance.
- **AWS CloudTrail:** Logs and audits API activity across the AWS environment.

#### Component Design
- **Networking**: Deployed within a VPC spanning multiple Availability Zones, using private subnets for the application and database to ensure security and redundancy.
- **Traffic Management**: Users access the system via Route 53, which routes requests to CloudFront for caching and delivery. WAF filters malicious requests before forwarding them to the Application Load Balancer.
- **Application Tier**: The Auto Scaling Group runs EC2 instances in private subnets. The ALB evenly distributes traffic to ensure high availability and fault tolerance.
- **Data Tier**: The data tier is isolated within private subnets, communicating securely with the application tier to avoid public exposure.
- **Storage**: Amazon S3 stores static files, mostly images, and build artifacts from the CI/CD pipeline.
- **CI/CD Pipeline**: Developers push code to GitLab, triggering AWS CodePipeline.
**CodeBuild** compiles and tests the code, stores artifacts in **S3**, and **CodeDeploy** handles automated deployments to EC2 instances.
- **Security**: Access is managed through IAM and Cognito, while Secrets Manager protects sensitive credentials like database passwords and API keys.
- **Monitoring & Logging**: CloudWatch provides monitoring, alerts, and performance insights.
**CloudTrail records all API calls for auditing and compliance purposes.

### 4. Technical Implementation

**Implementation Phases**
This project has two parts—developing Spring Boot back-end and React frontend and deploy the website to AWS using AWS services each following 4 phases.
- **Build Theory and Draw Architecture:** Gather requirements for the web application, design system architecture (Spring Boot REST API + React front-end), and define the database schema (Month 1).
- **Develop, Test:** Implement the Spring Boot backend with REST APIs (authentication, user management, meal/recipe CRUD, shopping cart, etc.), and build the React frontend (UI/UX, routing, forms, state management). Conduct unit tests for backend services, integration tests for API endpoints, and frontend tests (Jest/React Testing Library). (Month 1–2)
- **Calculate Price and Check Practicality:** Use AWS Pricing Calculator to estimate costs for EC2 (backend hosting), RDS (database), S3 (static files and images), VPC (network), Route53 (domain) then adjust if needed (Month 2).
- **AWS Integration:** Integrate AWS services into the application. Deploy the website on **EC2**, store pictures on **S3**, configure **RDS** for database, using **VPC** for managing network, **Route53** for domain, and set up CI/CD pipelines (GitHub Actions or AWS CodePipeline). Perform staging tests before final production release. (Month 3)


### Technical Requirements

- **Back-end (Spring Boot):** REST API for authentication, user management, meal/recipe CRUD, shopping cart, and order processing. Includes security (JWT, Spring Security), validation, and integration with AWS RDS.
- **Front-end (React):** Responsive web application with user-friendly UI/UX, routing (React Router), state management (Redux or Context API), and API integration with the Spring Boot backend.
- **Database (AWS RDS):** Relational database (MySQL/PostgreSQL) to store users, recipes, shopping cart, and order data.
- **Storage (AWS S3):** Used for hosting static React build files and storing user-uploaded images (e.g., recipe pictures, profile images).
- **Hosting & Networking (AWS EC2 & AWS VPC):** Spring Boot backend deployed on EC2 instances, secured and isolated within a VPC for networking and access control.
- **Domain Management (AWS Route 53):** Custom domain configuration and DNS management for the website.
- **CI/CD (GitHub Actions or AWS CodePipeline):** Automated build, test, and deployment pipeline for both back-end and front-end.
- **Testing Tools:** JUnit/Mockito for backend unit and integration tests, Jest/React Testing Library for front-end testing, Postman for API testing.
- **Authentication & Security:** JWT authentication and HTTPS configuration; optional AWS Cognito for managing user access.

---

### 5. Timeline & Milestones
- **Pre-Project (Month 1):** Team formation, role assignment (back-end, front-end, AWS deployment), requirement gathering, and drafting the initial system architecture.
- **Project Execution (Months 1–3):**
  - **Month 1:** Build theory and draw architecture (Spring Boot backend + React frontend design, database schema). Begin initial development of backend and frontend.
  - **Month 2:** Continue backend and frontend development, perform unit and integration testing. Use AWS Pricing Calculator to evaluate hosting costs and refine architecture for cost-effectiveness.
  - **Month 3:** Integrate AWS services (EC2, S3, VPC, Route 53), configure CI/CD pipelines, conduct staging tests, and deploy the website to production.
- **Post-Launch:** Up to 9 months for maintenance, optimization, and feature enhancements (e.g., scaling backend, improving UI/UX, adding new features).

### 6. Budget Estimation
- Amazon Route 53: $0.9/month (1 Hosted zones, 1 millions per month standard queries, 1 Basic Checks Within AWS)
- AWS WAF: $11.6/month (1/month Number of Web Access Control Lists (Web ACLs) utilized, 4/month Number of Rules added per Web ACL, 2/month Number of Managed Rule Groups per Web ACL, 1 million/month Number of web requests received across all web ACLs)
- Amazon CloudFront: $0.06/month (Asia Pacific: 1 GB/month Data transfer out to origin)
- Application Load Balancer (ALB): $18.63/month (1 Number of Application Load Balancers, 10 GB per month Processed bytes(EC2 instances and IP addresses as targets), 1 per second Average number of new connections per ALB, 1 seconds Average connection duration, 2 Average number of requests per second per ALB, 3 Average number of rule evaluations per request)
- Amazon EC2 Application: $52.92/month (Shared Instances, Linux, Daily spike traffic, 1 Baseline, 2 Peak, 3 hours duration of peaks, m6g.large, EC2 Instance Savings Plans)
- Amazon EC2 Data Tier: $9.78/month (Shared Instances, Linux, t4g.small, EC2 Instances Savings Plans)
- Amazon S3: $3.72/month (1GB/month S3 Standard storage, 1MB S3 Standard Average Object Size, 10000  PUT, COPY, POST, LIST requests to S3 Standard, 100000 GET, SELECT, and all other requests from S3 Standard, 1GB per month Inbound Data Transfer, 30GB per month Outbound Data Transfer)
- AWS CodePipeline: $0 (1 Number of active pipelines of type V1 used per account per month, 60 Number of action execution minutes used in pipeline of type V2 per account per month)
- AWS CodeBuild: $0 (On-Demand Lambda AWS CodeBuild Compute TypeType, 3 Number of builds in a month, 10 secons Average build duration, lambda.arm.2GB, Linux)
- AWS CodeDeploy: 0$
- AWS Secrets Manager: $0.4/month (1 Number of secrets, 30 days Average duration of each secret, 5/month API calls)
- Amazon Cognito: $14.25/month (1000 Number of monthly active users (MAU), Disabled Advanced security features, 1000 Number of monthly active users (MAU) who sign in through SAML or OIDC federation)
- Amazon CloudWatch: $4.91/month (7 Number of Metrics (includes detailed and custom metrics), 7 GetMetricData: Number of metrics requested, 3 GetMetricWidgetImage: Number of metrics requested, 5 Number of other API requests, 1GB Standard Logs: Data Ingested, 1GB Infrequent Access Logs: Data Ingested, 1GB Value of Standard Logs Delivered to CloudWatch Logs, 1GB Infrequent Access Logs Delivered to CloudWatch Logs, Yes to Store Logs: Assuming 1 month retention, Disabled Logs Delivered to S3: Format Converted to Apache Parquet, 2 Number of Dashboards, 5 Number of Standard Resolution Alarm Metrics, 1000 Monthly visitors to your web application, 20 Number of RUM events per visit, 100 Percentage of RUM event)
- AWS CloudTrail: $1.77/month(1/month Write management events, 1 Write management trails, 1/month Read management events, 1 Read management trails, 1 S3 operations, 1 S3 trails, 1GB Data ingested - CloudTrail, 1GB Data Retention - One-year extendable retention pricing)
- VPC Endpoints: $16.05/month(1 Number of In-use public IPv4 addresses, 20GB/month Intra-Region Data Transfer, 100GB/month Outbound Data Transfer)

Total: $134.99/month or $1,619.88/year.

### 7. Risk Assessment

*Risk Matrix*
- EC2 and RDS downtime: Medium impact, medium probability
- Database bottlenecks: Medium impact, during peak traffic
- Cost overruns: High impact, very low probability
- Security threats: High impact, low probability

*Mitigation Strategies*
- Enable Auto Scaling for EC2 instances.
- Set up AWS Budget Alerts to monitor and control spending.
- Implement IAM roles, Security Groups, and AWS WAF for enhanced security.

### Contingency Plans
- Failover & Recovery: Promote RDS read replica or restore snapshot; redeploy EC2 from AMI.

## 8. Expected Outcomes

*Technical Improvements*
- CI/CD pipeline + Infrastructure as Code for reliable deployment and rollback.
- Monitoring and alerts with CloudWatch for system health.

*Long-term Value*
- Collect user preference data to build recommendation systems for personalized meal suggestions.
- Scale infrastructure to support thousands of users.
- Potential integration with delivery services.
