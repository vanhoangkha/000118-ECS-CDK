---
title : "Tạo Stack mới cho product service"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 6.1 </b> "
---

#### Tạo Stack mới cho product service

1. Mở CDK project tạo stack mới với tên là **ProductsServiceStack** và cho **ProductsServiceStack** kế thừa lớp Stack của thư viện amazon.awscdk

![Architect](/images/6/newstack/01.png?featherlight=false&width=60pc)

2. Để tạo ProductsServiceStack chúng ta cần VPC, Cluster, NetworkLoadBalancer, ApplicationLoadBalancer và Repository mà chúng ta đã tạo trước đó. Nên chúng ta cần tạo 1 record class tên là **ProductsServiceProps** để truyền vào ProductsServiceStack. Thêm đoạn code bên ngoài class ClusterStack

```java
record ProductsServiceProps(
    Vpc vpc,
    Cluster cluster,
    NetworkLoadBalancer networkLoadBalancer,
    ApplicationLoadBalancer applicationLoadBalancer,
    Repository repository
){}
```

![Architect](/images/6/newstack/02.png?featherlight=false&width=60pc)

3. Tạo Contructor

```java
public ProductsServiceStack(final Construct scope, final String id,
                    final StackProps props, ProductsServiceProps productsServiceProps) {
        super(scope, id, props);
        }
```

![Architect](/images/6/newstack/03.png?featherlight=false&width=60pc)

