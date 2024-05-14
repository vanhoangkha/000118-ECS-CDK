---
title : "Tạo ECS cluster"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 5.1 </b> "
---

#### Tạo ECS Cluster stack

1. Mở CDK project tạo stack mới với tên là **ClusterStack.java**

![Architect](/images/5/ecsCLuster/01.png?featherlight=false&width=60pc)

2. Để tạo ECS stack chúng ta cần VPC mà chúng ta đã tạo trước đó. Nên chúng ta cần tạo 1 record class để truyền VPC vào ECS Stack. Thêm đoạn code bên ngoài class ClusterStack

```java
record ClusterStackProps(
    Vpc vpc
){}
```

![Architect](/images/5/ecsCLuster/02.png?featherlight=false&width=60pc)

3. Tạo thuộc tính  **cluster** và phương thức getter
```java
private final Cluster cluster;

public Cluster getCluster() {
        return cluster;
    }
```
![Architect](/images/5/ecsCLuster/03.png?featherlight=false&width=60pc)


4. Tạo constructor 

```java
public ClusterStack(final Construct scope, final String id,
            final StackProps props, ClusterStackProps clusterStackProps) {
        super(scope, id, props);
            }
```

![Architect](/images/5/ecsCLuster/04.png?featherlight=false&width=60pc)

5.  Khởi tạo đối tượng VPC bằng đoạn code sau trong contructor
```java
       this.cluster = new Cluster(this, "Cluster", ClusterProps.builder()
                .build());
```

![Architect](/images/5/ecsCLuster/05.png?featherlight=false&width=60pc)

6. Thêm tên của cluster là **ECommerce** 
```java
       .clusterName("ECommerce")
```

![Architect](/images/5/ecsCLuster/06.png?featherlight=false&width=60pc)

7. Thêm VPC vào cluster
 ```java
       .vpc(clusterStackProps.vpc())
```
![Architect](/images/5/ecsCLuster/07.png?featherlight=false&width=60pc)

8. Cuối cùng là set containerInsight bằng true

```java
       .containerInsights(true)
```

![Architect](/images/5/ecsCLuster/08.png?featherlight=false&width=60pc)

#### Tổ chức ECS stack in CDK project

9. Mở file root **Fcj2024CdkApp** 

![Architect](/images/5/ecsCLuster/09.png?featherlight=false&width=60pc)

10. Tạo ECS Stack với id là **Vpc** bằng đoạn code
```java
ClusterStack clusterStack = new ClusterStack(app, "Cluster", StackProps.builder()
                .build());
 ```

![Architect](/images/5/ecsCLuster/10.png?featherlight=false&width=60pc)


11. Truyền VPC đã tạo trước đó cho ClusterStackProps của ECS stack 

```java
   new ClusterStackProps(vpcStack.getVpc())
```
![Architect](/images/5/ecsCLuster/11.png?featherlight=false&width=60pc)

12. Thêm environmen* và Tags cho ECS Stack

```java
   .env(environment)
   .tags(infraTags)
```

![Architect](/images/5/ecsCLuster/12.png?featherlight=false&width=60pc)

13. Bởi vì ECS cần có VPC nhưng chúng ta không thể biết chắc chắn rằng VPC có được tạo trước CES hay không. Vì vậy hãy thêm một ràng buộc là khi nào việc tạo VPC hoàn thành rồi ECS mới được tạo bằng cách

```java
   clusterStack.addDependency(vpcStack);
```

![Architect](/images/5/ecsCLuster/13.png?featherlight=false&width=60pc)

#### Deloy Ecs cluster bằng AWS CDK

14. Để deloy ecs cluster, mở terminal và nhập

+ Ta có thể deploy tất cả các stack bằng 
  
```bash
   cdk deploy --all
```

+ Ngoài ra chúng ta có thể bỏ qua bước confirm bằng cách 

```bash
   cdk deploy --all --require-approval never
```

![Architect](/images/5/ecsCLuster/14.png?featherlight=false&width=60pc)

#### Deloy Ecs cluster bằng AWS CDK

15. Kiểm tra cluster vừa tạo

Truy cập vào AWS console, Nhập **ECS**, chọn **Elastic Container Service**

![Architect](/images/5/ecsCLuster/15.png?featherlight=false&width=60pc)

16. Trong giao diện ECS, ta có thể thấy 1 cluster tên **ECommerce** đã được tạo

![Architect](/images/5/ecsCLuster/16.png?featherlight=false&width=60pc)

