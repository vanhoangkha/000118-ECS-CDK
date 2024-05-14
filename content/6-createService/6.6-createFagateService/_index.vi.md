---
title : "Tạo AWS Fargate service"
date :  "`r Sys.Date()`" 
weight : 6
chapter : false
pre : " <b> 6.6 </b> "
---

#### Tạo AWS Fargate service

1. Khởi tạo FargateService trong  ProductsServiceStack contructor với id là **ProductsService**

```java
  FargateService fargateService = new FargateService(this, "ProductsService",
                FargateServiceProps.builder()
                        .build());
```

![Architect](/images/6/fargate/10.png?featherlight=false&width=60pc)

2. Thêm service name là **ProductsService**

```java
.serviceName("ProductsService")
```

![Architect](/images/6/fargate/11.png?featherlight=false&width=60pc)

3. Tiếp theo chúng ta cần thêm cluster đã tạo trước đó để tạo fargate service

```java
  .cluster(productsServiceProps.cluster())
```
![Architect](/images/6/fargate/12.png?featherlight=false&width=60pc)

4. Ngoài cluster, chúng ta cần có task definition để tạo fragate. Chúng ta sẽ dùng **fargateTaskDefinition** đã tạo bên trên

```java
  .taskDefinition(fargateTaskDefinition)
```
![Architect](/images/6/fargate/13.png?featherlight=false&width=60pc)

5. Tiếp theo chúng ta sẽ khai báo bao nhiêu instance chúng ta muốn sử dụng. Trong workshop này chúng ta sẽ dử dụng 2

```java
  .desiredCount(2)
```
![Architect](/images/6/fargate/14.png?featherlight=false&width=60pc)

6. Tiếp theo để giữ fargate trong private subnet nối bên ngoài thông qua NAT Gateway chúng ta sẻ không thêm public IP

```java
   .assignPublicIp(false)
```
![Architect](/images/6/fargate/15.png?featherlight=false&width=60pc)

7. Ngoài ra nếu trong phần tạo PVC bạn không tạo NAT Gateway, bạn có thể làm như sau

```java
   .assignPublicIp(true)
```

{{% notice warning %}}
  Đừng làm trong môi trường production 
{{% /notice %}}

![Architect](/images/6/fargate/16.png?featherlight=false&width=60pc)

8. ECS service của chúng ta cần permission để pull image từ ECR repository. Vì vậy chúng ta cần phải cấp quyền như sau

```java
productsServiceProps.repository().grantPull(Objects.requireNonNull(fargateTaskDefinition.getExecutionRole()));
```
![Architect](/images/6/fargate/17.png?featherlight=false&width=60pc)


9. Cuối cùng 1 đều quan trọng nữa là chúng ta cần định nghĩa là service của chúng ta chấp nhận requests từ http port 8081

```java
fargateService.getConnections().getSecurityGroups().get(0).addIngressRule(Peer.anyIpv4(), Port.tcp(8081));
```
![Architect](/images/6/fargate/18.png?featherlight=false&width=60pc)

