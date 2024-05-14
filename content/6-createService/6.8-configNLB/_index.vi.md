---
title : "Cấu hình AWS Network Load Balancer"
date :  "`r Sys.Date()`" 
weight : 8
chapter : false
pre : " <b> 6.8 </b> "
---

#### Tạo NetworkListener

1. Trước tiên chùng ta cần khởi tạo đối tượng NetworkListener 

```java
NetworkListener networkListener = productsServiceProps.networkLoadBalancer()
                .addListener("ProductsServiceNlbListener", BaseNetworkListenerProps.builder()
                        .build());
```
![Architect](/images/6/configNLB/01.png?featherlight=false&width=60pc)

2. Bây giờ, chúng ta sẽ cấu hình cho networkListener lắng nghe trên port 8081

```java
.port(8081)
```

![Architect](/images/6/configNLB/02.png?featherlight=false&width=60pc)

3. Sử dụng protocol là giao thức mà listener sử dụng để lắng nghe các kết nối. Ở đây chúng ta dùng TCP

```java
 .protocol(software.amazon.awscdk.services.elasticloadbalancingv2.Protocol.TCP)
```

{{% notice info %}}
software.amazon.awscdk.services.elasticloadbalancingv2.Protocol.TCP: Đây là cách để chỉ định giao thức TCP thông qua CDK. Protocol.TCP là một hằng số trong CDK để đại diện cho giao thức TCP.
{{% /notice %}}

![Architect](/images/6/configNLB/03.png?featherlight=false&width=60pc)

#### Cấu hình NLB target group

4. Sử dụng phương thức **addTargets** được gọi trên đối tượng networkListener để thêm các target (đích) mới cho listener đã được cấu hình trước đó trên NLB.

```java
networkListener.addTargets("ProductsServiceNlbTarget",
                AddNetworkTargetsProps.builder()
                        .build()
                );
```
![Architect](/images/6/configNLB/04.png?featherlight=false&width=60pc)

5. Tiếp theo cho ta sẽ cho các kết nối sẽ được gửi đến cổng 8081 trên các target.

```java
   .port(8081)
```
![Architect](/images/6/configNLB/05.png?featherlight=false&width=60pc)

6. Để kết nối đến các target ta sử dụng giao thức TPC

```java
.protocol(software.amazon.awscdk.services.elasticloadbalancingv2.Protocol.TCP)
```

![Architect](/images/6/configNLB/06.png?featherlight=false&width=60pc)

7. Tiếp theo tạo tên của nhóm target (target group) mà các target sẽ được thêm vào là **``productsServiceNlb``**

```java
.targetGroupName("productsServiceNlb")
```
![Architect](/images/6/configNLB/07.png?featherlight=false&width=60pc)

8. Tiếp theo chúng ta sẽ chỉ định danh sách các target cụ thể cần được thêm vào bằng phương thức targets

```java
    .targets(Collections.singletonList())
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

10. Tiếp theo chúng ta sẽ định nghĩa tên của container trong dịch vụ Fargate mà các kết nối sẽ được gửi đến là **productsService**

```java
.targets(Collections.singletonList(
            fargateService.loadBalancerTarget(LoadBalancerTargetOptions.builder()
                    .containerName("productsService")
                    .build())
                    ))
```
![Architect](/images/6/configNLB/10.png?featherlight=false&width=60pc)

11. Cuối cùng cấu hình port 8081 của container trong dịch vụ Fargate mà các kết nối sẽ được gửi đến và giao thức là TCP

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


