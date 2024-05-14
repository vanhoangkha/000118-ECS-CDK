---
title : " Triển khai Stack lên AWS ECR"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 3.3 </b> "
---

#### Triển khai Stack lên AWS ECR

1. Để triển khia stak lên AWS ECR chúng ta nhập **```cdk deploy Ecr --profile default``**

![Architect](/images/3/implement/01.png?featherlight=false&width=80pc)

2. Trong quá trình tạo resource, có vài câu hỏi về security policy chúng ta cần thay đổi. Nhập **``y``**

![Architect](/images/3/implement/02.png?featherlight=false&width=80pc)

3. Tạo ECR thành công

![Architect](/images/3/implement/03.png?featherlight=false&width=80pc)

4. Truy cập vào AWS console nhập **ECR** và chọn **Elastic Container Registry**


![Architect](/images/3/implement/04.png?featherlight=false&width=80pc)

5. Ta có thể thấy repository **productsservice** đã được tạo

![Architect](/images/3/implement/05.png?featherlight=false&width=80pc)

