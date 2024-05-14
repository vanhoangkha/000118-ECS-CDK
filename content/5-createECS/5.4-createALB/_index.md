---
title: " Create Application Load Balancer"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: "<b>5.4</b>"
---

#### Create Application Load Balancer

1. First, let's create an Application Load Balancer (ALB). To create the ALB, we need to initialize the `applicationLoadBalancer` object within the constructor:

```java
   this.applicationLoadBalancer = new ApplicationLoadBalancer(this, "Alb",
       ApplicationLoadBalancerProps.builder()
           .build());
```

![Architect](/images/5/createNLB/11.png?featherlight=false&width=60pc)

2. Next, set the name for the Application Load Balancer (ALB) to **ECommerceAlb** using:

```java
.loadBalancerName("ECommerceAlb")
```

![Architect](/images/5/createNLB/12.png?featherlight=false&width=60pc)

3. As mentioned earlier, we will not expose the Application Load Balancer (ALB) and Network Load Balancer (NLB) to the internet outside of the VPC. Therefore, we will configure them as follows:

```java
   .internetFacing(false)
   .vpc(nlbStackProps.vpc())

```
![Architect](/images/5/createNLB/13.png?featherlight=false&width=60pc)
