---
title : "Tổ chức Stack"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 7.3 </b> "
---

#### Tổ chức Stack

1. Mở file root **Fcj2024CdkApp** 

![Architect](/images/7/3/01.png?featherlight=false&width=60pc)

2. Tạo **ApiStack** với id là **``Api``**

```java
ApiStack apiStack = new ApiStack(app, "Api", StackProps.builder()
                .build(), 
                new ApiStackProps(
                        nlbStack.getNetworkLoadBalancer(),
                        nlbStack.getVpcLink()));
```

![Architect](/images/7/3/02.png?featherlight=false&width=60pc)

3.  Add environment and tags to the ApiStack.

```java
   .env(environment)
   .tags(infraTags)
```
![Architect](/images/7/3/03.png?featherlight=false&width=60pc)

4. Add a dependency to ensure that the Network Load Balancer (NLB) is created before the API Gateway.

```java
apiStack.addDependency(nlbStack);

```
![Architect](/images/7/3/04.png?featherlight=false&width=60pc)

#### Resource Check

5. In the AWS console interface, navigate to API Gateway.

   ![Architect](/images/7/3/05.png?featherlight=false&width=60pc)

6. Within the **API Gateway** interface, select **APIs** to find an API named **ECommerceAPI** that has been created.

   ![Architect](/images/7/3/06.png?featherlight=false&width=60pc)

7. Choose **ECommerceAPI** and then select **GET**.

   ![Architect](/images/7/3/07.png?featherlight=false&width=60pc)

#### Test API on Postman

8. Within the **ECommerceAPI** interface, navigate to **Stages** and copy the **Invoke URL**.

   ![Architect](/images/7/3/08.png?featherlight=false&width=60pc)

9. Open Postman and create a GET request with the path **```Invoke URL/product```**.

   ![Architect](/images/7/3/09.png?featherlight=false&width=60pc)
