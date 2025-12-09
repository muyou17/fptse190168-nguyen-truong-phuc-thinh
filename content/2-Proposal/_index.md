---
title: "Proposal"
date: "2025-09-09"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---


[Word document version](/Proposal.docx)


# MealPlan
## Personalized Food Ingredient Sales Platform
### 1. Executive Summary  
The Personalized Food Ingredient Sales Platform focuses on enabling faster and more efficient shopping. Users register accounts to access a diverse recipe database, receive AI-driven meal suggestions based on purchase history, and order with doorstep delivery. Leveraging AWS cloud infrastructure, the platform ensures flexible scalability, high performance, and secure management.

### 2. Problem Statement  
*Current Problem*  
Customers face challenges in efficiently sourcing ingredients for meal preparation, often spending significant time searching for supplies that match recipes. Existing platforms offer recipe and menu suggestions but lack integrated ingredient purchasing, requiring users to source supplies independently, which is time-consuming and prone to errors in portioning.

*Solution*  
The platform employs Spring Boot for a robust REST API back-end handling user registration, recipe management, shopping cart, and order processing, with React delivering a user-friendly front-end featuring AI-driven meal recommendations. Data is stored in Amazon EC2 (PostgreSQL), images and static assets in Amazon S3, with front-end, back-end, AI model deployed on another Amazon EC2 instance within a secure VPC, and Route 53 managing the domain. Users can customize portions, receive personalized recipe suggestions, and order ingredients for delivery. Key features include a diverse recipe library, accurate calorie calculations, and relatively low operational costs.

*Benefits & Return on Investment*
The solution establishes a comprehensive platform for the nutrition startup to expand services while collecting user data for enhanced recommendation systems. It eliminates manual calorie calculations and fragmented shopping through an integrated system, simplifying nutrition planning and driving revenue from ingredient sales. Estimated monthly cost is $119.51 (per AWS Pricing Calculator), totaling approximately $1434.12 for 12 months. Development leverages open-source frameworks, incurring no additional hardware costs. ROI is expected within 6–12 months through user time savings and consistent order revenue.

### 3. Solution Architecture  

The website is hosted on EC2. Data is stored on an EC2 instance. Images are stored in S3. Code is pushed to GitHub for management and automatically uploaded to S3 so CodeDeploy can deploy the application to the server. CloudFront is used to improve loading performance. Cognito manages user identities. CloudTrail monitors and stores activity logs. CloudWatch monitors and manages performance and the health of AWS resources and applications. IAM grants permissions to services. Secrets Manager stores sensitive information.

![AWS Architecture](/images/2-Proposal/architecture.jpeg)

*AWS Services Used*  
- *WAF*: Protects the web application from cyber-attacks.  
- *AWS CloudFront*: Improves website loading speed.  
- *AWS EC2*: Hosts the application, NAT instance, and database.  
- *AWS VPC*: Virtual private network.  
- *AWS S3*: Stores code, log files, and images.  
- *CodeDeploy*: Deploys code to EC2.  
- *GitLab*: Hosts source code and pushes it to S3.  
- *Amazon Cognito*: Manages user authentication for the web application.  
- *IAM*: Creates users and roles.  
- *Secrets Manager*: Stores sensitive information.  
- *CloudTrail*: Monitors and stores activity logs.  
- *CloudWatch*: Monitors and manages the performance and health of AWS resources.

### 4. Technical Deployment  
*Deployment Phases*  
This project includes two main parts: developing the Spring Boot backend and the React frontend, and deploying the website on AWS using AWS services. Each part includes four phases.

1. Theory & Architecture Design: Gather web application requirements, design the system architecture (Spring Boot REST API + React frontend), and define the database schema. (January)

2. Development & Testing: Implement the Spring Boot backend with REST APIs (authentication, user management, meal/recipe CRUD, shopping cart, etc.) and build the React frontend (UI/UX, routing, forms, state management). Conduct unit tests for backend services, integration tests for API endpoints, and frontend tests (Jest/React Testing Library). (January–February)

3. Cost Estimation & Feasibility Check: Use the AWS Pricing Calculator to estimate costs for EC2 (backend hosting), EC2 (database), S3 (static files and images), VPC (networking), and Route 53 (domain). Adjust as needed. (February)

4. AWS Integration: Integrate AWS services into the application. Deploy the website on EC2, store images on S3, configure EC2 for the database, use VPC for networking, Route 53 for domain management, and set up CI/CD pipelines (GitHub Actions or AWS CodePipeline). Perform staging tests before official release. (March)

*Technical Requirements*  
1. Backend (Spring Boot): REST APIs for authentication, user management, meal/recipe CRUD, shopping cart, and order processing. Includes security (JWT, Spring Security).  
2. Frontend (React): Responsive web application with a user-friendly UI/UX integrated with the backend API.  
3. Database (EC2): Relational database (MySQL/PostgreSQL) hosted on EC2, storing users, recipes, shopping carts, and order data.  
4. Storage (AWS S3): Used to store user-uploaded images.  
5. Hosting & Networking (AWS EC2 & AWS VPC): Application deployed on EC2 instances.  
6. CI/CD (GitHub Actions or AWS CodePipeline): Automated build and deployment pipeline for backend and frontend.  
7. Authentication & Security: JWT authentication and HTTPS configuration; optional AWS Cognito for user access management.

### 5. Roadmap & Deployment Milestones  
- **January**: Build theoretical foundation and draw architecture (Spring Boot backend + React frontend design, database schema). Begin initial backend and frontend development.  
- **February**: Continue backend and frontend development, perform unit and integration tests. Use AWS Pricing Calculator to evaluate hosting costs and refine architecture for cost efficiency.  
- **March**: Integrate AWS services, configure CI/CD pipelines, conduct staging tests, and deploy the website to production.  
- **Post-launch**: Up to 3 months for maintenance, optimization, and feature improvements.

### 6. Cost Estimate  
Costs can be viewed via the [AWS Pricing Calculator](https://calculator.aws/#/estimate?id=0a74d43130ed1b5856ad5b775a746e30d39583db).  

### **AWS Services**  
- AWS WAF: $11.6/month  
- Application Load Balancer (ALB): $18.63/month  
- Amazon EC2 Application: $19.27/month  
- Amazon EC2 Data Tier: $9.64/month  
- Amazon EC2 NAT Instances: $19.27/month  
- Amazon S3: $3.72/month  
- AWS CodeDeploy: $0  
- AWS Secrets Manager: $0.4/month  
- Amazon Cognito: $14.25/month  
- Amazon CloudWatch: $4.91/month  
- AWS CloudTrail: $1.77/month  
- VPC Endpoints: $16.05/month  

**Total: $119.51/month, $1,434.12/year**

### 7. Risk Assessment  
*Risk Matrix*  
- Network loss: Medium impact / Medium probability.  
- EC2 / ALB / AZ failure: High impact / Low–medium probability.  
- Secret leakage: High impact / Low–medium probability.  
- Cost overrun: Medium impact / Medium probability.  

*Mitigation Strategies*  
- **Availability**: Multi-AZ + Auto Scaling + ALB; health checks; caching with CloudFront; Route 53 failover.  
- **Security**: IAM with least-privilege; Secrets Manager with rotation and access logging; WAF/Shield protection.  
- **Cost Management**: AWS Budgets & Cost Explorer; rightsizing; Reserved or Spot Instances where appropriate.

*Fallback Plan*  
- Automatic rollback using CodeDeploy.  
- Manual fallback: GitLab runner → prebuilt AMIs or ASGs.

### 8. Expected Outcomes  
- Fully automated CI/CD (GitLab → CodePipeline → CodeBuild → CodeDeploy) reduces manual errors.  
- Multi-layer security (IAM, WAF, Secrets Manager) with auditing via CloudTrail.
