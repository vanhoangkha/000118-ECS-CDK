---
title: " Setting Up IntelliJ IDEA"
date: "`r Sys.Date()`"
weight: 3
chapter: false
pre: "<b>2.3</b>"
---

#### Setting Up IntelliJ IDEA

1. Open IntelliJ IDEA and navigate to the Spring Boot project you just created.

   ![Open Project](/images/1/repareIntelli/01.png?featherlight=false&width=60pc)

2. Configure the project:
   - Right-click on the project and select **Open Module Settings**.

   ![Open Module Settings](/images/1/repareIntelli/02.png?featherlight=false&width=60pc)

3. Under **Project Settings**, ensure that your project has **SDK** set to **21** and **language level** set to **21**.

   ![Project SDK and Language Level](/images/1/repareIntelli/03.png?featherlight=false&width=60pc)

4. Under **Platform Settings**, ensure that your platform **SDK** is set to **21**.

   ![Platform SDK](/images/1/repareIntelli/04.png?featherlight=false&width=60pc)

#### Running the Spring Boot Project

5. Open the file **application.properties** and add **server.port=8081** to configure the project to run on port 8081.

   ![Configure Port](/images/1/repareIntelli/05.png?featherlight=false&width=60pc)

6. Click on the Run icon to run the project. You can see the project running on port 8081.

   ![Run Project](/images/1/repareIntelli/06.png?featherlight=false&width=60pc)

#### Testing the Project with Postman

7. Create an HTTP **GET** request with the URL **`http://localhost:8081/actuator/health`** to test.
   - The response should show **"status": "UP"** indicating that the project is running normally.

   ![Test with Postman](/images/1/repareIntelli/07.png?featherlight=false&width=60pc)

#### Adding the Log4j2 Library

8. Open the **build.gradle** file:
   - In the **dependencies** section, add the Log4j2 library with the following code: 
     ```gradle
     implementation 'org.springframework.boot:spring-boot-starter-log4j2'
     ```
   - Add configurations:
     ```gradle
     configurations {
         configureEach {
             exclude group: 'org.springframework.boot', module: 'spring-boot-starter-logging'
             exclude group: 'commons-logging', module: 'commons-logging'
         }
     }
     ```

   ![Add Log4j2](/images/1/repareIntelli/08.png?featherlight=false&width=60pc)

#### Creating a Controller

9. Inside the **com.firstcloudjourney.productsservice** directory, create a folder named **products**.

   ![Create Products Folder](/images/1/repareIntelli/09.png?featherlight=false&width=60pc)

10. Inside the **products** folder, create a directory named **controllers** and create a file named **ProductsController.java** inside the **controllers** directory.

    ![Create Controller](/images/1/repareIntelli/10.png?featherlight=false&width=60pc)

11. Copy and paste the following code into **ProductsController.java**:

    ```java
    import org.apache.logging.log4j.LogManager;
    import org.apache.logging.log4j.Logger;
    import org.springframework.web.bind.annotation.GetMapping;
    import org.springframework.web.bind.annotation.RequestMapping;
    import org.springframework.web.bind.annotation.RestController;

    @RestController
    @RequestMapping("/api/products")
    public class ProductsController {
        private static final Logger LOG = LogManager.getLogger(ProductsController.class);

        @GetMapping
        public String getAllProducts() {
            LOG.info("Get all products");
            return "All products";
        }
    }
    ```

    ![Products Controller Code](/images/1/repareIntelli/11.png?featherlight=false&width=60pc)

12. Run the project and test with Postman by creating an HTTP **GET** request with the URL **`http://localhost:8081/api/products`**.

    ![Test Controller](/images/1/repareIntelli/12.png?featherlight=false&width=60pc)

13. Check the logs returned in the terminal when making the HTTP GET request.

    ![Terminal Logs](/images/1/repareIntelli/13.png?featherlight=false&width=60pc)
