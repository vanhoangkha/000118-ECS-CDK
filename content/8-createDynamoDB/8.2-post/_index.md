---
title: " Create REST operation to create a new Product"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b>8.2</b>"
---

#### Create REST Operation to Create a New Product

1. Open the file **ApiStack.java**. Within the **createProductsResource** method, use the **addMethod** function to create a PUT method for the **productsResource** resource.


```java
productsResource.addMethod("POST");
```

![Architect](/images/8/post/01.png?featherlight=false&width=60pc)


2. Next, we will define an **Integration** to handle POST requests to the endpoint **/products**. This part is similar to the GET integration, but we just need to specify **integrationHttpMethod("POST")**.


```java
productsResource.addMethod("POST", new Integration(
                IntegrationProps.builder()
                        .type(IntegrationType.HTTP_PROXY)
                        .integrationHttpMethod("POST")
                        .uri("http://" + apiStackProps.networkLoadBalancer().getLoadBalancerDnsName() +
                                ":8081/api/products")
                        .options(IntegrationOptions.builder()
                                .vpcLink(apiStackProps.vpcLink())
                                .connectionType(ConnectionType.VPC_LINK)
                                .build())
                        .build()));
```

![Architect](/images/8/post/02.png?featherlight=false&width=60pc)
