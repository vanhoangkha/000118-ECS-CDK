---
title: " Deploy Stack to AWS ECR"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: "<b>3.3</b>"
---

#### Deploy Stack to AWS ECR

1. To deploy the stack to AWS ECR, run the following command:

   ```bash
   cdk deploy Ecr --profile default
   ```

2. During the resource creation process, if prompted with security policy questions, enter **`y`** to proceed.

   ![Architect](/images/3/implement/02.png?featherlight=false&width=80pc)

3. The creation of the ECR repository is successful.

   ![Architect](/images/3/implement/03.png?featherlight=false&width=80pc)

4. Access the AWS console, navigate to **ECR**, and select **Elastic Container Registry**.

   ![Architect](/images/3/implement/04.png?featherlight=false&width=80pc)

5. You will see that the **productsservice** repository has been successfully created.

   ![Architect](/images/3/implement/05.png?featherlight=false&width=80pc)

