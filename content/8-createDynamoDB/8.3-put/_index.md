---
title: " Create REST Operation to Update Product by ID"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: "<b>8.3</b>"
---

#### Create REST Operation to Update Product by ID

1. Create a new Resource object by calling the `addResource("{id}")` method on the `productsResource` object. This is part of configuring the routing of the API Gateway, allowing for handling requests to `/{id}`.

```java
Resource productIdResource = productsResource.addResource("{id}");
```
![Architect](/images/8/post/01.png?featherlight=false&width=60pc)

2. Add a new Method to the **productIdResource** by using the **addMethod("PUT")** method.
```java
   productIdResource.addMethod("PUT");
```
![Architect](/images/8/post/02.png?featherlight=false&width=60pc)

3. Next, we need to configure an Integration for the Method. We can reuse the integration from the previous section and change the **integrationHttpMethod** to **PUT** for the HTTP method.


```java
productIdResource.addMethod("PUT", new Integration(
                IntegrationProps.builder()
                        .type(IntegrationType.HTTP_PROXY)
                        .integrationHttpMethod("PUT")
                        .uri("http://" + apiStackProps.networkLoadBalancer().getLoadBalancerDnsName() +
                                ":8080/api/products/{id}")
                        .options(IntegrationOptions.builder()
                                .vpcLink(apiStackProps.vpcLink())
                                .connectionType(ConnectionType.VPC_LINK)
                                .build())
                        .build()));
```
![Architect](/images/8/post/03.png?featherlight=false&width=60pc)

4. Then, we need to add requestParameters under connectionType().

```java
.requestParameters()
```

![Architect](/images/8/post/04.png?featherlight=false&width=60pc)

5. The **requestParameters** method requires a data type of **Map<String, String>**. Let's define it.

   + First, create a **Map** named **productIdIntegrationParameters**:
     - Within this map, use **"integration.request.path.id"** as a key and assign its value to **"method.request.path.id"**. This configuration sets up API Gateway to extract the **id** value from the incoming request path sent by the client and pass it to the backend with the same parameter name in the backend's request path.

```java
   Map<String, String> productIdIntegrationParameters = new HashMap<>();
   productIdIntegrationParameters.put("integration.request.path.id", "method.request.path.id");
```
6. Use **MethodOptions.builder().requestParameters(productIdMethodParameters).build()** to configure the method parameters, in this case **productIdMethodParameters**, which specifies that **path.id** is required when sending a request.

   + First, create a Map named **productIdMethodParameters** to ensure that **ID** is required when sending a request:

   ```java
   Map<String, Boolean> productIdMethodParameters = new HashMap<>();
   productIdMethodParameters.put("method.request.path.id", true);
   ```
   + Then, include **productIdMethodParameters** in methodOptions
```java
MethodOptions.builder()
.requestParameters(productIdMethodParameters)
.build()
```

![Architect](/images/8/post/06.png?featherlight=false&width=60pc)

