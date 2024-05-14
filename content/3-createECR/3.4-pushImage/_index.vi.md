---
title : " Push Docker image lên ECR"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 3.4 </b> "
---

#### Push Docker image lên ECR repository

1. Hãy đảm bảo là bạn đã cài AWS toolkit. Nếu chưa vào IntelliJ IDEA **setting** chọn **plugins** và tìm **AWS toolkit** để cài đặt

![Architect](/images/3/pushImage/01.png?featherlight=false&width=80pc)

2. Chọn biểu tưởng AWS toolkit bên phải phần mềm IntelliJ IDEA

![Architect](/images/3/pushImage/02.png?featherlight=false&width=80pc)

3. Chọn profile và region
- Với profile chọn **defautl**
- Region chọn **ap-southeast-1**

![Architect](/images/3/pushImage/03.png?featherlight=false&width=80pc)

4. Chọn **Exploxer** để xem tất cả dịch vụ AWS toolkit hỗ trợ sau đó chọn **ECR**

![Architect](/images/3/pushImage/04.png?featherlight=false&width=80pc)

5. Ta có thể thấy có 2 repository chúng ta sẽ push docker image lên **productsservice** repository bằng cách lick phải vào  **productsservice** và chọn **push to Repository...**

![Architect](/images/3/pushImage/05.png?featherlight=false&width=80pc)

6. Pop up **Push to ECR** hiện lên 
- Với **local image** chọn **productsservice:1.0.0**
- Với **ECR Repository** chọn **productsservice**
- Với **Remote Tag** nhập **```1.0.0```**
- Cuối cùng chọn **Push**

![Architect](/images/3/pushImage/06.png?featherlight=false&width=80pc)

# Kiểm tra image trên ECR

7. Trong giao diện AWS console truy cập vào **ECR**

![Architect](/images/3/pushImage/07.png?featherlight=false&width=80pc)

8. Chọn **productsservice** Repository ta có thể thấy docker image phiên bản 1.0.0 đã được push lên


![Architect](/images/3/pushImage/08.png?featherlight=false&width=80pc)

