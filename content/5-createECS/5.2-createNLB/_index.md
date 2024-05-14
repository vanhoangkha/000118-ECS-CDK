---
title: " Create Network Load Balancer"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b>5.2</b>"
---

#### Create Network Load Balancer stack

1. In your CDK project, create a new stack named **NlbStack.java** and have the `NlbStack` class inherit from the `Stack` class from the `amazon.awscdk` library.

   ![Architecture](/images/5/createNLB/01.png?featherlight=false&width=60pc)

2. To create the NLB stack, we need the VPC that we created earlier. Therefore, we will create a record class named **NlbStackProps** to pass the VPC into the NLB Stack. Add the following code outside the `ClusterStack` class:

```java
   record NlbStackProps(
       Vpc vpc
   ){}
```

![Architect](/images/5/createNLB/02.png?featherlight=false&width=60pc)

3. Inside the NLB Stack, we will create **Vpc Link**, **Network Load Balancer**, and **Application Load Balancer**. Therefore, we need to define three attributes corresponding to them:

```java
   private final VpcLink vpcLink;
   private final NetworkLoadBalancer networkLoadBalancer;
   private final ApplicationLoadBalancer applicationLoadBalancer;
```
![Architect](/images/5/createNLB/03.png?featherlight=false&width=60pc)

4. Next, let's create getters for the three attributes:

```java
   public VpcLink getVpcLink() {
       return vpcLink;
   }

   public NetworkLoadBalancer getNetworkLoadBalancer() {
       return networkLoadBalancer;
   }

   public ApplicationLoadBalancer getApplicationLoadBalancer() {
       return applicationLoadBalancer;
   }
```

![Architect](/images/5/createNLB/04.png?featherlight=false&width=60pc)

5. Create a constructor for the `NlbStack` class:

```java
   public NlbStack(final Construct scope, final String id,
                   final StackProps props, NlbStackProps nlbStackProps) {
       super(scope, id, props);
   }
```

![Architect](/images/5/createNLB/05.png?featherlight=false&width=60pc)

#### Create Network Load Balancer

6. First, let's create a Network Load Balancer (NLB). To do this, we need to initialize the `networkLoadBalancer` object within the constructor:

```java
   this.networkLoadBalancer = new NetworkLoadBalancer(this, "Nlb",
       NetworkLoadBalancerProps.builder()
           .build());
```
![Architect](/images/5/createNLB/06.png?featherlight=false&width=60pc)

7. Next, set the name of the NLB to **ECommerceNlb** using:
```java
   .loadBalancerName("ECommerceNlb")
```

![Architect](/images/5/createNLB/07.png?featherlight=false&width=60pc)

8. As shown in the design diagram, we will not expose the Network Load Balancer (NLB) and Application Load Balancer (ALB) to the internet outside of the VPC. Therefore, we will configure them as follows:

```java
.internetFacing(false)
.vpc(nlbStackProps.vpc())
```
![Architect](/images/5/createNLB/08.png?featherlight=false&width=60pc)

