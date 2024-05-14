---
title : "Tổ chức Stack"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 7.3 </b> "
---

#### Tổ chức Stack

1. Mở file root **Fcj2024CdkApp** 

![Architect](/images/7/3/01.png?featherlight=false&width=60pc)

2. Tạo **ApiStack** với id là **``Api``**

```java
ApiStack apiStack = new ApiStack(app, "Api", StackProps.builder()
                .build(), 
                new ApiStackProps(
                        nlbStack.getNetworkLoadBalancer(),
                        nlbStack.getVpcLink()));
```

![Architect](/images/7/3/02.png?featherlight=false&width=60pc)

3.  Thêm environmen* và Tags cho ApiStack

```java
   .env(environment)
   .tags(infraTags)
```
![Architect](/images/7/3/03.png?featherlight=false&width=60pc)

4. Thêm phụ thuộc để đảm bảo rằng NLB được tao trước API Gateway

```java
apiStack.addDependency(nlbStack);
```
![Architect](/images/7/3/04.png?featherlight=false&width=60pc)

#### Kiểm tra resource

5. Trong giao diện AWS console, nhập API Gateway

![Architect](/images/7/3/05.png?featherlight=false&width=60pc)

6. Trong giao diện **API Gateway** chọn **APIs** ta có thể thấy một API tên **ECommerceAPI** đã được tạo

![Architect](/images/7/3/06.png?featherlight=false&width=60pc)

7. Chọn **ECommerceAPI** sau đó chọn **GET** 

![Architect](/images/7/3/07.png?featherlight=false&width=60pc)

#### Test API trên postman

8. Trong giao diện **ECommerceAPI** chọn **Stages** và copy **Invoke URL**

![Architect](/images/7/3/08.png?featherlight=false&width=60pc)

9. Mở postman và tạo phương thức GET với đường dẫn **```Invoke URL/product```**

![Architect](/images/7/3/09.png?featherlight=false&width=60pc)
