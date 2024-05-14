---
title: " Create AWS Fargate Service"
date: "`r Sys.Date()`"
weight: 6
chapter: false
pre: "<b>6.6</b>"
---

#### Create AWS Fargate Service

1. Initialize a `FargateService` within the `ProductsServiceStack` constructor with the ID **ProductsService**:

```java
   FargateService fargateService = new FargateService(this, "ProductsService",
       FargateServiceProps.builder()
           .build());

```

![Architect](/images/6/fargate/10.png?featherlight=false&width=60pc)

2. Set the service name to **ProductsService**:

```java
   .serviceName("ProductsService")

```

![Architect](/images/6/fargate/11.png?featherlight=false&width=60pc)

3. Next, specify the cluster that was created earlier to create the Fargate service:

```java
.cluster(productsServiceProps.cluster())

```
![Architect](/images/6/fargate/12.png?featherlight=false&width=60pc)

4. In addition to the cluster, we need to specify the task definition to use for creating the Fargate service. We will use the `fargateTaskDefinition` that was created earlier:

```java
   .taskDefinition(fargateTaskDefinition)

```
![Architect](/images/6/fargate/13.png?featherlight=false&width=60pc)

5. Next, specify the desired number of instances to use. In this workshop, we will use 2 instances:

```java
.desiredCount(2)

```
![Architect](/images/6/fargate/14.png?featherlight=false&width=60pc)

6. Next, to keep the Fargate tasks in a private subnet and allow outbound connectivity through a NAT Gateway without assigning a public IP:

```java
   .assignPublicIp(false)

```
![Architect](/images/6/fargate/15.png?featherlight=false&width=60pc)

7. Additionally, if you did not create a NAT Gateway in the VPC creation phase, you can enable public IP assignment for the Fargate service:
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


9. Finally, another important aspect is defining that our service accepts requests on HTTP port 8081:

```java
fargateService.getConnections().getSecurityGroups().get(0).addIngressRule(Peer.anyIpv4(), Port.tcp(8081));

```
![Architect](/images/6/fargate/18.png?featherlight=false&width=60pc)



