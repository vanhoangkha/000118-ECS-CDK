---
title : "Tạo Application Load Balancer"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 5.4 </b> "
---


#### Tạo Application Load Balancer

1. Trước tiên chúng ta sẽ tạo Application Load Balancer (NLB). Để tạo SLB chúng ta cần khởi tạo đối tượng **applicationLoadBalancer** trong contructor

```java
this.applicationLoadBalancer = new ApplicationLoadBalancer(this, "Alb",
                ApplicationLoadBalancerProps.builder()
                        .build());
```

![Architect](/images/5/createNLB/11.png?featherlight=false&width=60pc)

2. Tiếp theo đặt tên cho NLB là **ECommerceNlb** bằng cách

```java
.loadBalancerName("ECommerceAlb")
```

![Architect](/images/5/createNLB/12.png?featherlight=false&width=60pc)

3. Như đã để cập trước đó. Chúng ta sẽ khổng để NLB và ALB ngoài VPC nên chúng ta sẽ làm như sau

```java
.internetFacing(false)
.vpc(nlbStackProps.vpc())
```
![Architect](/images/5/createNLB/13.png?featherlight=false&width=60pc)
