---
title: "Preparation Steps"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b>2.</b>"
---

#### Preparation Steps

1. **Java Development Kit (JDK)**
   - Firstly, we need to install the **Java Development Kit (JDK)**. In this workshop, we will use **JDK 21 LTS**. You can download it [here](https://www.oracle.com/java/technologies/downloads/#java21).
   - Next, we need to install **Maven**. You can install it following the instructions [here](https://maven.apache.org/install.html).

2. **NodeJS**
   - To install the **AWS CDK CLI**, we need to install NodeJS. Visit [this link](https://nodejs.org/en/download/) to download the latest LTS version.
   - Verify if NodeJS is installed correctly by running the following command in the terminal:

     ```bash
     node -v
     ```

     This command will display the installed NodeJS version:

     ```
     v18.15.0
     ```

   - Check the installed NPM version by running the following command in the terminal:

     ```bash
     npm -v
     ```

     This command will display the installed NPM version:

     ```
     9.6.2
     ```

3. **AWS CLI**
   - Visit [this link](https://aws.amazon.com/cli/) and download the latest version of AWS CLI.
   - After installation is complete, open a terminal and verify the installed AWS CLI version using the following command:

     ```bash
     aws --version
     ```

4. **AWS CDK**
   - AWS Cloud Development Kit, or AWS CDK, will be used to build infrastructure-as-code responsible for creating the application infrastructure using AWS services.
   - After installing the above packages, run the following command in your terminal window to install AWS CDK:

     ```bash
     npm install -g aws-cdk
     ```

     - Verify if CDK is installed correctly by running the following command in the terminal:

     ```
     cdk --version
     ```

5. **Postman**
   - Postman is a very useful free application. By using it, you can send requests to the applications developed in this workshop, whether they are running on your computer or deployed on your AWS account.
   - Download Postman based on the [instructions](https://www.postman.com/).

6. **IntelliJ IDEA Community Edition**
   - The IDE that will be used is IntelliJ IDEA Community Edition, from JetBrains. This is one of the most modern IDEs for development in Java and other programming languages.
   - Download IntelliJ IDEA Community Edition from [this link](https://www.jetbrains.com/idea/download/?section=mac).

7. **Docker Desktop**
   - Docker Desktop will be used to create Docker images of all applications to be built in this workshop before uploading the image to AWS.
   - Download Docker via [this link](https://www.docker.com/products/docker-desktop/).
