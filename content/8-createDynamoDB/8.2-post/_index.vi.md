---
title : "Tạo REST operation để tạo product mới"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 8.2 </b> "
---

#### Tạo REST operation để tạo product mới

1. Mở file **ApiStack.java** trong **createProductsResource** dùng phương thức **addMethod** để tạo method PUT cho **productsResource** resource

```java
productsResource.addMethod("POST");
```

![Architect](/images/8/post/01.png?featherlight=false&width=60pc)


2. Tiếp theo chúng ta sẽ định nghĩa một **Integration** để xử lý yêu cầu POST tới endpoint **/products**. Phần này tương tự với GET chúng ta chỉ cần thay **integrationHttpMethod("POST")**

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
