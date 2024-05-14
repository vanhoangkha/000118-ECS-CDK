---
title : "Chuẩn bị IntelliJ IDEA"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 2.3 </b> "
---

#### Chuẩn bị IntelliJ IDEA

1. Dùng IntelliJ IDEA mở project spring boot vừa tạo

![Architect](/images/1/repareIntelli/01.png?featherlight=false&width=60pc)

2. Cài đặt project
- lick phải vào project chọn **Open Module Settings**
![Architect](/images/1/repareIntelli/02.png?featherlight=false&width=60pc)

3. Chọn **Project Setting**, hãy đảm bảo rằng project của bạn có **SDK** là **21** và **language level** là **21**
![Architect](/images/1/repareIntelli/03.png?featherlight=false&width=60pc)

4. Chọn **Platform setting**, hãy đảm bảo rằng platform **SDK** của bạn là **21**
![Architect](/images/1/repareIntelli/04.png?featherlight=false&width=60pc)

#### Chạy spring boot project

5. Mở file **application.properties** thêm vào **server.port=8081** để cấu hình project chạy trên port 8081
![Architect](/images/1/repareIntelli/05.png?featherlight=false&width=60pc)

6. Nhấn vào biểu tượng Run để chạy project, bạn có thể thấy project đang chạy trên port 8081
![Architect](/images/1/repareIntelli/06.png?featherlight=false&width=60pc)

#### Kiểm thử project trên postman

7. Tạo request HTTP **GET** với đường dẫn **```http://localhost:8080/actuator/health```** để kiểm tra
- Reponse trả về **"status": "UP"** chứng tỏ project hoạt đông bình thường

![Architect](/images/1/repareIntelli/07.png?featherlight=false&width=60pc)

#### Thêm thư viên **Log4j2**

8. Mở file **build.grade** 
   - Trong phần **dependencies** thêm thư viện Log4j2 bằng cách thêm đoạn mã sau **```implementation 'org.springframework.boot:spring-boot-starter-log4j2'```**
   - Thêm **configurations**
  ```
  configurations {
	configureEach {
		exclude group: 'org.springframework.boot', module: 'spring-boot-starter-logging'
		exclude group: 'commons-logging', module: 'commons-logging'
	}
}
  ```
![Architect](/images/1/repareIntelli/08.png?featherlight=false&width=60pc)


#### Tạo Controller
9. Trong thư mục **com.firstcloudjourney.productsservice** tạo folder **products**
![Architect](/images/1/repareIntelli/09.png?featherlight=false&width=60pc)

10. Trong thư mục **products** tạo thư mục **controllers** và tạo file **ProductsController.java** từ thư mục  **controllers**

![Architect](/images/1/repareIntelli/10.png?featherlight=false&width=60pc)

11. Copy đoạn code sau vào file  **ProductsController.java**

```java
@RestController
@RequestMapping("/api/products")
public class ProductsController {
    private static final Logger LOG = LogManager.getLogger(ProductsController.class);

    @GetMapping
    public String getAllProducts() {
        LOG.info("Get all products");
        return "All products";
    }
}
```

![Architect](/images/1/repareIntelli/11.png?featherlight=false&width=60pc)

12.  Chạy project và test trên postman bằng cách tạo request HTTP **GET** với đường dẫn **```http://localhost:8081/api/products```**

![Architect](/images/1/repareIntelli/12.png?featherlight=false&width=60pc)

13. Kiểm tra log trả về trên termnial khi gọi HTTP GET
![Architect](/images/1/repareIntelli/13.png?featherlight=false&width=60pc)

