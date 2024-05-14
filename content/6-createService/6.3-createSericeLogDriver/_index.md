---
title: " Create Service Log Driver"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: "<b>6.3</b>"
---

#### Create Service Log Driver

1. During the development of a Spring Boot application, we need logging for debugging the application. Therefore, we will use CloudWatch Log Groups.

2. To create a log group, first initialize an **AwsLogDriver** object within the constructor:

```java
   AwsLogDriver logDriver = new AwsLogDriver(AwsLogDriverProps.builder()
       .build());

```

![Architect](/images/6/logDriver/02.png?featherlight=false&width=60pc)

3. Initialize a **LogGroup** object within the **AwsLogDriver**:

```java
.logGroup(new LogGroup(this, "LogGroup",
       LogGroupProps.builder()
           .build()))

```
![Architect](/images/6/logDriver/03.png?featherlight=false&width=60pc)

4. Next, we need to add the necessary properties for the LogGroup:
   + Set the log group name to **ProductsService**:
     ```java
     .logGroupName("ProductsService")
     ```
   + Configure the log group to be deleted when the Stack is deleted:
     ```java
     .removalPolicy(RemovalPolicy.DESTROY)
     ```
   + Finally, specify how long AWS should retain these logs. Let's retain them for one month:
     ```java
     .retention(RetentionDays.ONE_MONTH)
     ```

![Architect](/images/6/logDriver/04.png?featherlight=false&width=60pc)

5. Finally, we need to set a prefix for our log files:

```java
   .streamPrefix("ProductsService")

```
![Architect](/images/6/logDriver/05.png?featherlight=false&width=60pc)

