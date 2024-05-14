---
title: " Add Container Service to Task Definition"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: "<b>6.4</b>"
---

#### Add Container Service to Task Definition

1. To add a container, use the **addContainer** method of the previously initialized **fargateTaskDefinition**:

```java
fargateTaskDefinition.addContainer()
```
![Architect](/images/6/addContainer/01.png?featherlight=false&width=60pc)

2. Specify the ID as **ProductsServiceContainer** and create **ContainerDefinitionOptions**:

```java
fargateTaskDefinition.addContainer("ProductsServiceContainer",
       ContainerDefinitionOptions.builder()
           .build());
```

![Architect](/images/6/addContainer/02.png?featherlight=false&width=60pc)

3. Thêm image từ ECR Repository với phiên bản **1.0.0** chúng ta đã tạo trước đó

3. Add an image from the ECR Repository with version **1.0.0** that we created earlier:

```java
.image(ContainerImage.fromEcrRepository(productsServiceProps.repository(), "1.0.0"))
```
![Architect](/images/6/addContainer/03.png?featherlight=false&width=60pc)

4. Next, set the name for the container and add the previously created logDriver:
```java
   .containerName("productsService")
   .logging(logDriver)
```
![Architect](/images/6/addContainer/04.png?featherlight=false&width=60pc)

5. Since our application uses port 8081, we need to add port mappings for the container service:
```java
   .portMappings(Collections.singletonList(PortMapping.builder()
       .containerPort(8081)
       .protocol(Protocol.TCP)
       .build()))

```

![Architect](/images/6/addContainer/05.png?featherlight=false&width=60pc)

6. Next, we need to create environment variables. Its value is a `Map<String, String>`:
   + First, declare a variable `envVariables`:

     ```java
     Map<String, String> envVariables = new HashMap<>();
     envVariables.put("SERVER_PORT", "8081");
     ```

   + Then, add environment variables to the service container:

     ```java
     .environment(envVariables)
     ```

![Architect](/images/6/addContainer/06.png?featherlight=false&width=60pc)
