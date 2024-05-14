---
title: " Organize and Deploy VPC Stack using AWS CDK"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b>4.2</b>"
---

#### Organize VPC stack in CDK project

1. Open the root file **Fcj2024CdkApp**.

   ![Step 1](/images/3/orstack/01.png?featherlight=false&width=60pc)

2. Create a VPC Stack with the ID **Vpc** using the following code:

   ```java
   VpcStack vpcStack = new VpcStack(app, "Vpc", StackProps.builder()
                   .build());
   ```
![Architect](/images/4/createVPC/07.png?featherlight=false&width=60pc)

3. Add **environment** and **Tags**

   ![Step 3](/images/4/createVPC/08.png?featherlight=false&width=60pc)

#### Deploy VPC stack

4. Open a terminal and run the command **`cdk list`** to view the list of available tasks. Here, you will see **Ecr** and **Vpc**.

   ![Step 4](/images/4/createVPC/09.png?featherlight=false&width=60pc)

5. Next, run **`cdk deploy --all`** to deploy all stacks. When prompted about changeset execution policy, enter **y**.

   ![Step 5](/images/4/createVPC/10.png?featherlight=false&width=60pc)

6. Review the changes during deployment. You will see that CDK recognizes that the ECR has not changed and that a new stack for VPC is being created.

   ![Step 6](/images/4/createVPC/11.png?featherlight=false&width=60pc)

#### Check created resources on AWS Console

7. Access the AWS Console, navigate to the VPC interface, and select **Your VPCs**. You should see **ECommerceVPC** listed.

   ![Step 7](/images/4/createVPC/12.png?featherlight=false&width=60pc)

8. Similarly, check for Subnets, NAT Gateways, Security Groups, etc.

   ![Step 8](/images/4/createVPC/13.png?featherlight=false&width=60pc)
