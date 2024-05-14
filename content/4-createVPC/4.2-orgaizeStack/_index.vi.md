---
title : "Tổ chức và deploy VPC stack sử dụng AWS CDK"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---

#### Tổ chức VPC stack in CDK project

1. Mở file root **Fcj2024CdkApp** 

![Architect](/images/3/orstack/01.png?featherlight=false&width=60pc)

2. Tạo VPC Stack với id là **Vpc** bằng đoạn code
```java
VpcStack vpcStack = new VpcStack(app, "Vpc", StackProps.builder()
                .build());
 ```

![Architect](/images/4/createVPC/07.png?featherlight=false&width=60pc)

3. Thêm **environment** và **Tags**

![Architect](/images/4/createVPC/08.png?featherlight=false&width=60pc)

#### Deploy VPC stack
4. Bật termial và nhập lệnh **```cdk list``** để xem danh sách các task đang có. Ở đây ta có thể thấy **Ecr** và **Vpc**


![Architect](/images/4/createVPC/09.png?featherlight=false&width=60pc)


5. Tiếp theo nhập **cdk deploy --all** để deploy tất cả stack. Khi được hỏi về những thay đổi policy. Nhập **y**

![Architect](/images/4/createVPC/10.png?featherlight=false&width=60pc)

6. Kiểm tra những thay đổi khi deploy. Ta có thể thấy rằng CDK nhận ra CER không đổi và chúng ta đang tạo một stack mới là VPC

![Architect](/images/4/createVPC/11.png?featherlight=false&width=60pc)

#### Kiểm tra resouce đã tạo trên aws console

7. Truy cập vào aws console vào giao diện VPC và chọn **Your VPCs**. Ta có thể thấy **ECommerceVPC** đã được tạo

![Architect](/images/4/createVPC/12.png?featherlight=false&width=60pc)


8. Tương tự như vậy với Subnet,NAT gateway, security group...

![Architect](/images/4/createVPC/13.png?featherlight=false&width=60pc)
