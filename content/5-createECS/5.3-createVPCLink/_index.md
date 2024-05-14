---
title: " Create VPC Link"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: "<b>5.3</b>"
---

#### Create VPC Link

1. To create a VPC Link, we need to initialize the `vpcLink` object that was previously declared:
```java
   this.vpcLink = new VpcLink(this, "VpcLink",
       VpcLinkProps.builder()
           .build());

```
![Architect](/images/5/createNLB/09.png?featherlight=false&width=60pc)

2. Next, we will connect the VPC Link to the Network Load Balancer (NLB) by setting the NLB as the target for the VPC Link:
```java
   .targets(Collections.singletonList(this.networkLoadBalancer))
```
![Architect](/images/5/createNLB/10.png?featherlight=false&width=60pc)

   