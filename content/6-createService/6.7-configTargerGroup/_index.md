---
title: " Configure AWS ALB Target Group and Health Check Mechanism"
date: "`r Sys.Date()`"
weight: 7
chapter: false
pre: "<b>6.7</b>"
---

#### Configure AWS ALB Target Group and Health Check Mechanism

1. First, create a target group from the **applicationListener** using the **.addTargets** method with an ID of **`ProductsServiceAlbTarget`**:

```java
applicationListener.addTargets("ProductsServiceAlbTarget",
       AddApplicationTargetsProps.builder()
           .build()
   );

```
![Architect](/images/6/configTarget/01.png?featherlight=false&width=60pc)

2. Set the target group name to **`productsServiceAlb`**:

```java
.targetGroupName("productsServiceAlb")
```
![Architect](/images/6/configTarget/02.png?featherlight=false&width=60pc)

3. Next, specify port 8081 for the Load Balancer to send traffic to, using the HTTP protocol to communicate with the targets:

```java
.port(8081)
.protocol(ApplicationProtocol.HTTP)
```

![Architect](/images/6/configTarget/03.png?featherlight=false&width=60pc)

4. Next, add the list of Fargate services to the target group:

```java
.targets(Collections.singletonList(fargateService))
```

![Architect](/images/6/configTarget/04.png?featherlight=false&width=60pc)

5. Next, define the deregistration delay of 30 seconds, allowing the Load Balancer to wait before deregistering an unavailable target:

```java
.deregistrationDelay(Duration.seconds(30))
```

![Architect](/images/6/configTarget/05.png?featherlight=false&width=60pc)

6. Next, configure the health check and enable health check to ensure that targets are operating correctly:

```java
.healthCheck(HealthCheck.builder()
       .enabled(true)
       .build())

```

![Architect](/images/6/configTarget/06.png?featherlight=false&width=60pc)

7. Kế tiếp chúng ta sẽ cấu hình khoảng thời gian giữa các lần kiểm tra sức khỏe là 30 giây và thời gian tối đa để chờ đợi phản hồi từ một target trong mỗi lần kiểm tra là 10 giây

```java
.interval(Duration.seconds(30))
.timeout(Duration.seconds(10))
```
![Architect](/images/6/configTarget/07.png?featherlight=false&width=60pc)

8. Finally, declare the endpoint used to check the health of targets as **`/actuator/health`** with port 8081 used in the health check to communicate with targets:
   ```java
   .path("/actuator/health")
   .port(8081)
   ```

![Architect](/images/6/configTarget/08.png?featherlight=false&width=60pc)
