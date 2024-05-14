---
title : "Tổ chức CDK stack in CDK project"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 3.2 </b> "
---

#### Tổ chức CDK stack in CDK project

1. Mở file root **Fcj2024CdkApp** 

![Architect](/images/3/orstack/01.png?featherlight=false&width=60pc)

2. Tạo ECR Stack bằng đoạn code
```java
EcrStack ecrStack = new EcrStack(app, "Ecr", StackProps.builder()
                .build());
```

![Architect](/images/3/orstack/02.png?featherlight=false&width=60pc)

3. Tạo **Evirontment** bằng đoạn code

```
Environment environment = Environment.builder()
                .account("your_account_id")
                .region("ap-southeast-1")
                .build();
```
+ Với account là Account ID của tài khoản AWS của bạn 

![Architect](/images/3/orstack/0301.png?featherlight=false&width=60pc)

+ Với region là region bạn muốn tạo resource ở đây chúng ta sẽ chọn **``ap-southeast-1``** 

![Architect](/images/3/orstack/03.png?featherlight=false&width=60pc)

4. Thêm **Evirontment** vào ECRStack bằng đoạn code 
```
.env(environment)
```

![Architect](/images/3/orstack/04.png?featherlight=false&width=60pc)

5. Tạo thuộc tính **infraTags** cho Tags

```
Map<String, String> infraTags = new HashMap<>();
        infraTags.put("team", "FirstCloudJourney");
        infraTags.put("cost", "ECommerceInfra");
```

![Architect](/images/3/orstack/05.png?featherlight=false&width=60pc)

6. Thêm Tags cho ECRStack

```
.tags(infraTags)
```

![Architect](/images/3/orstack/06.png?featherlight=false&width=60pc)

7. Kiểm tra các stack mà CDK sẽ tạo cho cloudformation bằng lệnh
```
cdk list
```
- ở đây CDK cho chúng ta thấy có 1 stack duy nhất đó là Ecr

![Architect](/images/3/orstack/07.png?featherlight=false&width=60pc)

8. Để chuẩn bị môi trường triển khai cho các ứng dụng CDK trên AWS, bao gồm việc tạo các tài nguyên cần thiết để triển khai và quản lý các ứng dụng CDK một cách hiệu quả trên nền tảng AWS chúng ta cần chạy lệnh
```
cdk bootstrap --profile default
```

![Architect](/images/3/orstack/08.png?featherlight=false&width=60pc)

9. Để xem những gì chúng ta vừa toạ từ lệnh **cdk bootstrap --profile default** truy cập vào **AWS console** nhập **Cloudformation**

![Architect](/images/3/orstack/09.png?featherlight=false&width=60pc)


10. Trong giao diện **Cloudformation**, ta có thể thấy môt **CDKToolkit** vừ được tạo

![Architect](/images/3/orstack/10.png?featherlight=false&width=60pc)

11. Chọn **CDKToolkit** và sau đó chọn **resource** tab để xem những resource đã được tạo 

![Architect](/images/3/orstack/11.png?featherlight=false&width=60pc)
