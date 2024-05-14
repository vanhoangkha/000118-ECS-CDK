---
title: "Introduction"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b>1.</b>"
---

#### This workshop will cover the following AWS resources and tools:

+ **AWS ECS**: Elastic Container Service is AWS's container orchestration service. With ECS, you can manage the execution of Docker-based microservices efficiently and at scale. With AWS Fargate, a serverless compute engine for containers from Amazon Web Services, you don't need to provision EC2 instances, reducing the operational cost of container-based applications.

+ **AWS ECR**: With Elastic Container Registry (ECR) from AWS, you can create private repositories to store Docker images of your microservices.

+ **AWS VPC**: With Virtual Private Cloud (VPC) from AWS, you can secure your infrastructure with private subnets and network security policies for inbound and outbound traffic rules.

+ **AWS ALB**: AWS Application Load Balancer allows you to balance HTTP traffic across all available application instances, and with integrated target groups, each instance can be monitored to only receive traffic if it is operating normally.

+ **API Gateway REST**: With AWS API Gateway, you can protect your application's REST APIs and perform validation checks on query string parameters and request contents.

+ **CloudWatch Logs**: Its responsibility is to aggregate application logs and their metrics. The applications built in this workshop will log to CloudWatch Logs in JSON format using the log4j2 library. This enables us to insert parameters into the logs for use in queries in the AWS CloudWatch Logs Insights dashboard.

+ **CloudWatch Alarms**: With alarms from CloudWatch, you can monitor unusual events from applications and AWS resources.

+ **DynamoDB**: DynamoDB is a powerful and non-relational NoSQL database service. This workshop introduces the usage of DynamoDB enhanced client from AWS SDK v2 for Java, which is a high-level library allowing client-side class mappings with DynamoDB tables.

+ **SQS**: SQS is a queue service that allows asynchronous communication between applications, facilitating message and event exchanges.
