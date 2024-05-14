---
title: " Create REST Operation to GET Product by ID"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: "<b>8.4</b>"
---

#### Create REST Operation to GET Product by ID

1. We can quickly create a GET method by ID by copying the previously created PUT method and replacing **PUT** with **GET**.

```java
        // GET /products/{id}
        productIdResource.addMethod("GET", new Integration(
                IntegrationProps.builder()
                        .type(IntegrationType.HTTP_PROXY)
                        .integrationHttpMethod("GET")
                        .uri("http://" + apiStackProps.networkLoadBalancer().getLoadBalancerDnsName() +
                                ":8080/api/products/{id}")
                        .options(IntegrationOptions.builder()
                                .vpcLink(apiStackProps.vpcLink())
                                .connectionType(ConnectionType.VPC_LINK)
                                .requestParameters(productIdIntegrationParameters)
                                .build())
                        .build()), MethodOptions.builder()
                .requestParameters(productIdMethodParameters)
                .build());
```

![Architect](/images/8/post/07.png?featherlight=false&width=60pc)
