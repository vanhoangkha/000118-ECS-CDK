---
title : "Tạo X-Ray inspector"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 9.3 </b> "
---

#### Thêm một container phụ (sidecar) X-Ray vào ProductsServices task definition

1. Mở project **FCJ2024_SDK** và chọn file **ProductsServiceStack.java**

![Architect](/images/9/addsidecar/01.png/?featherlight=false&width=60pc)

2. Thêm các biến mội trường cho X-Ray như sau

```java
envVariables.put("AWS_XRAY_DAEMON_ADDRESS", "0.0.0.0:2000");
envVariables.put("AWS_XRAY_CONTEXT_MISSING", "IGNORE_ERROR");
envVariables.put("AWS_XRAY_TRACING_NAME", "productsservice");
```
![Architect](/images/9/addsidecar/02.png/?featherlight=false&width=60pc)

3. Trong phần **fargateTaskDefinition.addContainer** thêm CPU là **348** và memory là **896**

```java
.cpu(384)
.memoryLimitMiB(896)
```

![Architect](/images/9/addsidecar/03.png/?featherlight=false&width=60pc)

4. Tiếp theo, thêm một container phụ (sidecar) X-Ray vào ProductsServices task definition như sau

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
