---
title: " Organize and Deploy NLB Stack"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: "<b>5.5</b>"
---

#### Organize and Deploy NLB Stack

1. Open the root file **Fcj2024CdkApp**.

   ![Architecture](/images/5/createNLB/14.png?featherlight=false&width=60pc)

2. Create an NLB Stack with the ID **Nlb** using the following code:

```java
NlbStack nlbStack = new NlbStack(app, "Nlb", StackProps.builder()
       .build());
 ```

![Architect](/images/5/createNLB/15.png?featherlight=false&width=60pc)

3. Pass the previously created VPC to the `ClusterStackProps` of the ECS stack:

```java
new ClusterStackProps(vpcStack.getVpc())
```
![Architect](/images/5/createNLB/16.png?featherlight=false&width=60pc)

4. Add environmen and Tags for NLB Stack

```java
   .env(environment)
   .tags(infraTags)
```

![Architect](/images/5/createNLB/17.png?featherlight=false&width=60pc)

5. Because the Network Load Balancer (NLB) requires a VPC to be created, let's add a dependency to ensure that the VPC is created before the NLB:
```java
   nlbStack.addDependency(vpcStack);
```

![Architect](/images/5/createNLB/18.png?featherlight=false&width=60pc)

#### Deploy NLB using AWS CDK

6. Deploy the Network Load Balancer (NLB) using AWS CDK:

```bash
   cdk deploy --all --require-approval never
```

![Architect](/images/5/createNLB/19.png?featherlight=false&width=60pc)

#### Check AWS Resources Created

7. In the AWS Management Console, navigate to EC2.

   ![Architecture](/images/5/createNLB/20.png?featherlight=false&width=60pc)

8. In the EC2 interface, select **Load Balancers** to view the created Network Load Balancer (NLB) and Application Load Balancer (ALB).

   ![Architecture](/images/5/createNLB/21.png?featherlight=false&width=60pc)

9. In the EC2 interface, enter "API Gateway" into the search bar and select **API Gateway** from the results.

   ![Architecture](/images/5/createNLB/22.png?featherlight=false&width=60pc)

10. In the **API Gateway** interface, select **VPC links** to view the successfully created VPC link.

   ![Architecture](/images/5/createNLB/23.png?featherlight=false&width=60pc)
