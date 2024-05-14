---
title: " Creating an Infrastructure Project with AWS CDK"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b>2.1</b>"
---

#### Setting up an Infrastructure Project using AWS CDK

1. Open a terminal in the directory where you want to create the project.

   ![Terminal](/images/1/infra/01.png?featherlight=false&width=60pc)

2. Enter the following command to initialize a new AWS CDK application using Java:

   ```bash
   cdk init app --language java
   ```
![Architect](/images/1/infra/02.png?featherlight=false&width=60pc)

{{% notice tip %}}
This workshop will use Java. Alternatively, you can create the project with Python or TypeScript.
{{% /notice %}}

3. Open IntelliJ IDEA and import the project you just created.

![Architect](/images/1/infra/03.png?featherlight=false&width=60pc)