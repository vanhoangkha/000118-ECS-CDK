---
title: " Organize CDK Stack in CDK Project"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b>3.2</b>"
---

#### Organize CDK Stack in CDK Project

1. Open the root file **Fcj2024CdkApp**.

![Architect](/images/3/orstack/01.png?featherlight=false&width=60pc)

2. Create an ECR Stack using the following code snippet:
   
   ```java
   EcrStack ecrStack = new EcrStack(app, "Ecr", StackProps.builder().build());
   ```
![Architect](/images/3/orstack/02.png?featherlight=false&width=60pc)

3. Create an **Environment** using the following code:

    ```java
    Environment environment = Environment.builder()
        .account("your_account_id")
        .region("ap-southeast-1")
        .build();
    ```

   - Replace `your_account_id` with your AWS account ID.
   - Use `ap-southeast-1` as the desired AWS region for resource creation.

   ![Architect](/images/3/orstack/0301.png?featherlight=false&width=60pc)
   ![Architect](/images/3/orstack/03.png?featherlight=false&width=60pc)

4. Add the **Environment** to the ECRStack using the following code:

    ```java
    .env(environment)
    ```

   ![Architect](/images/3/orstack/04.png?featherlight=false&width=60pc)

5. Create properties **infraTags** for Tags:

    ```java
    Map<String, String> infraTags = new HashMap<>();
    infraTags.put("team", "FirstCloudJourney");
    infraTags.put("cost", "ECommerceInfra");
    ```

   ![Architect](/images/3/orstack/05.png?featherlight=false&width=60pc)

6. Apply Tags to the ECRStack:

    ```java
    .tags(infraTags)
    ```

   ![Architect](/images/3/orstack/06.png?featherlight=false&width=60pc)

7. Check the stacks that CDK will create for CloudFormation using the command:

    ```
    cdk list
    ```

   - Here, CDK shows that only one stack (`Ecr`) will be created.

   ![Architect](/images/3/orstack/07.png?featherlight=false&width=60pc)

8. To prepare the deployment environment for CDK applications on AWS, including creating necessary resources for deployment and managing CDK applications effectively on the AWS platform, run the command:

    ```
    cdk bootstrap --profile default
    ```

   ![Architect](/images/3/orstack/08.png?featherlight=false&width=60pc)

9. To view the resources created after running **cdk bootstrap --profile default**, access the **AWS console** and navigate to **CloudFormation**.

   ![Architect](/images/3/orstack/09.png?featherlight=false&width=60pc)

10. In the **CloudFormation** interface, you can see a **CDKToolkit** stack has been created.

    ![Architect](/images/3/orstack/10.png?featherlight=false&width=60pc)

11. Select **CDKToolkit** and then choose the **Resources** tab to view the created resources.

    ![Architect](/images/3/orstack/11.png?featherlight=false&width=60pc)
