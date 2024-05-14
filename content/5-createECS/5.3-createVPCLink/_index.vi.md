---
title : "Tạo VPC Link"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 5.3 </b> "
---

#### Tạo VPC link

1. Để tạo VPC link chúng ta cần khởi tạo đối tượng **vpcLink** đã khai báo trước đó

```java
this.vpcLink = new VpcLink(this, "VpcLink",
                VpcLinkProps.builder()
                        .build());
```
![Architect](/images/5/createNLB/09.png?featherlight=false&width=60pc)

2. Tiếp theo chúng ta sẽ kết nối PVC Link với Network Load Balancer tạo 1 target đến NLB

```java
.targets(Collections.singletonList(this.networkLoadBalancer))
```
![Architect](/images/5/createNLB/10.png?featherlight=false&width=60pc)
