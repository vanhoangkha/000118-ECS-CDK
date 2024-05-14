---
title : "Tạo VPC và Nat gateway"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 4.1 </b> "
---

#### Tạo VPC và Nat gateway

1. Mở CDK project tạo stack mới với tên là **VpcStack.java**

![Architect](/images/4/createVPC/01.png?featherlight=false&width=60pc)

2. Kế thừa Stack của thư viện awscdk vào tạo constructor 

![Architect](/images/4/createVPC/02.png?featherlight=false&width=60pc)

3. Tạo thuộc tính VPC và tạo phương thức getter
 - Thêm thuộc tính bằng đoạn code sau
   ```java
   private final Vpc vpc;
   ```
 - Tiếp đó chúng ta tạo hàm getter để có thể sử dụng VPC này cho các resource khác
    ```java
       public Vpc getVpc() {
        return vpc;
    }
   ```

![Architect](/images/4/createVPC/03.png?featherlight=false&width=60pc)

4. Khởi tạo đối tượng VPC bằng đoạn code sau trong contructor
    ```java
       this.vpc = new Vpc(this, "Vpc", VpcProps.builder() .build());
     
    ```
![Architect](/images/4/createVPC/04.png?featherlight=false&width=60pc)

5. Tiếp theo đặt tên cho PVC là **ECommerceVPC** và số lương Availability Zones là 2 thông qua đoạn code

   ```java
        .vpcName("ECommerceVPC")
        .maxAzs(2)
    
   ```
![Architect](/images/4/createVPC/05.png?featherlight=false&width=60pc)

6. Nếu bạn muốn VPC của bạn là Public và không sử dụng Nat gateway bạn có thể thêm đoạn code 
   
```java
  .natGateways(0)
```

{{% notice warning %}}
 Bạn có thể không sử dụng Nate gateway khi làm lab để tiết kiệm chi phí nhưng đừng làm trong môi trường production 
{{% /notice %}}

![Architect](/images/4/createVPC/06.png?featherlight=false&width=60pc)
