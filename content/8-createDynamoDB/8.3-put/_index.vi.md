---
title : "Tạo REST operation để UPDATE product bằng ID"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 8.3 </b> "
---

#### Tạo REST operation để cập nhật product bằng ID

1. Tạo một đối tượng Resource mới bằng cách gọi phương thức addResource("{id}") trên đối tượng productsResource. Đây là một phần của cấu hình định tuyến của API Gateway, cho phép xử lý các request đến /{id}.

```java
Resource productIdResource = productsResource.addResource("{id}");
```
![Architect](/images/8/post/01.png?featherlight=false&width=60pc)

2. Thêm một Method mới vào **productIdResource** bằng cách sử dụng phương thức **addMethod("PUT")** 

```java
productIdResource.addMethod("PUT")
```
![Architect](/images/8/post/02.png?featherlight=false&width=60pc)

3. Tiếp theo ta cần cấu hình Integration cho Method. Ta có thể sử dụng lại intergation ở phần trước và thay đổi integrationHttpMethod với phương thức HTTP là PUT

```java
productIdResource.addMethod("PUT", new Integration(
                IntegrationProps.builder()
                        .type(IntegrationType.HTTP_PROXY)
                        .integrationHttpMethod("PUT")
                        .uri("http://" + apiStackProps.networkLoadBalancer().getLoadBalancerDnsName() +
                                ":8080/api/products/{id}")
                        .options(IntegrationOptions.builder()
                                .vpcLink(apiStackProps.vpcLink())
                                .connectionType(ConnectionType.VPC_LINK)
                                .build())
                        .build()));
```
![Architect](/images/8/post/03.png?featherlight=false&width=60pc)

4. Sau đó chúng ta cần thêm requestParameter sau connection type

```java
.requestParameters()
```

![Architect](/images/8/post/04.png?featherlight=false&width=60pc)

5. Bạn có thể thấy requestParameters yêu cầu kiểu dữ liệu Map<String,Sring> hãy định nghĩa nó.
   
+ Trước tiên tạo Map tên là **``productIdIntegrationParameters``**
  - Trong đó **integration.request.path.id** như một key trong productIdIntegrationParameters và gán giá trị của nó thành method.request.path.id, bạn đang cấu hình API Gateway để lấy giá trị id từ path của request gốc mà client gửi đến, và truyền nó tới backend dưới cùng tên tham số trong request path của backend.


```java
Map<String, String> productIdIntegrationParameters = new HashMap<>();
productIdIntegrationParameters.put("integration.request.path.id", "method.request.path.id");
```

![Architect](/images/8/post/05.png?featherlight=false&width=60pc)

6. Sử dụng **MethodOptions.builder().requestParameters(productIdMethodParameters).build()** để thiết lập các tham số của method, trong trường hợp này là productIdMethodParameters, nơi chỉ định rằng path.id là bắt buộc khi gửi request.

- Trước tiên tạo Map tên là **productIdMethodParameters** để đảm bảo rằng ID là bắt buộc khi gửi request

```java
Map<String, Boolean> productIdMethodParameters = new HashMap<>();
productIdMethodParameters.put("method.request.path.id", true);
```

- Thêm **productIdMethodParameters** vào methodOptions

```java
MethodOptions.builder()
.requestParameters(productIdMethodParameters)
.build()
```

![Architect](/images/8/post/06.png?featherlight=false&width=60pc)
