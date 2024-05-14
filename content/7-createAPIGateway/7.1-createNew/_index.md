---
title: " Create API Gateway Resource"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b>7.1</b>"
---

#### Create API Gateway Resource

1. Open your CDK project and create a new stack named **ApiStack**. Inherit the `Stack` class from the `amazon.awscdk` library:

```java
   public class ApiStack extends Stack {
       
   }

```

![Architect](/images/7/1/01.png?featherlight=false&width=60pc)

2. To create `ApiStack`, we require the VPC, Cluster, and NetworkLoadBalancer that we previously set up. Therefore, we need to create a record class named **ApiStackProps** to pass into `ApiStack`. Add the following code outside the `ApiStack` class:

```java
   record ApiStackProps(
           NetworkLoadBalancer networkLoadBalancer,
           VpcLink vpcLink
   ){}
```

![Architect](/images/7/1/02.png?featherlight=false&width=60pc)

3. Create the `ApiStack` constructor:

```java
   public ApiStack(final Construct scope, final String id,
                   final StackProps props, ApiStackProps apiStackProps) {
       super(scope, id, props);
   }
```
![Architect](/images/7/1/03.png?featherlight=false&width=60pc)

4. Initialize a `RestApi` object named **ECommerceAPI**. This represents an API Gateway where API endpoints are declared and configured.
```java
   RestApi restApi = new RestApi(this, "RestApi",
                RestApiProps.builder()
                        .restApiName("ECommerceAPI")
                        .build()
                );
```
![Architect](/images/7/1/04.png?featherlight=false&width=60pc)

