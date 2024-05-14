---
title: " Add Listener to Application Load Balancer"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: "<b>6.5</b>"
---

#### Add Listener to Application Load Balancer

1. Initialize an **ApplicationListener**:

```java
ApplicationListener applicationListener = productsServiceProps.applicationLoadBalancer();

```

![Architect](/images/6/addListenALB/05.png?featherlight=false&width=60pc)

2. Add a listener with the ID **ProductsServiceAlbListener** and initialize **ApplicationListenerProps**:

```java
   .addListener("ProductsServiceAlbListener", ApplicationListenerProps.builder()
       .build());

```
![Architect](/images/6/addListenALB/06.png?featherlight=false&width=60pc)

3. Next, configure the ALB listener on port 8081 using the HTTP protocol:

```java
   .port(8081)
   .protocol(ApplicationProtocol.HTTP)
```

![Architect](/images/6/addListenALB/07.png?featherlight=false&width=60pc)

4. Finally, add the **applicationListener** to the Application Load Balancer:

```java
   .loadBalancer(productsServiceProps.applicationLoadBalancer())
```

![Architect](/images/6/addListenALB/08.png?featherlight=false&width=60pc)

