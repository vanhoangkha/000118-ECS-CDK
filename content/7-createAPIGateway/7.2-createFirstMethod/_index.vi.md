---
title : "Tạo product resource và method đầu tiên"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 7.2 </b> "
---

#### Tạo product resource và method đầu tiên

1. Tạo phương thức **createProductsResource** với 2 tham số là RestApi và ApiStackProps và khởi tạo trong contructor

```java
private void createProductsResource(RestApi restApi, ApiStackProps apiStackProps) {
}
```
![Architect](/images/7/2/01.png?featherlight=false&width=60pc)

2. Tạo một tài nguyên (resource) mới trong API Gateway với đường dẫn là **/products**. Sử dụng **restApi.getRoot().addResource("products")** sẽ tạo ra một tài nguyên con mới (sub-resource) có đường dẫn là **/products** từ root của API (restApi).

```java
//products
Resource productsResource = restApi.getRoot().addResource("products");
```
![Architect](/images/7/2/02.png?featherlight=false&width=60pc)

3. Thêm phương thức GET cho productsResource bằng phương thức **addMethod**

```java
productsResource.addMethod("GET");
```
![Architect](/images/7/2/03.png?featherlight=false&width=60pc)

4. Tiếp theo chúng ta sẽ định nghĩa một **Integration** để xử lý yêu cầu GET tới endpoint **/products**.

```java
productsResource.addMethod("GET", new Integration(
                IntegrationProps.builder()
                        .build()));
```
![Architect](/images/7/2/04.png?featherlight=false&width=60pc)

5. Sau đó, chọn loại **Integration** là **HTTP_PROXY**, nghĩa là sử dụng proxy HTTP để chuyển tiếp yêu cầu và chọn Loại phương thức của Integration là GET.

```java
.type(IntegrationType.HTTP_PROXY)
.integrationHttpMethod("GET")
```

![Architect](/images/7/2/05.png?featherlight=false&width=60pc)

6. Đặt URI của backend mà API Gateway sẽ chuyển tiếp yêu cầu tới. Trong đó, DNS name của NetworkLoadBalancer được sử dụng để tạo URL và cổng 8081 cùng với đường dẫn API /api/products được gọi.

```java
.uri("http://" + apiStackProps.networkLoadBalancer().getLoadBalancerDnsName() +":8080/api/products")
```

![Architect](/images/7/2/06.png?featherlight=false&width=60pc)

7. Tiếp theo dùng options được sử dụng để cấu hình thêm các tùy chọn **IntegrationOptions**

```java
    .options(IntegrationOptions.builder()
    .build())
```
![Architect](/images/7/2/07.png?featherlight=false&width=60pc)

8. Chỉ định VpcLink để xác định kết nối VPC nào sẽ được sử dụng cho phương thức Integration và xác định loại kết nối là VPC_LINK, nghĩa là kết nối qua VPC của AWS.

```java
.vpcLink(apiStackProps.vpcLink())
.connectionType(ConnectionType.VPC_LINK)
```
![Architect](/images/7/2/08.png?featherlight=false&width=60pc)
