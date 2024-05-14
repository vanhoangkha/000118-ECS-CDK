---
title : "Tạo REST operation để GET product bằng ID"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 8.4 </b> "
---

#### Tạo REST operation để GET product bằng ID

1. Chúng ta có thể tạo nhanh GET method bằng ID bằng cách copy lại PUT method đã tạo và thay PUT bằng GET

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
