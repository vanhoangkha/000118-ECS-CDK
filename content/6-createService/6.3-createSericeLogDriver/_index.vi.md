---
title : "Tạo Service Log Driver"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 6.3 </b> "
---

#### Tạo Service Log Driver

1. Trong quá trình phát triển a spring boot application, chúng ta cần log để debug application do đó chúng ta cần sử dụng Log Group của CloudWatch
   
2. Để tạo log group, trước tiên ta cần khởi tạo đối tượng **AwsLogDriver** trong contructor
```java
   AwsLogDriver logDriver = new AwsLogDriver(AwsLogDriverProps.builder()
                .build());
```

![Architect](/images/6/logDriver/02.png?featherlight=false&width=60pc)

3. Khởi tạo đối tượng **LogGroup** trong **AwsLogDriver**

```java
    .logGroup(new LogGroup(this, "LogGroup",
        LogGroupProps.builder()
        .build()))
```
![Architect](/images/6/logDriver/03.png?featherlight=false&width=60pc)

4. Tiếp theo chúng ta cần thêm các thuộc tính cần thiết cho LogGroup
  + Với tên cho groug log là **ProductsService**
   ```java
   .logGroupName("ProductsService")
   ```
  + Tiếp theo cho ta sẽ cấu hình xoá log group khi delete Stack
   ```java
   .removalPolicy(RemovalPolicy.DESTROY)
   ```
  + Cuối cùng cho ta sẽ cho AWS biết là sẽ giữ những log này trong bao lâu. Ỡ đâu chúng ta sẽ giữ 1 tháng
   ```java
   .retention(RetentionDays.ONE_MONTH)
   ```

![Architect](/images/6/logDriver/04.png?featherlight=false&width=60pc)

5. Cuối cùng chúng tao cần tạo prefix cho file của chúng ta

```java
    .streamPrefix("ProductsService")
```
![Architect](/images/6/logDriver/05.png?featherlight=false&width=60pc)

