---
title : "Tạo REST operation để DELETE product bằng ID"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 8.5 </b> "
---

#### Tạo REST operation để DELETE product bằng ID

1. Chúng ta có thể tạo nhanh DELETE method bằng ID bằng cách copy lại PUT method đã tạo và thay PUT bằng DELETE

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
