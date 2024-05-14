---
title: " Organize and Deploy Stack"
date: "`r Sys.Date()`"
weight: 9
chapter: false
pre: "<b>6.9</b>"
---

#### Organize ProductsServiceStack Stack

1. Open the root file **Fcj2024CdkApp**:

   ![Architect](/images/6/organizeStack/01.png?featherlight=false&width=60pc)

2. Create and Set Tags:

   - Create a `Map` named `productsServiceTags` to define tags for the stack and related resources. Tags are stored as key-value pairs in the map.

     ```java
     Map<String, String> productsServiceTags = new HashMap<>();
     productsServiceTags.put("team", "FirstCloudJourney");
     productsServiceTags.put("cost", "ProductsService");
     ```


![Architect](/images/6/organizeStack/02.png?featherlight=false&width=60pc)

3. Create Stack ProductsServiceStack

```java
 ProductsServiceStack productsServiceStack = new ProductsServiceStack(app, "ProductsService",
                StackProps.builder()
                        .env(environment)
                        .tags(productsServiceTags)
                        .build(),
                new ProductsServiceProps(
                        vpcStack.getVpc(),
                        clusterStack.getCluster(),
                        nlbStack.getNetworkLoadBalancer(),
                        nlbStack.getApplicationLoadBalancer(),
                        ecrStack.getProductsServiceRepository()));
```
![Architect](/images/6/organizeStack/03.png?featherlight=false&width=60pc)

4. Add Dependencies to Ensure VPC, Cluster, NLB, and ECR are Created Before Creating the Service:

```java
   productsServiceStack.addDependency(vpcStack);
   productsServiceStack.addDependency(clusterStack);
   productsServiceStack.addDependency(nlbStack);
   productsServiceStack.addDependency(ecrStack);
```

![Architect](/images/6/organizeStack/04.png?featherlight=false&width=60pc)

#### Deploy ProductsService Stack using AWS CDK

5. Open a terminal and run the following command to deploy:

```
cdk deploy --all --require-approval never
```

![Architect](/images/6/organizeStack/05.png?featherlight=false&width=60pc)

6. After successful deployment, access the AWS Management Console. Navigate to **ECS**:

![Architect](/images/6/organizeStack/06.png?featherlight=false&width=60pc)

7. In the **ECS** dashboard, you can see a cluster named **ECommerce** has been created along with 2 running tasks:

![Architect](/images/6/organizeStack/07.png?featherlight=false&width=60pc)

8. Select the **ECommerce** cluster and then choose a task to view details about the created tasks:

![Architect](/images/6/organizeStack/08.png?featherlight=false&width=60pc)
