---
title: " Push Docker image to ECR"
date: "`r Sys.Date()`"
weight: 4
chapter: false
pre: "<b>3.4</b>"
---

#### Push Docker image to ECR repository

1. Ensure you have installed the AWS Toolkit. If not, go to IntelliJ IDEA **Settings**, select **Plugins**, and search for **AWS Toolkit** to install it.

   ![Architect](/images/3/pushImage/01.png?featherlight=false&width=80pc)

2. Click on the AWS Toolkit icon on the right side of IntelliJ IDEA.

   ![Architect](/images/3/pushImage/02.png?featherlight=false&width=80pc)

3. Select profile and region:
   - For **Profile**, choose **default**.
   - For **Region**, choose **ap-southeast-1**.

   ![Architect](/images/3/pushImage/03.png?featherlight=false&width=80pc)

4. Open the **Explorer** to view all AWS Toolkit-supported services, then select **ECR**.

   ![Architect](/images/3/pushImage/04.png?featherlight=false&width=80pc)

5. Right-click on the **productsservice** repository and choose **Push to Repository...** to push the Docker image.

   ![Architect](/images/3/pushImage/05.png?featherlight=false&width=80pc)

6. In the **Push to ECR** popup:
   - For **Local Image**, select **productsservice:1.0.0**.
   - For **ECR Repository**, choose **productsservice**.
   - For **Remote Tag**, enter **`1.0.0`**.
   - Finally, click **Push**.

   ![Architect](/images/3/pushImage/06.png?featherlight=false&width=80pc)

# Check the image on ECR

7. In the AWS Management Console, navigate to **ECR**.

   ![Architect](/images/3/pushImage/07.png?featherlight=false&width=80pc)

8. Select the **productsservice** repository to view the Docker image version **1.0.0** that has been pushed.

   ![Architect](/images/3/pushImage/08.png?featherlight=false&width=80pc)
