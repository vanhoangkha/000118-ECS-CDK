---
title : "Tổ chức và deploy NLB Stack"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 5.5 </b> "
---

#### Tổ chức và deploy NLB Stack

1. Mở file root **Fcj2024CdkApp** 

![Architect](/images/5/createNLB/14.png?featherlight=false&width=60pc)

2. Tạo NLB Stack với id là **Nlb** bằng đoạn code
```java
NlbStack nlbStack = new NlbStack(app, "Nlb", StackProps.builder()
                .build(),);
 ```

![Architect](/images/5/createNLB/15.png?featherlight=false&width=60pc)

3. Truyền VPC đã tạo trước đó cho ClusterStackProps của ECS stack 

```java
   new NlbStackProps(vpcStack.getVpc())
```
![Architect](/images/5/createNLB/16.png?featherlight=false&width=60pc)

4.  Thêm environmen và Tags cho NLB Stack

```java
   .env(environment)
   .tags(infraTags)
```

![Architect](/images/5/createNLB/17.png?featherlight=false&width=60pc)

5. Vì NLB cần có VPC để tạo. Hãy thêm phụ thuộc để đảm bảo rằng VPC được tạo hoàn thành trước khi tạo NLB

```java
nlbStack.addDependency(vpcStack);
```

![Architect](/images/5/createNLB/18.png?featherlight=false&width=60pc)

#### Deloy NLB bằng AWS CDK


6. Deploy NLB bằng AWS CDK

```bash
   cdk deploy --all --require-approval never
```

![Architect](/images/5/createNLB/19.png?featherlight=false&width=60pc)

#### Kiểm ta AWS resource đã tạo

7. Trong giao diện AWS console nhập EC2

![Architect](/images/5/createNLB/20.png?featherlight=false&width=60pc)

8. Trong giao diện EC2 chọn **Load balancers** ta có thể thấy NLB và ALB đã được tạo

![Architect](/images/5/createNLB/21.png?featherlight=false&width=60pc)

9. Trong giao diện EC2. Nhập **API gateway** vào than tìm kiếm và chọn **API Gateway**

![Architect](/images/5/createNLB/22.png?featherlight=false&width=60pc)

10. Trong gia diện **API Gateway** chọn **VPC links** ta có thể thấy VPC link đã được tạo thành công

![Architect](/images/5/createNLB/23.png?featherlight=false&width=60pc)
