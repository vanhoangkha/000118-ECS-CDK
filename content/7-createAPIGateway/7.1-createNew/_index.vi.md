---
title : "Tạo API Gateway resource"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 7.1 </b> "
---

#### Tạo API Gateway resource

1. Mở CDK project tạo stack mới với tên là **ApiStack** và cho **ApiStack** kế thừa lớp Stack của thư viện amazon.awscdk

```java
public class ApiStack extends Stack {
    
}
```

![Architect](/images/7/1/01.png?featherlight=false&width=60pc)

2. Để tạo ApiStack chúng ta cần VPC, Cluster, NetworkLoadBalancer mà chúng ta đã tạo trước đó. Nên chúng ta cần tạo 1 record class tên là **ProductsServiceProps** để truyền vào ApiStack. Thêm đoạn code bên ngoài class ApiStack

```java
record ApiStackProps(
        NetworkLoadBalancer networkLoadBalancer,
        VpcLink vpcLink
){}
```

![Architect](/images/7/1/02.png?featherlight=false&width=60pc)

3. Tạo ApiStack contructor

```java
public ApiStack(final Construct scope, final String id,
                                final StackProps props, ApiStackProps apiStackProps) {
        super(scope, id, props);
        }
```
![Architect](/images/7/1/03.png?featherlight=false&width=60pc)

4. Khởi tạo Một đối tượng RestApi, được đặt tên là **ECommerceAPI**. Đây là một API Gateway, nơi các API endpoint được khai báo và cấu hình.

```java
   RestApi restApi = new RestApi(this, "RestApi",
                RestApiProps.builder()
                        .restApiName("ECommerceAPI")
                        .build()
                );
```
![Architect](/images/7/1/04.png?featherlight=false&width=60pc)

