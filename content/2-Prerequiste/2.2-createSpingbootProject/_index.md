---
title: " Creating a Spring Boot Project"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b>2.2</b>"
---

#### Creating a Spring Boot Project

1. Visit [start.spring.io](https://start.spring.io/).

   ![Spring Initializr](/images/1/spring/01.png?featherlight=false&width=60pc)

2. Choose the programming language and framework version:
   - For **Project**, select **Gradle - Groovy**.
   - For **Language**, choose **Java**.
   - For **Spring Boot** version, select **3.2.4**.

   ![Project Metadata](/images/1/spring/02.png?featherlight=false&width=60pc)

3. Configure Project Metadata:
   - Set **Group** to **`com.firstcloudjourney`**.
   - Set **Artifact** to **`productsservice`**.
   - Other fields will be automatically filled.
   - Choose Java version as **21**.

   ![Project Metadata Configuration](/images/1/spring/03.png?featherlight=false&width=60pc)

4. Add Dependencies:
   - Click **ADD DEPENDENCIES** to open a pop-up.
   - Add **Spring Web** and **Spring Boot Actuator** packages.

   ![Add Dependencies](/images/1/spring/04.png?featherlight=false&width=60pc)

5. Generate the Spring Boot Project:
   - Click **GENERATE** to download the created project.

   ![Generate Project](/images/1/spring/05.png?featherlight=false&width=60pc)
