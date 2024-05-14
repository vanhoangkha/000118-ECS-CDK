---
title : "Testing the implementation"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 9.4 </b> "
---

#### Testing the implementation

1. Quay trở là **productsservice** project. Đóng gói project bằng jar trong Grade

![Architect](/images/9/implement/010.png/?featherlight=false&width=60pc)

2. Mở DockerFile và tạo image tag là **productsservice:1.3.0**

![Architect](/images/9/implement/02.png/?featherlight=false&width=60pc)

3. Sau khi hoàn thành buid Docker Image. Push nó lên ECR Repository với Remote Tag là **1.3.0**

![Architect](/images/9/implement/03.png/?featherlight=false&width=60pc)


4. Trở lại **FCJ2024_CDK** project thay đổi image tag của **fargateTaskDefinition.addContainer** là **1.3.0**

![Architect](/images/9/implement/04.png/?featherlight=false&width=60pc)

5. Deploy service bằng dòng lệnh

```
cdk deploy --all --require-approval never
```

![Architect](/images/9/implement/05.png/?featherlight=false&width=60pc)

6. Sau khi hoành thành deploy. Chúng ta truy cập vào giao diện **ECS** chọn **Task**. Sau đó chọn 1 task bất kỳ trong 2 task đang chạy.

![Architect](/images/9/implement/06.png/?featherlight=false&width=60pc)

7. Trong giao diện **task** vừa được chọn. Ta có thể thấy một container phụ (sidecar) tên **XRayProductsService** đang chạy cùng với **productsService** container

![Architect](/images/9/implement/07.png/?featherlight=false&width=60pc)

8. Tiếp theo chúng ta sẽ test API trên postman để kiểm tra Đo lường dịch vụ AWS ECS trên AWS X-ray

![Architect](/images/9/implement/08.png/?featherlight=false&width=60pc)

9. Truy cập vào giao diện **CloudWatch** chọn **Trace Map** để xem được đường đi của request

![Architect](/images/9/implement/09.png/?featherlight=false&width=60pc)

10. Cuối cùng để xem thông tin do lường của ECS chúng ta chọn biểu tượng ECS trên **trace map**

![Architect](/images/9/implement/10.png/?featherlight=false&width=60pc)

11. Tượng tự với DynamoDB

![Architect](/images/9/implement/11.png/?featherlight=false&width=60pc)

12. Mở Postman tạo phương thức Post để tạo một product mới
