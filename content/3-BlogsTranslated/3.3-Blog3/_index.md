---
title: "Blog 3"
date: "2025-10-10"
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---


# Getting started with Amazon EC2 bare metal instances for Amazon RDS for Oracle and Amazon RDS Custom for Oracle
*by Sameer Malik and Nitin Saxena | on 04 SEP 2025 | in Amazon EC2 Bare Metal, Amazon RDS, Amazon RDS Custom, Intermediate (200), RDS for Oracle, Technical How-to | [Permalink](https://aws.amazon.com/blogs/database/getting-started-with-amazon-ec2-bare-metal-instances-for-amazon-rds-for-oracle-and-amazon-rds-custom-for-oracle/)*

Amazon Relational Database Service (Amazon RDS) for Oracle is a fully managed commercial database that makes it straightforward to set up, operate, and scale Oracle deployments in the cloud. Amazon RDS Custom for Oracle makes it possible for you as a database administrator to access and customize your Oracle database environment and operating system.

In this post, we explore the support for AWS bare metal instances on Amazon EC2 bare metal Instances for Amazon RDS for Oracle and RDS Custom for Oracle.

Amazon EC2 bare metal instances are designed to provide your applications with direct access to the processor and memory of the underlying server. Amazon EC2 bare metal instances provide capacity fully dedicated for use in a non-shared tenancy model and offer a higher level of isolation compared to shared virtualized instances.

These bare metal instances might also offer additional licensing benefits to use your eligible software licenses for Bring Your Own License (BYOL) licensing models from vendors such as Microsoft and Oracle. For additional details on the licensing benefits of using Amazon EC2 bare metal instances, refer to the post Oracle Licensing Efficiency With Dedicated Hosts from AWS licensing partner House of Brick.

Additionally, Amazon EC2 bare metal instances on Amazon RDS for Oracle and RDS Custom for Oracle are offered at 25% lower compute cost than virtualized instances of the same size, making Oracle workloads on RDS bare metal instances more cost-effective.

## Common use cases for EC2 bare metal instances

The following are common use cases for EC2 bare metal instances:

* Support for workloads with restrictive licensing – This is often the primary driver, especially for traditional enterprise software like Oracle Database, SQL Server, SAP, or Windows Server, where licensing might be more favorable on physical cores or dedicated hardware.
* High-performance computing (HPC) and scientific simulations – These workloads require the absolute highest performance, direct access to specialized hardware features, or very low-latency inter-node communication.
* Legacy applications – These applications are not supported in virtualized environments or require specific hardware access.
* Isolation and security – Bare metal instances offer a higher level of isolation compared to shared virtualized instances, which can be beneficial for highly sensitive workloads or compliance requirements. It also serves as an option when your organization is looking to run certain applications on a single-tenant infrastructure in non-virtualized environments to meet any specific corporate compliance and regulatory requirements.

## Benefits of using RDS bare metal instances on Amazon RDS for Oracle and RDS Custom for Oracle

RDS bare metal instances offer the following benefits:
* Licensing benefits – Because RDS bare metal instances provide capacity fully dedicated for your use in a non-shared tenancy model, the licensing can become more akin to on-premises deployments where customers (depending on their contract with Oracle) can apply traditional core-based licensing and Oracle processor core factor.
* Lower compute cost – RDS bare metal instances for Amazon RDS for Oracle and RDS Custom for Oracle offer 25% lower compute costs compared to equivalent virtualized instances. For example, an m6i.metal bare metal instance costs 25% less than an m6i.32xlarge virtualized instance.
* Effective database consolidation – With the licensing benefits and lower compute costs for bare metal instances on Amazon RDS for Oracle and RDS Custom for Oracle, you can effectively consolidate your database workloads using currently supported database consolidation methods such as schema consolidation and Oracle multi-tenant options.
* Enhanced performance for specific workloads – Customers running their Oracle Database workload on the virtualized environment and utilizing more than half the cores of the entire physical server can now enjoy the performance of the full physical server (such as CPU, memory, IOPS, and throughput) at no additional licensing cost.

## Create an RDS for Oracle bare metal instance using the Amazon RDS console

In this section, we demonstrate how to create an RDS for Oracle DB instance with the bare metal instance type using the AWS Management Console. For details and prerequisites, refer to Create an Oracle DB instance.
1. Sign in to the Amazon RDS console.
2. In the upper-right corner of the Amazon RDS console, choose the AWS Region in which you want to create the DB instance.
3. In the navigation pane, choose Databases.
4. Choose Create database.
5. Select Standard create.
6. For Engine type, select Oracle.
7. For Database management type, select Amazon RDS.
8. For Edition, select Oracle Enterprise Edition.
9. For License model, leave the default Bring Your Own License (BYOL).
10. For Engine version, choose your preferred version.
11. In the Templates section, select Production.
12. For DB instance identifier, enter a name for your DB instance.
13. In the Credentials Settings section, provide a username for the admin user, select Self-managed for Credentials management, and enter your password.
14. For Encryption key, use your choice of encryption key.
15. In the Instance configuration section, for the DB instance class, choose a bare metal instance (for example, db.m6i.metal).
16. Complete the rest of the instance creation steps and choose Create database.

After you create your DB instance, you can verify its status on the Amazon RDS console.

## Create an RDS for Oracle bare metal instance using the AWS CLI

You can also create a bare metal instance using the AWS Command Line Interface (AWS CLI), as shown in the following code. Make sure the AWS CLI is configured with the necessary credentials and default Region.

```bash
aws rds create-db-instance \
        --db-instance-identifier metaldb \
        --db-instance-class db.m6i.metal \
        --engine oracle-ee \
        --allocated-storage 100 \
        --master-username <username> \
        --master-user-password <YourStrongPassword> \
        --engine-version 19.0.0.0.ru-2025-04.spb-1.r1\
        --license-model license-included \
        --publicly-accessible false \
       --storage-encrypted \
        --vpc-security-group-ids sg-xxxxxxxxxxxxxxxxx \
        --db-subnet-group-name my-db-subnet-group
```

## Clean up

To avoid paying the cost for running the RDS instance, delete the above provisioned RDS instance. You can delete a DB instance using the AWS Management Console, the AWS CLI, or the RDS API. For more details on the how to delete the RDS instance, refer to Deleting a DB instance.

## Conclusion

With Amazon EC2 bare metal instances for Amazon RDS for Oracle and RDS Custom for Oracle, you can benefit from additional licensing flexibility and lower cost options to effectively run your Oracle Database workloads on Amazon RDS for Oracle and RDS Custom for Oracle. For more details about this launch, refer to What’s new.

## About the authors

### Sameer Malik

Sameer has been working on relational Databases for over 23 years now and is currently working as a Principal Database Solution Architect at AWS focused on RDS and Aurora. He has helped several customers with the migration and modernization of their Database workloads to AWS.

### Nitin Saxena

Nitin is a Software Development Manager in RDS DBS Managed Commercial Engines with Amazon Web Services. He focuses on services like RDS Oracle and RDS Custom for Oracle. He enjoys designing and developing new features on RDS Oracle and RDS Custom to solve customer problems.
