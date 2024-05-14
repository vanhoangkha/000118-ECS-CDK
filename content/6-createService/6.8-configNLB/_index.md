---
title: " Configure AWS Network Load Balancer"
date: "`r Sys.Date()`"
weight: 8
chapter: false
pre: "<b>6.8</b>"
---

#### Create a Network Listener

1. First, initialize a `NetworkListener` object:

```java
   NetworkListener networkListener = productsServiceProps.networkLoadBalancer()
       .addListener("ProductsServiceNlbListener", BaseNetworkListenerProps.builder()
           .build());

```
![Architect](/images/6/configNLB/01.png?featherlight=false&width=60pc)

2. Now, configure the `networkListener` to listen on port 8081:

```java
.port(8081)
```

![Architect](/images/6/configNLB/02.png?featherlight=false&width=60pc)

3. Specify the protocol that the listener uses to listen for connections. In this case, we use TCP.

```java
 .protocol(software.amazon.awscdk.services.elasticloadbalancingv2.Protocol.TCP)
```

{{% notice info %}}
software.amazon.awscdk.services.elasticloadbalancingv2.Protocol.TCP: This is the way to specify the TCP protocol using CDK. `Protocol.TCP` is a constant in CDK representing the TCP protocol.
{{% /notice %}}

![Architect](/images/6/configNLB/03.png?featherlight=false&width=60pc)


#### Configure NLB Target Group

4. Use the `addTargets` method called on the `networkListener` object to add new targets to the listener previously configured on the Network Load Balancer (NLB).

```java
   networkListener.addTargets("ProductsServiceNlbTarget",
       AddNetworkTargetsProps.builder()
           .build()
   );
```
![Architect](/images/6/configNLB/04.png?featherlight=false&width=60pc)

5. Next, specify that connections will be sent to port 8081 on the targets.

```java
.port(8081)
```
![Architect](/images/6/configNLB/05.png?featherlight=false&width=60pc)

6. Connect to the targets using the TCP protocol.

```java
.protocol(software.amazon.awscdk.services.elasticloadbalancingv2.Protocol.TCP)
```

![Architect](/images/6/configNLB/06.png?featherlight=false&width=60pc)

7. Next, specify the name of the target group to which the targets will be added as **productsServiceNlb**.

```java
.targetGroupName("productsServiceNlb")
```
![Architect](/images/6/configNLB/07.png?featherlight=false&width=60pc)

8. Next, we will specify the list of specific targets that need to be added using the `targets` method.

```java
.targets(Collections.singletonList(/* List of targets */))
```
![Architect](/images/6/configNLB/08.png?featherlight=false&width=60pc)

9. Tạo ra một target sử dụng cho dịch vụ Fargate. Một target Fargate sẽ được tạo để xử lý các kết nối gửi đến từ NLB.

```java
.targets(Collections.singletonList(
    fargateService.loadBalancerTarget(LoadBalancerTargetOptions.builder()
        .build())
))
```
![Architect](/images/6/configNLB/09.png?featherlight=false&width=60pc)

10. Next, we will define the name of the container in the Fargate service to which connections will be sent as **productsService**.

```java
   .targets(Collections.singletonList(
       fargateService.loadBalancerTarget(LoadBalancerTargetOptions.builder()
           .containerName("productsService")
           .build())
   ))

```
![Architect](/images/6/configNLB/10.png?featherlight=false&width=60pc)

11. Finally, configure port 8081 of the container in the Fargate service to which connections will be sent, using the TCP protocol.

```java
   .targets(Collections.singletonList(
       fargateService.loadBalancerTarget(LoadBalancerTargetOptions.builder()
           .containerName("productsService")
           .containerPort(8081)
           .protocol(Protocol.TCP)
           .build())
   ))
```
![Architect](/images/6/configNLB/11.png?featherlight=false&width=60pc)


