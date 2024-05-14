---
title : "Thêm Listener vào Application Load Balancer"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 6.5 </b> "
---

#### Thêm Listener vào Application Load Balancer

1. Khởi tạo **ApplicationListener** 

```java
ApplicationListener applicationListener = productsServiceProps.applicationLoadBalancer()
```

![Architect](/images/6/addListenALB/05.png?featherlight=false&width=60pc)

2. Thêm 1 listener với id là **ProductsServiceAlbListener** và khởi tạo **ApplicationListenerProps**

```java
.addListener("ProductsServiceAlbListener", ApplicationListenerProps.builder()
                        .build());
```
![Architect](/images/6/addListenALB/06.png?featherlight=false&width=60pc)

3. Tiếp theo cho ta sẽ cho ALB listener trên port 8081 và sử dụng giao thức HTTP

```java
   .port(8081)
   .protocol(ApplicationProtocol.HTTP)
```

![Architect](/images/6/addListenALB/07.png?featherlight=false&width=60pc)

4. Cuối cùng thêm **applicationListener** vào Application Load Balancer

```java
   .loadBalancer(productsServiceProps.applicationLoadBalancer())
```

![Architect](/images/6/addListenALB/08.png?featherlight=false&width=60pc)

