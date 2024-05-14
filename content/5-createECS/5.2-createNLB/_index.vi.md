---
title : "Tạo Network Load Balancer"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---

#### Tạo Network Load Balancer stack

1. Trong CDK project tạo stack mới với tên là **NlbStack.java** và sau đó cho class NlbStack kế thừa lớp Stack cửa thư viện amazon.awscdk

![Architect](/images/5/createNLB/01.png?featherlight=false&width=60pc)

2. Để tạo NLB stack chúng ta cần VPC mà chúng ta đã tạo trước đó. Nên chúng ta cần tạo 1 record class tên là **NlbStackProps** để truyền VPC vào NLB Stack. Thêm đoạn code bên ngoài class ClusterStack

```java
record NlbStackProps(
        Vpc vpc
){}
```

![Architect](/images/5/createNLB/02.png?featherlight=false&width=60pc)

3. Trong NLB Stack chúng ta sẽ tạo **Vpc Link**, **Network Load Balancer**, **Application Load Balancer** nên trước tiên chúng ta cần tạo 3 thuộc tính ứng với chúng.

```java
private final VpcLink vpcLink;
private final NetworkLoadBalancer networkLoadBalancer;
private final ApplicationLoadBalancer applicationLoadBalancer;
```
![Architect](/images/5/createNLB/03.png?featherlight=false&width=60pc)

4. Tiếp theo, chúng ta tạo các getter cho 3 thuộc tính trên

```java
public VpcLink getVpcLink() {
        return vpcLink;
}

public NetworkLoadBalancer getNetworkLoadBalancer() {
        return networkLoadBalancer;
}

public ApplicationLoadBalancer getApplicationLoadBalancer() {
        return applicationLoadBalancer;
}
```

![Architect](/images/5/createNLB/04.png?featherlight=false&width=60pc)

5. Tạo contructor cho class NlbStack 

```java
public NlbStack(final Construct scope, final String id,
                        final StackProps props, NlbStackProps nlbStackProps) {
        super(scope, id, props);
    }
```

![Architect](/images/5/createNLB/05.png?featherlight=false&width=60pc)

#### Tạo network load balancer

6. Trước tiên chúng ta sẽ tạo network load balancer (NLB). Để tạo NLB chúng ta cần khởi tạo đối tượng **networkLoadBalancer** trong contructor

```java
this.networkLoadBalancer = new NetworkLoadBalancer(this, "Nlb",
                NetworkLoadBalancerProps.builder()
                        .build());
```
![Architect](/images/5/createNLB/06.png?featherlight=false&width=60pc)

7. Tiếp theo đặt tên cho NLB là **ECommerceNlb** bằng cách

```java
.loadBalancerName("ECommerceAlb")
```

![Architect](/images/5/createNLB/07.png?featherlight=false&width=60pc)

8. Như trong thiết kế trên diagram. Chúng ta sẽ khổng để NLB và ALB ngoài VPC nên chúng ta sẽ làm như sau

```java
.internetFacing(false)
.vpc(nlbStackProps.vpc())
```
![Architect](/images/5/createNLB/08.png?featherlight=false&width=60pc)

