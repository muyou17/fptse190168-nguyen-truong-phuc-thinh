---
title: "Proposal"
date: "2025-10-02"
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

# Personalized Food Ingredient Sales Platform

### 1. Executive Summary
The Personalized Food Ingredient Sales Platform focuses on enabling faster and more efficient ingredient shopping by providing fresh ingredients alongside customized recipes. Users register accounts to access a diverse recipe database, receive AI-driven meal suggestions based on purchase history and preferences, and order ingredients with doorstep delivery. The system allows portion customization and precise calorie/macro calculations for convenience. Leveraging AWS cloud infrastructure, the platform ensures flexible scalability, high performance, and secure management.

### 2. Problem Statement
### What’s the Problem?
Customers face challenges in efficiently sourcing ingredients for meal preparation, often spending significant time searching for supplies that match recipes. Existing platforms offer recipe and menu suggestions but lack integrated ingredient purchasing, requiring users to source supplies independently, which is time-consuming and prone to errors in portioning.

### The Solution
The platform employs Spring Boot for a robust REST API backend handling user registration, recipe management, shopping cart, and order processing, with React delivering a user-friendly frontend featuring AI-driven meal recommendations. Data is stored in AWS RDS (PostgreSQL), images and static assets in Amazon S3, with the backend deployed on Amazon EC2 within a secure VPC, and Route 53 managing the domain. Users can customize portions, receive personalized recipe suggestions, and order ingredients for delivery. Key features include a diverse recipe library, accurate calorie calculations, and low operational costs.

### Benefits and Return on Investment
The solution establishes a comprehensive platform for the nutrition startup to expand services while collecting user data for enhanced recommendation systems. It eliminates manual calorie calculations and fragmented shopping through an integrated system, simplifying nutrition planning and driving revenue from ingredient sales. Estimated monthly costs are $21.13 (per AWS Pricing Calculator), totaling approximately $253.56 for 12 months. Development leverages open-source frameworks, incurring no additional hardware costs. ROI is expected within 6–12 months through user time savings and consistent order revenue.

### 3. Solution Architecture
The platform leverages a VPC-based AWS architecture to host a scalable web application. The front-end components run in a public subnet and connect securely to back-end services within private subnets. Traffic from the internet flows through an Internet Gateway (IGW) to the front-end instances, which interact with back-end instances for business logic and data processing. The database resides in a private subnet for enhanced security and isolation. Route 53 manages DNS routing, while an S3 bucket provides application storage accessible through VPC endpoints. IAM controls access permissions, and CI/CD pipelines are orchestrated using AWS CodeBuild and CodePipeline to automate deployments.

![Architecture](/images/2-Proposal/architecture.png)

### AWS Services Used
- **Amazon VPC**: Provides an isolated network with public and private subnets across one Availability Zone.
- **Amazon EC2**: Hosts front-end and back-end application servers.
- **Amazon RDS**: Managed relational database service in a private subnet.
- **Amazon S3**: Used as application storage for assets and data backups.
- **VPC Endpoint**: Enables private connection from EC2 instances to S3 without Internet access.
- **Amazon Route 53**: Manages domain name resolution for external access.
- **AWS CodeBuild & CodePipeline**: Automate build and deployment processes from GitHub to EC2.
- **AWS IAM**: Manages secure user access and permissions.

### Component Design
- **Networking**: The architecture runs within a single VPC containing one public and two private subnets.

- **Front-End Layer**: EC2 instance in the public subnet handles web traffic via the IGW, registered with Route 53.

- **Back-End Layer**: EC2 instance in the private subnet processes requests from the front-end and communicates with the database.

- **Data Layer**: RDS instance resides in a private subnet for persistent data storage.

- **Storage Layer**: Application data and files are stored in Amazon S3, accessible through the VPC Endpoint.

- **CI/CD Pipeline**: CodePipeline and CodeBuild pull code from GitHub, build, and deploy updates to the respective EC2 instances.

- **Security & Access Control**: IAM roles and policies manage user permissions, ensuring only authorized services can access resources.

## 4. Technical Implementation

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

## 5. Timeline & Milestones

### Project Timeline

- **Pre-Project (Month 1):** Team formation, role assignment (backend, frontend, AWS deployment), requirement gathering, and drafting the initial system architecture.
- **Project Execution (Months 1–3):**
  - **Month 1:** Build theory and draw architecture (Spring Boot backend + React frontend design, database schema). Begin initial development of backend and frontend.
  - **Month 2:** Continue backend and frontend development, perform unit and integration testing. Use AWS Pricing Calculator to evaluate hosting costs and refine architecture for cost-effectiveness.
  - **Month 3:** Integrate AWS services (EC2, RDS, S3, VPC, Route 53), configure CI/CD pipelines, conduct staging tests, and deploy the website to production.
- **Post-Launch:** Up to 3 months for maintenance, optimization, and feature enhancements (e.g., scaling backend, improving UI/UX, adding new features).

## 6. Budget Estimation

- AWS Service:
  - VPC: $0.00/month (no cost for subnets, route tables, security groups)
  - EC2: ≈$7.60/month (shared instances, Linux, constant usage, t4g.micro, 360 hours, 20 GB gp3)
  - Route 53: $1.10/month (1 hosted zone, <1M DNS queries, 1 DNS Firewall)
  - S3 Standard: ≈$0.33/month (10 GB, ~5000 requests, outbound data transfer)
  - RDS: ≈$12.00/month (db.t4.micro, 20 GB storage, PostgreSQL, 360 hours, no performance insight)
  - CodeBuild: ≈$0.10/month (3 of build in a month, 10 minutes duration, arm1.small, Linux)
  - CodePipeline: $0.00/month (40 free action, free tier)

Total: ≈$21.13/month (excluding outbound data transfer), ≈253.56/12 months

## 7. Risk Assessment

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
