---
title : "Tạo cloudformation stack đầu tiên"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 3.1 </b> "
---

#### Chuẩn bị cho CDK project

1. Xoá file **Fcj2024CdkStack** đã được tạo sẵn trước đó

![Architect](/images/3/createfirststack/01.png?featherlight=false&width=60pc)

2. Xoá những phần comment không cần thiết

![Architect](/images/3/createfirststack/02.png?featherlight=false&width=60pc)

3. Tạo 1 stack mới bằng việc tạo 1 file java mới và đặt tên là **EcrStack.java**


![Architect](/images/3/createfirststack/03.png?featherlight=false&width=60pc)

{{% notice tip %}}
  Bằng cách sử dụng CDK, bạn có thể viết mã để tạo các stack và tài nguyên của CloudFormation. Mã của bạn sẽ được biên dịch thành các template CloudFormation tương ứng, sau đó có thể được triển khai bởi CloudFormation như bình thường
{{% /notice %}}

4. Sau khi hoàn thành thành để class EcrStack kế thừa từ lớp Stack của thư viện Amzon CDK

![Architect](/images/3/createfirststack/04.png?featherlight=false&width=60pc)

5. Tạo stack contructor 

![Architect](/images/3/createfirststack/05.png?featherlight=false&width=60pc)

6. Tạo AWS resource thông qua repository
- Đầu tiên tạo thuộc tính **productsServiceRepository** bằng dòng lệnh bên dưới
  
  ```java
  private final Repository productsServiceRepository;
  ```

- Tiếp theo Khởi đạo thuộc tính **productsServiceRepository** trong contructor

  ```java
  this.productsServiceRepository = new Repository();
  ```

![Architect](/images/3/createfirststack/06.png?featherlight=false&width=60pc)

7. Để khởi tạo **Repository** ta thêm các parameter sau
 + **this**, this đề cập đến đối tượng lớp hiện tại (thường là một CDK stack).
 + **"ProductsService"**: Đây là tên duy nhất (ID) của tài nguyên được tạo. Tên này là duy nhất trong phạm vi của một stack CDK.
 + Sau đó tạo ECR Repository properties bằng **RepositoryProps.builder().build()**
    + builder() là một phương thức tĩnh của lớp RepositoryProps để khởi tạo một đối tượng Builder. Đây là một bước chuẩn bị để thiết lập các thuộc tính cho RepositoryProps.
    + Phương thức build() trên đối tượng Builder được gọi để kết thúc quá trình xây dựng và trả về một đối tượng RepositoryProps hoàn chỉnh với các thuộc tính đã thiết lập.

![Architect](/images/3/createfirststack/07.png?featherlight=false&width=60pc)

8. Tạo ECR repository **name** bằng cách thêm đoạn code **```.repositoryName("productsservice")```** trước build()

![Architect](/images/3/createfirststack/08.png?featherlight=false&width=60pc)

9. Them **.removalPolicy(RemovalPolicy.DESTROY)** để đảm bảo rằng khi khi xoá cloudformation stack resource của chúng ta cũng sẽ được xoá theo

![Architect](/images/3/createfirststack/09.png?featherlight=false&width=60pc)

10. Tiếp theo cho phép override các image tren CER repository bằng việc cho phép **IMMUTABLE** thông qua đoạn code **imageTagMutability(TagMutability.IMMUTABLE)**

![Architect](/images/3/createfirststack/10.png?featherlight=false&width=60pc)

11. Cho phép tự động xoá image khi xoá ECR bằng cách thêm code **autoDeleteImages(true)**

![Architect](/images/3/createfirststack/11.png?featherlight=false&width=60pc)

12. Tạo phương thức Getter
```
public Repository getProductsServiceRepository() {
        return productsServiceRepository;
    }
```

![Architect](/images/3/createfirststack/12.png?featherlight=false&width=60pc)

