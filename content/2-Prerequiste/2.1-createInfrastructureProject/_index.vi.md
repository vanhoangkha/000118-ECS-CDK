---
title : "Tạo infrastructure project with AWS CDK"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 2.1 </b> "
---

#### infrastructure project sử dụng AWS CDK

1. Mở termial tại thư mục bạn muốn tạo project

![Architect](/images/1/infra/01.png?featherlight=false&width=60pc)

2. Nhập lệnh sau 

```bash
   cdk init app --language java
```

![Architect](/images/1/infra/02.png?featherlight=false&width=60pc)

{{% notice tip %}}
 Bài workshop này sẻ sử dụng java. Ngoài ra bạn có thể tạo bằng python hoặc typescipt
{{% /notice %}}

3. Mở IntelliJ IDEA và mở project chúng ta vừa tạo

![Architect](/images/1/infra/03.png?featherlight=false&width=60pc)