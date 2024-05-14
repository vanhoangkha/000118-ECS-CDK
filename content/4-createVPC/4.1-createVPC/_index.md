---
title: " Create VPC and Nat Gateway"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b>4.1</b>"
---

#### Create VPC and NAT Gateway

1. Open your CDK project and create a new stack named **VpcStack.java**.

   ![Step 1](/images/4/createVPC/01.png?featherlight=false&width=60pc)

2. Inherit from `Stack` from the `awscdk` library and create a constructor.

   ![Step 2](/images/4/createVPC/02.png?featherlight=false&width=60pc)

3. Define the VPC property and create a getter method.
   - Add the property in the `VpcStack.java` class:
     ```java
     private final Vpc vpc;
     ```
   - Then, create a getter method to access this VPC for other resources:
     ```java
     public Vpc getVpc() {
         return vpc;
     }
     ```

   ![Step 3](/images/4/createVPC/03.png?featherlight=false&width=60pc)

4. Initialize the VPC object in the constructor of `VpcStack.java` with the following code:
   ```java
   this.vpc = new Vpc(this, "Vpc", VpcProps.builder().build());
    ```
![Architect](/images/4/createVPC/04.png?featherlight=false&width=60pc)

5. Next, name the VPC as **ECommerceVPC** and specify the number of Availability Zones as 2 using the following code:

   ```java
   .vpcName("ECommerceVPC")
   .maxAzs(2)
   ```
![Architect](/images/4/createVPC/05.png?featherlight=false&width=60pc)

6. If you want your VPC to be public and not use a NAT Gateway, you can add the following code:

   ```java
   .natGateways(0)
   ```

{{% notice warning %}}
You may choose not to use a NAT Gateway in a lab environment to save costs, but avoid doing this in a production environment.
{{% /notice %}}

![Architect](/images/4/createVPC/06.png?featherlight=false&width=60pc)

