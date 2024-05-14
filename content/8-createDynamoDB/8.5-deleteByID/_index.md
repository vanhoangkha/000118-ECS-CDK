---
title: " Create REST Operation to DELETE Product by ID"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: "<b>8.5</b>"
---

#### Create REST Operation to DELETE Product by ID

1. We can quickly create a DELETE method by ID by copying the previously created PUT method and replacing **PUT** with **DELETE**.

```java
productIdResource.addMethod("DELETE", new Integration(
                IntegrationProps.builder()
                        .type(IntegrationType.HTTP_PROXY)
                        .integrationHttpMethod("DELETE")
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

![Architect](/images/8/post/08.png?featherlight=false&width=60pc)