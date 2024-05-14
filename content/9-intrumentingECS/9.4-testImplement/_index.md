---
title: "Testing the implementation"
date: "`r Sys.Date()`" 
weight: 4
chapter: false
pre: "<b>9.4</b>"
---

#### Testing the implementation

1. Switch back to the **productsservice** project. Package the project into a JAR using Gradle.

![Architect](/images/9/implement/010.png/?featherlight=false&width=60pc)

2. Open the Dockerfile and create an image tag named **productsservice:1.3.0**.

![Architect](/images/9/implement/02.png/?featherlight=false&width=60pc)

3. Once the Docker image build is complete, push it to the ECR Repository with a remote tag of **1.3.0**.

![Architect](/images/9/implement/03.png/?featherlight=false&width=60pc)

4. Return to the **FCJ2024_CDK** project and update the image tag in **fargateTaskDefinition.addContainer** to **1.3.0**.

![Architect](/images/9/implement/04.png/?featherlight=false&width=60pc)

5. Deploy the service using the following command:

```
cdk deploy --all --require-approval never
```

![Architect](/images/9/implement/05.png/?featherlight=false&width=60pc)

6. After completing the deployment, access the **ECS** interface and select **Task**. Then choose any task from the running tasks.

![Architect](/images/9/implement/06.png/?featherlight=false&width=60pc)

7. In the selected **task** interface, you can see a sidecar container named **XRayProductsService** running alongside the **productsService** container.

![Architect](/images/9/implement/07.png/?featherlight=false&width=60pc)

8. Next, we will test the API in Postman to measure the AWS ECS service using AWS X-Ray.

![Architect](/images/9/implement/08.png/?featherlight=false&width=60pc)

9. Access the **CloudWatch** interface and select **Trace Map** to view the request path.

![Architect](/images/9/implement/09.png/?featherlight=false&width=60pc)

10. Finally, to view ECS measurement information, select the ECS icon on the **trace map**.

![Architect](/images/9/implement/10.png/?featherlight=false&width=60pc)

11. Similarly, with DynamoDB.

![Architect](/images/9/implement/11.png/?featherlight=false&width=60pc)


