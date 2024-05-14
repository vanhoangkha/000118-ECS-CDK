---
title: " Create a New Stack for Product Service"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b>6.1</b>"
---

#### Create a New Stack for Product Service

1. Open your CDK project and create a new stack named **ProductsServiceStack**. Have **ProductsServiceStack** inherit from the `Stack` class from the `amazon.awscdk` library.

   ![Architecture](/images/6/newstack/01.png?featherlight=false&width=60pc)

2. To create the `ProductsServiceStack`, we need to include several dependencies such as VPC, Cluster, NetworkLoadBalancer, ApplicationLoadBalancer, and Repository that were created earlier. Therefore, we need to define a record class named **ProductsServiceProps** to pass into the `ProductsServiceStack`. Add the following code snippet outside the `ClusterStack` class:

```java
   record ProductsServiceProps(
       Vpc vpc,
       Cluster cluster,
       NetworkLoadBalancer networkLoadBalancer,
       ApplicationLoadBalancer applicationLoadBalancer,
       Repository repository
   ){}
```

![Architect](/images/6/newstack/02.png?featherlight=false&width=60pc)

3. Create a Constructor:

```java
   public ProductsServiceStack(final Construct scope, final String id,
                               final StackProps props, ProductsServiceProps productsServiceProps) {
       super(scope, id, props);
   }

```

![Architect](/images/6/newstack/03.png?featherlight=false&width=60pc)

