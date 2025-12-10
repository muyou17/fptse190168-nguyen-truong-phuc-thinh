---
title: "Blog 2"
date: "2025-10-09"
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---


# Reduce your Amazon ElastiCache costs by up to 60% with Valkey and CUDOS
*by Chris Gillespie, Brenno Passanha, and Yuriy Prykhodko | on 04 SEP 2025 | in Amazon ElastiCache, Cloud Cost Optimization, Intermediate (200), Technical How-to | [Permalink](https://aws.amazon.com/blogs/database/reduce-your-amazon-elasticache-costs-by-up-to-60-with-valkey-and-cudos/)*

In this post, we show you how to save costs on Amazon ElastiCache by upgrading your cluster engine to ElastiCache for Valkey. If you’re currently using ElastiCache for Redis OSS, you can achieve up to 60% cost savings by upgrading to Valkey.

We also show you the CUDOS dashboard, part of the open source Cloud Intelligence Dashboards (CID) framework, to see the ElastiCache Redis-to-Valkey cost savings available to your organization, track progress achieving these, and prioritize upgrade efforts.

## Amazon ElastiCache for Valkey

Valkey is an open source, high performance, key-value datastore stewarded by Linux Foundation and backed by over 50 companies including Ericsson, Google Cloud, Aiven, Oracle, Percona, ByteDance and Amazon Web Services (AWS). Valkey is a drop-in replacement for Redis OSS and has seen rapid adoption since its project inception. Valkey is available as a managed service through ElastiCache.

With ElastiCache for Valkey, you can choose between self-designed (node-based) clusters and serverless caching deployment options. ElastiCache for Valkey Serverless is priced 33% lower than other supported engines, and self-designed (node-based) clusters are priced 20% lower. You can move to these lower prices through in-place, zero downtime upgrades of your existing ElastiCache clusters from Redis OSS to Valkey. Your existing ElastiCache for Redis OSS Reservations will continue to apply.

## Upgrading to ElastiCache for Valkey

When ElastiCache applies the upgrade from Redis OSS to Valkey, it follows the same process as a major version upgrade to the existing Redis OSS engine. If your cluster is self-designed, you should follow the upgrade best practices.

When starting the upgrade, you choose the major version of Valkey. Valkey is available from version 7.2, which includes the changes from Redis OSS up to version 7.2.4. If your cluster is running a version of Redis OSS earlier than 7.2.4, you should take changes that could impact your workload into account.

Upgrading to version 8.1 will bring additional benefits such as faster scaling, Bloom filters, conditional updates (and more). Version 8.0 included an improvement in memory utilization over Valkey 7.2 (and thus all Redis OSS versions), with a further improvement released in version 8.1. The following chart shows the memory improvements for a sample workload. A deeper exploration can be found in the post Year One of Valkey: Open-Source Innovations and ElastiCache version 8.1.

A 40% improvement in memory usage might allow you to downsize your node type in a self-designed cluster. This would deliver a further cost saving of 50% for the same workload. Your overall savings in this case would be 60% (`1 – (0.8 * 0.5)`).

You can start the ElastiCache Redis OSS to Valkey upgrade through the AWS Management Console, AWS Command Line Interface (AWS CLI), AWS API, or AWS CloudFormation.
* The console – For self-designed or serverless clusters, follow the instructions in Upgrade from ElastiCache for Redis OSS to ElastiCache for Valkey in Get started with Amazon ElastiCache for Valkey.
* AWS CLI – Use the modify-cache-cluster or modify-serverless-cache commands. Set the engine to valkey and specify a major version. For example, to upgrade a self-designed cluster to Valkey version 8.0, use the following:
```bash
aws elasticache modify-cache-cluster \
 --cache-cluster-id my-cluster \
 --engine valkey \
 —engine-version 8.0
```
For a serverless cluster, use:
```bash
aws elasticache modify-serverless-cache-instance \
 --cache-cluster-id my-serverless-cluster \
 --engine valkey \
 --engine-version 8.0
```
* API – Use the ModifyCacheCluster or ModifyServerlessCacheInstance API, setting the Engine parameter to valkey and EngineVersion to the desired major version.
* CloudFormation – Update the CacheClusterEngine and CacheClusterEngineVersion properties in your AWS::ElastiCache::CacheCluster or AWS::ElastiCache::GlobalReplicationGroup resources.

The status of your cluster will show as modifying during the upgrade and change back to available once complete. The upgrade process will automatically handle the migration of your data. Your cluster will remain available throughout the upgrade.

Planning and prioritizing migration efforts, understanding per-resource potential savings, and tracking efficiency gains at scale require comprehensive visibility. To streamline these tasks, AWS offers the CUDOS dashboard, which provides detailed insights into cost-saving opportunities across your ElastiCache resources.

## The Cloud Intelligent Dashboards Framework

CUDOS dashboard is part of the open source Cloud Intelligence Dashboards (CID) framework, which you can deploy in your AWS account using the provided infrastructure as code (IaC) templates. The framework helps you drive financial accountability, optimize costs, and increase operational efficiency across your AWS organizations. The CUDOS dashboard provides detailed and actionable insights, enabling data-driven decisions for cost efficiency across your AWS infrastructure.

## ElastiCache upgrade insights in CUDOS

CUDOS version 5.6 introduces a comprehensive ElastiCache section that transforms your Redis-to-Valkey migration into a data-driven optimization strategy. The dashboard provides:
* Visual adoption tracking – Monitor your adoption progress with clear breakdowns showing the mix of Redis OSS, Valkey self-designed, and Valkey serverless clusters over time.
* Quantified savings – See your historical cost efficiency gains achieved from clusters already upgraded to Valkey.
* Granular recommendations – Per-cluster analysis showing current Redis OSS costs, estimated Valkey costs, and exact savings potential for each AWS account and ElastiCache cluster.

The dashboard’s detailed cost breakdown table shows exactly which clusters offer the highest savings potential—with current costs on Redis OSS, estimated costs if upgraded to Valkey, and projected monthly savings. You can filter these savings opportunities based on your company taxonomy, such as cost allocation tags, organizational units, cost categories, or other dimensions that align with your organization’s decision-making process.

## Getting started with CUDOS

To get started with CUDOS, you can explore the ElastiCache section in an interactive demo dashboard. Follow the deployment guide to set up CUDOS in your organization.

If you’re already using CUDOS, follow the update guidance to upgrade to version 5.6. You can also use the add organizational taxonomy guide to add the ability to filter visuals by tags, cost categories, and other dimensions of your organizational taxonomy.

You can explore the Cloud Intelligence Dashboards page to learn about the different dashboards for your operational insights.

Conclusion
ElastiCache for Valkey is a drop-in replacement for ElastiCache for Redis OSS. By upgrading your existing ElastiCache for Redis OSS clusters to ElastiCache for Valkey you will:
* See a 33% cost reduction for serverless clusters, or 20% cost reduction for self-designed (node-based) clusters.
* Be able to continue using your existing ElastiCache for Redis OSS Reservations with your clusters.
* Use an in-place, zero downtime upgrade process with minimal application disruption to upgrade existing clusters.
* See an improvement in ElastiCache memory utilization, with the potential of downsizing the nodes in a self-designed cluster, delivering an additional 50% cost saving.

To support calculating and tracking these efficiency gains across your organization and prioritizing migration efforts, you can use CUDOS dashboard, which provides a comprehensive ElastiCache Redis-to-Valkey migration section.

At AWS, we believe Valkey represents the future of in-memory data stores. For a step-by-step guide on how to get started with ElastiCache for Valkey, see Get started with Amazon ElastiCache for Valkey, or visit the Amazon ElastiCache documentation.

## About the authors

### Chris Gillespie

Chris is a UK-based Senior Solutions Architect. He spends most of his work with fast-moving “born in the cloud” customers. Outside of work, he fills his time with family and trying to get fit.

### Brenno Passanha

Brenno is a Senior Technical Account Manager. He is part of the Cloud Operations Technical Field Community, focusing on Cloud Financial Management. Outside of work, Brenno enjoys raising his children, traveling the world, and creating memories through new experiences.

### Yuriy Prykhodko

Yuriy is a Principal Technical Account Manager based in Luxembourg. He helps customers build highly reliable and cost effective systems on AWS. He is also a FinOps SME and Cloud Intelligence Dashboards author and lead. In his free time he enjoys playing basketball and traveling all around the world with his family and friends.
