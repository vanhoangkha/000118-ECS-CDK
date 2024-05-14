---
title: " Create ECS Cluster"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b>5.1</b>"
---

#### Create ECS Cluster stack

1. Open your CDK project and create a new stack named **ClusterStack.java**.

   ![Step 1](/images/5/ecsCLuster/01.png?featherlight=false&width=60pc)

2. To create the ECS stack, we need the VPC that we previously created. Therefore, we need to create a record class to pass the VPC into the ECS Stack. Add the following code outside the `ClusterStack` class:

   ```java
   record ClusterStackProps(Vpc vpc) {}
   ```
![Architect](/images/5/ecsCLuster/02.png?featherlight=false&width=60pc)

3. Create the **cluster** property and a getter method within the `ClusterStack` class:

   ```java
   private final Cluster cluster;

   public Cluster getCluster() {
       return cluster;
   }
   ```
![Architect](/images/5/ecsCLuster/03.png?featherlight=false&width=60pc)

4. Create constructor 

```java
public ClusterStack(final Construct scope, final String id,
            final StackProps props, ClusterStackProps clusterStackProps) {
        super(scope, id, props);
            }
```

![Architect](/images/5/ecsCLuster/04.png?featherlight=false&width=60pc)

5.  Initialize the Cluster object in the constructor of `ClusterStack.java` using the following code:

```java
       this.cluster = new Cluster(this, "Cluster", ClusterProps.builder()
                .build());
```

![Architect](/images/5/ecsCLuster/05.png?featherlight=false&width=60pc)

6. Set the name of the cluster to **ECommerce*** 
```java
       .clusterName("ECommerce")
```

![Architect](/images/5/ecsCLuster/06.png?featherlight=false&width=60pc)

7. Add VPC to the cluster

 ```java
.vpc(clusterStackProps.vpc())
```
![Architect](/images/5/ecsCLuster/07.png?featherlight=false&width=60pc)

8. Finally, set containerInsight to true.

```java
       .containerInsights(true)
```

![Architect](/images/5/ecsCLuster/08.png?featherlight=false&width=60pc)

#### Organize ECS stack in CDK project

9. Open the file named **Fcj2024CdkApp** in the root directory.

   ![Architecture](/images/5/ecsCLuster/09.png?featherlight=false&width=60pc)

10. Create an ECS Stack with the ID **Vpc** using the following code snippet:
   ```java
   ClusterStack clusterStack = new ClusterStack(app, "Cluster", StackProps.builder()
                   .build());
   ````
![Architect](/images/5/ecsCLuster/10.png?featherlight=false&width=60pc)

11. Pass the previously created VPC to the ClusterStackProps of the ECS stack:

```java
   new ClusterStackProps(vpcStack.getVpc())
```
![Architect](/images/5/ecsCLuster/11.png?featherlight=false&width=60pc)

12. Add environment and tags to the ECS Stack:

```java
   .env(environment)
   .tags(infraTags)
```

![Architect](/images/5/ecsCLuster/12.png?featherlight=false&width=60pc)

13. Because ECS requires a VPC but we cannot be certain whether the VPC has been created before the ECS, let's add a dependency constraint to ensure that the ECS is only created after the VPC has been successfully created:

```java
   clusterStack.addDependency(vpcStack);
```

![Architect](/images/5/ecsCLuster/13.png?featherlight=false&width=60pc)

#### Deploy ECS Cluster using AWS CDK

14. To deploy the ECS cluster, open your terminal and enter the following commands:

+ Deploy all stacks using:

```bash
cdk deploy --all
```

+ Alternatively, you can skip the confirmation prompt by using:
 
```bash
   cdk deploy --all --require-approval never
```

![Architect](/images/5/ecsCLuster/14.png?featherlight=false&width=60pc)

#### Deploy ECS Cluster using AWS CDK

15. Check the newly created cluster:

    + Access the AWS console, navigate to **ECS**, and select **Elastic Container Service**.

    ![Architecture](/images/5/ecsCLuster/15.png?featherlight=false&width=60pc)

16. In the ECS interface, you should see a cluster named **ECommerce** that has been created.

    ![Architecture](/images/5/ecsCLuster/16.png?featherlight=false&width=60pc)
