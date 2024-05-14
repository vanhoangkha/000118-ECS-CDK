---
title: " Adding X-Ray Sidecar to ProductsServices Task Definition"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: "<b>9.3</b>"
---

#### Adding an X-Ray Sidecar Container to ProductsServices Task Definition

1. Open the **FCJ2024_SDK** project and navigate to the **ProductsServiceStack.java** file.

   ![Architect](/images/9/addsidecar/01.png/?featherlight=false&width=60pc)

2. Add environment variables for X-Ray as follows:

```java
   envVariables.put("AWS_XRAY_DAEMON_ADDRESS", "0.0.0.0:2000");
   envVariables.put("AWS_XRAY_CONTEXT_MISSING", "IGNORE_ERROR");
   envVariables.put("AWS_XRAY_TRACING_NAME", "productsservice");
```

![Architect](/images/9/addsidecar/02.png/?featherlight=false&width=60pc)

3. In the **fargateTaskDefinition.addContainer** section, add CPU as **348** and memory as **896**

```java
.cpu(384)
.memoryLimitMiB(896)

```

![Architect](/images/9/addsidecar/03.png/?featherlight=false&width=60pc)

4. Next, add a sidecar container (X-Ray) to the ProductsServices task definition as follows:

```java
fargateTaskDefinition.addContainer("xray", ContainerDefinitionOptions.builder()
                        .image(ContainerImage.fromRegistry("public.ecr.aws/xray/aws-xray-daemon:latest"))
                        .containerName("XRayProductsService")
                        .logging(new AwsLogDriver(AwsLogDriverProps.builder()
                                .logGroup(new LogGroup(this, "XRayLogGroup", LogGroupProps.builder()
                                        .logGroupName("XRayProductsService")
                                        .removalPolicy(RemovalPolicy.DESTROY)
                                        .retention(RetentionDays.ONE_MONTH)
                                        .build()))
                                .streamPrefix("XRayProductsService")
                                .build()))
                        .portMappings(Collections.singletonList(PortMapping.builder()
                                        .containerPort(2000)
                                        .protocol(Protocol.UDP)
                                .build()))
                        .cpu(128)
                        .memoryLimitMiB(128)
                .build());
```
![Architect](/images/9/addsidecar/04.png/?featherlight=false&width=60pc)

5. Cấp quyền để cho phép các containers trong task có khả năng gửi dữ liệu đến AWS X-Ray mà không cần quyền đọc hoặc quản lý các tài nguyên X-Ray. Điều này giúp việc theo dõi và phân tích hiệu suất của ứng dụng được thực hiện một cách an toàn và hiệu quả.
   
```java
fargateTaskDefinition.getTaskRole().addManagedPolicy(ManagedPolicy.fromAwsManagedPolicyName("AWSXrayWriteOnlyAccess"));
```

![Architect](/images/9/addsidecar/05.png/?featherlight=false&width=60pc)
