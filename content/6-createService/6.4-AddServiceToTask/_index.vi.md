---
title : " Thêm container service vào task definition"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 6.4 </b> "
---

#### Thêm container service vào task definition

1. Để thêm container phương thức **addContainer** của **fargateTaskDefinition** đã khởi tạo trước đó

```java
fargateTaskDefinition.addContainer()
```
![Architect](/images/6/addContainer/01.png?featherlight=false&width=60pc)

2. Thêm id là **ProductsServiceContainer** và tạo **ContainerDefinitionOptions**

```java
   fargateTaskDefinition.addContainer("ProductsServiceContainer",
                ContainerDefinitionOptions.builder()
                        .build());
```

![Architect](/images/6/addContainer/02.png?featherlight=false&width=60pc)

3. Thêm image từ ECR Repository với phiên bản **1.0.0** chúng ta đã tạo trước đó

```java
   .image(ContainerImage.fromEcrRepository(productsServiceProps.repository(), "1.0.0"))
```
![Architect](/images/6/addContainer/03.png?featherlight=false&width=60pc)

4. Tiếp theo ta cần đặt tên cho Container và là thêm logDriver đã tạo trước đó
```java
    .containerName("productsService")
    .logging(logDriver)
```
![Architect](/images/6/addContainer/04.png?featherlight=false&width=60pc)

5. Bời vì ứng dụng của chúng ta sử dũng port 8081, do đó chúng ta cần thêm port cho container service

```java
 .portMappings(Collections.singletonList(PortMapping.builder()
                                        .containerPort(8081)
                                        .protocol(Protocol.TCP)
                                .build()))
```

![Architect](/images/6/addContainer/05.png?featherlight=false&width=60pc)

6. Tiếp theo chúng ta cần tạo environtment. Giá trị của nó là một Map<String, String>
   + Trước tiên chúng ta cần khai báo 1 biến envVariables  

```java
Map<String, String> envVariables = new HashMap<>();
        envVariables.put("SERVER_PORT", "8081");
```
   + Tiếp theo thêm environment cho service container
  
```java
    .environment(envVariables)
```
![Architect](/images/6/addContainer/06.png?featherlight=false&width=60pc)
