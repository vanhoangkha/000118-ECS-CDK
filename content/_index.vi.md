---
title : "Deploy Spring Boot applications onto AWS ECS Fargate using AWS CDK."
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
---

# Sử dụng AWS CDK để triển khai dịch vụ Spring Boot trên AWS ECS Fargate

#### Tổng quan

Trong wokshop này, chúng ta sẽ tạo ra một microservices bằng Java 21, sử dụng framework Spring Boot V3 và các container Docker, xây dựng một ứng dụng backend để tương tác với các tài nguyên của Amazon Web Services (AWS). Các tài nguyên này sẽ được tạo trong AWS bằng cách sử dụng AWS Cloud Development Kit (CDK) V2, một cách hiện đại để mô hình hóa và cấu hình cơ sở hạ tầng trong AWS. AWS CDK là một trong những công cụ tốt nhất cho việc quản lý cơ sở hạ tầng dưới dạng mã, hay còn gọi là IaC (Infrastructure as Code), trên AWS.

![Architect](/images/ws2.svg?featherlight=false&width=80pc)

#### Nội dung
1. [Giới thiệu](1-introduce/)
2. [Các bước chuẩn bị](2-Prerequiste/)
3. [Tạo kho lưu trữ hình ảnh ECR](3-createECR/)
4. [Tạo VPC và NAT Gateway](4-createVPC/)
5. [Tạo ECS](5-createECS/)
6. [Tạo dịch vụ ECS](6-createService/)
7. [Tạo API Gateway](7-createAPIGateway/)
8. [Tạo bảng DynamoDB](8-createDynamoDB/)
9. [Cài đặt dịch vụ ECS AWS](9-intrumentingECS/)
10. [Dọn dẹp tài nguyên](10-cleanResource/)
