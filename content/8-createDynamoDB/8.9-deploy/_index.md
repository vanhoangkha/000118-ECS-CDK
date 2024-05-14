---
title: " Perform Deployment for Testing"
date: "`r Sys.Date()`"
weight: 9
chapter: false
pre: "<b>8.9</b>"
---

#### Perform Deployment for Testing

1. In the **productsservice** project, select the **Gradle** icon and choose **jar** to package the project.

   ![Architect](/images/8/implement/01.png/?featherlight=false&width=60pc)

2. Next, create a new image. Open the **DockerFile** and select **Edit dockerfile**.

   ![Architect](/images/8/implement/02.png/?featherlight=false&width=60pc)

3. In the **Edit Dockerfile** interface, change the image tag to `1.1.0` to create a new Docker image, then select **Run**.

   ![Architect](/images/8/implement/03.png/?featherlight=false&width=60pc)

4. Push the newly created image to the ECR Repository by opening **AWS Toolkit**, selecting **ECR**, then choosing **productsservice**. Finally, right-click and select **Push to Repository**.

   ![Architect](/images/8/implement/04.png/?featherlight=false&width=60pc)
5. In the **Push to ECR** popup, select the Local image as **productsservice:1.0.0** and set the **Remote Tag** to **1.1.0**, then click **Push**.

   ![Architect](/images/8/implement/05.png/?featherlight=false&width=60pc)

6. Verify the pushed image on the **ECR** interface.

   ![Architect](/images/8/implement/06.png/?featherlight=false&width=60pc)

7. Open the **FCJ2024_CDK** project. Then open the file **ProductsServiceStack** and update the tag in the **fargateTaskDefinition.addContainer** section to **`1.1.0`**.

   ![Architect](/images/8/implement/07.png/?featherlight=false&width=60pc)

8. Next, use **`cdk deploy --all --require-approval never`** to deploy the changes.

   ![Architect](/images/8/implement/08.png/?featherlight=false&width=60pc)

9. Check if the new task definition is running by navigating to the **ECS** interface, selecting the **Cluster**, then selecting **ECommerce**, and finally selecting **task**.

   ![Architect](/images/8/implement/09.png/?featherlight=false&width=60pc)

10. Access the DynamoDB interface to verify if the product table has been created.

    ![Architect](/images/8/implement/10.png/?featherlight=false&width=60pc)
