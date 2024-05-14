---
title : "Cấu hình AWS ALB Target group và cơ chế health check"
date :  "`r Sys.Date()`" 
weight : 7
chapter : false
pre : " <b> 6.7 </b> "
---

##### Cầu hình AWS ALB Target group và cơ chế health check
1. Trước tiên tạo taget group từ **applicationListener** bằng phương thức **.addTargets** với ID là **```ProductsServiceAlbTarget```**

```java
applicationListener.addTargets("ProductsServiceAlbTarget",
                AddApplicationTargetsProps.builder()
                        .build()
                );
```
![Architect](/images/6/configTarget/01.png?featherlight=false&width=60pc)

2. Đặt tên cho target group là **```productsServiceAlb```**

```java
.targetGroupName("productsServiceAlb")
```
![Architect](/images/6/configTarget/02.png?featherlight=false&width=60pc)

3. Tiếp theo chúng ta sẽ tạo port 8001 để Load Balancer gửi traffic đến và dùng giao thức HTT để giao tiếp với các target

```java
.port(8081)
.protocol(ApplicationProtocol.HTTP)
```

![Architect](/images/6/configTarget/03.png?featherlight=false&width=60pc)

4. Tiếp thep ta cần thêm  danh sách các service Fargate vào target

```java
.targets(Collections.singletonList(fargateService))
```

![Architect](/images/6/configTarget/04.png?featherlight=false&width=60pc)

5. Tiếp theo chúng ta sẽ định nghĩa độ trễ cho phép là 30 giây trước khi Load Balancer hủy bỏ một target khi nó bị đánh dấu là không khả dụng

```java
.deregistrationDelay(Duration.seconds(30))
```

![Architect](/images/6/configTarget/05.png?featherlight=false&width=60pc)

6. Tiếp theo chúng ta sẽ cấu hình health check và bât tính năng health check để đảm bảo rằng các target đang hoạt động đúng cách.

```java
.healthCheck(HealthCheck.builder()
             .enabled(true)
             .build())
```

![Architect](/images/6/configTarget/06.png?featherlight=false&width=60pc)

7. Kế tiếp chúng ta sẽ cấu hình khoảng thời gian giữa các lần kiểm tra sức khỏe là 30 giây và thời gian tối đa để chờ đợi phản hồi từ một target trong mỗi lần kiểm tra là 10 giây

```java
.interval(Duration.seconds(30))
.timeout(Duration.seconds(10))
```
![Architect](/images/6/configTarget/07.png?featherlight=false&width=60pc)

8. Cuối cùng chúng ta sẽ khai báo đường dẫn(endpoint) được sử dụng để kiểm tra sức khỏe của các target là **```/actuator/health```** và port 8081 được sử dụng trong health check để giao tiếp với các target

![Architect](/images/6/configTarget/08.png?featherlight=false&width=60pc)
