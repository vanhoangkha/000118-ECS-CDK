---
title: "Deploy Spring Boot applications onto AWS ECS Fargate using AWS CDK."
date: "`r Sys.Date()`"
weight: 1
chapter: false
---

# Deploying Spring Boot Services on AWS ECS Fargate using AWS CDK

#### Overview

In this workshop, we will create a microservice using Java 11, leveraging the Spring Boot framework, and Docker containers to build a backend application that interacts with Amazon Web Services (AWS) resources. These resources will be provisioned on AWS using the AWS Cloud Development Kit (CDK) v2, a modern way to model and configure infrastructure on AWS. AWS CDK is one of the best tools for managing infrastructure as code (IaC) on AWS.

![Architect](/images/ws2.svg?featherlight=false&width=80pc)

#### Content
1. [Introduction](1-introduce/)
2. [Prerequisite steps](2-Prerequiste/)
3. [Create ECR Image Repository](3-createECR/)
4. [Create VPC and NAT Gateway](4-createVPC/)
5. [Create ECS](5-createECS/)
6. [Create ECS Service](6-createService/)
7. [Create API Gateway](7-createAPIGateway/)
8. [Create DynamoDB Table](8-createDynamoDB/)
9. [Instrumenting AWS ECS Service](9-intrumentingECS/)
10. [Clean up resources](10-cleanResource/)

