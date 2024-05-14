---
title: " Create ECS Task Definition"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b>6.2</b>"
---

# Create ECS Task Definition

1. Initialize a FargateTaskDefinition object within the **ProductsServiceStack** constructor:

```java
   FargateTaskDefinition fargateTaskDefinition = new FargateTaskDefinition(this, "TaskDefinition",
       FargateTaskDefinitionProps.builder()
           .build());
```
![Architect](/images/6/taskdefinition/01.png?featherlight=false&width=60pc)

2. Set the family name for the task definition group:

```java
   .family("products-service")
```
![Architect](/images/6/taskdefinition/02.png?featherlight=false&width=60pc)

3. Next, define the CPU and memory limits for the task definition:

```java
   .cpu(512)
   .memoryLimitMiB(1024)

```
![Architect](/images/6/taskdefinition/03.png?featherlight=false&width=60pc)