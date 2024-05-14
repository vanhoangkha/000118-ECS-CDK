---
title : "Tạo ECS task definition"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 6.2 </b> "
---

# Tạo CES task definition 

1. Trong **ProductsServiceStack** contructor khởi tạo đối tượng **FargateTaskDefinition**

```java
   FargateTaskDefinition fargateTaskDefinition = new FargateTaskDefinition(this, "TaskDefinition",
                FargateTaskDefinitionProps.builder()
                        .build());
```
![Architect](/images/6/taskdefinition/01.png?featherlight=false&width=60pc)

2. Tạo tên group của tất cả các task definition bạn đã tạo

```java
   .family("products-service")
```
![Architect](/images/6/taskdefinition/02.png?featherlight=false&width=60pc)

3. Tiếp theo chúng ta định nghĩa số lượng **cpu** là 512 và **memory** là 1024

```java
   .cpu(512)
   .memoryLimitMiB(1024)
```
![Architect](/images/6/taskdefinition/03.png?featherlight=false&width=60pc)

