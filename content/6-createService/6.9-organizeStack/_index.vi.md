---
title : "Tổ chức và deploy Stack"
date :  "`r Sys.Date()`" 
weight : 9
chapter : false
pre : " <b> 6.9 </b> "
---

#### Tổ chức ProductsServiceStack Stack

1. Mở file root **Fcj2024CdkApp** 

![Architect](/images/6/organizeStack/01.png?featherlight=false&width=60pc)

2. Tạo và Thiết lập Tags:

- Tạo ra một Map có tên là productsServiceTags để định nghĩa các tag (nhãn) cho stack và các tài nguyên liên quan. Các tag được lưu trữ dưới dạng cặp key-value trong map.

```java
 Map<String, String> productsServiceTags = new HashMap<>();
        productsServiceTags.put("team", "FirstCloudJourney");
        productsServiceTags.put("cost", "ProductsService");
```

![Architect](/images/6/organizeStack/02.png?featherlight=false&width=60pc)

3. Tạo Stack ProductsServiceStack

```java
 ProductsServiceStack productsServiceStack = new ProductsServiceStack(app, "ProductsService",
                StackProps.builder()
                        .env(environment)
                        .tags(productsServiceTags)
                        .build(),
                new ProductsServiceProps(
                        vpcStack.getVpc(),
                        clusterStack.getCluster(),
                        nlbStack.getNetworkLoadBalancer(),
                        nlbStack.getApplicationLoadBalancer(),
                        ecrStack.getProductsServiceRepository()));
```
![Architect](/images/6/organizeStack/03.png?featherlight=false&width=60pc)

4. Thêm Dependency để đảm bảo rằng VPC,Cluster,NLB và ECR được tạo trước khi tạo service

```java
productsServiceStack.addDependency(vpcStack);
productsServiceStack.addDependency(clusterStack);
productsServiceStack.addDependency(nlbStack);
productsServiceStack.addDependency(ecrStack);
```

![Architect](/images/6/organizeStack/04.png?featherlight=false&width=60pc)

#### Deloy ProductsService Stack bằng AWS CDK

5. Mở termial và nhập

```
cdk deploy --all --require-approval never
```
![Architect](/images/6/organizeStack/05.png?featherlight=false&width=60pc)

6. Sau khi deploy thành công. Truy cập vào giao diện AWS console. Nhập **ECS**

![Architect](/images/6/organizeStack/06.png?featherlight=false&width=60pc)

7. Trong giao diện **ECS** ta có thể thấy 1 cluster tên **ECommerce** đã được tạo cùng với 2 task đang chạy

![Architect](/images/6/organizeStack/07.png?featherlight=false&width=60pc)

8. Chọn Cluster **ECommerce** và sau đó chọn task để xem thông tinh về các task đã tạo

![Architect](/images/6/organizeStack/08.png?featherlight=false&width=60pc)
