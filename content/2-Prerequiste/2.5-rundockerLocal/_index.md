---
title: " Run Docker Image on Local Machine"
date: "`r Sys.Date()`"
weight: 5
chapter: false
pre: "<b>2.5</b>"
---

#### Run Docker Image on Local Machine

1. Right-click on the **productsservice:1.0.0** image we created earlier and select **Create Container**.

![Architect](/images/1/createDocker/14.png?featherlight=false&width=60pc)

2. A **Docker image configuration** popup will appear. Choose **Run** and the terminal will execute.

![Architect](/images/1/runDocker/02.png?featherlight=false&width=60pc)

3. Configure the Docker container:

   - In the **Services** interface, select the **Dashboard** tab and then click **Add** under Ports.

![Architect](/images/1/runDocker/03.png?featherlight=false&width=60pc)

4. A **Port bindings** popup will appear. Click **Modify Options**, then select **Host IP**.

![Architect](/images/1/runDocker/04.png?featherlight=false&width=60pc)

5. Enter **`0.0.0.0`** for the **Host IP**.

![Architect](/images/1/runDocker/05.png?featherlight=false&width=60pc)

6. Similarly, configure **Host Port** as **`8081`** and **Protocol** as **TCP**.

![Architect](/images/1/runDocker/06.png?featherlight=false&width=60pc)

7. For **--publish**, enter **`8081`** and select **Recreate container**.

![Architect](/images/1/runDocker/07.png?featherlight=false&width=60pc)

#### Test Application on Postman

8. Run the project and test on Postman by creating an HTTP **GET** request with the URL **`http://localhost:8081/api/products`**.

![Architect](/images/1/repareIntelli/12.png?featherlight=false&width=60pc)
