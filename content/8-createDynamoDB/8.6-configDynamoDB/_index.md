---
title: " Configure DynamoDB"
date: "`r Sys.Date()`"
weight: 6
chapter: false
pre: "<b>8.6</b>"
---

#### Add AWS SDK Dependencies to ProductsService Project

1. Open the `build.gradle` file and add AWS SDK dependencies as follows:

```java
implementation(platform("software.amazon.awssdk:bom:2.21.15"))
implementation("software.amazon.awssdk:dynamodb")
implementation("software.amazon.awssdk:dynamodb-enhanced")
```
![Architect](/images/8/configDynamo/01.png?featherlight=false&width=60pc)

#### Create a Products Model to Represent it in the New DynamoDB Table

2. Inside the `products` directory, create a new directory named **`Models`** and then create a new Java file named **`Product.java`**.

   ![Architect](/images/8/configDynamo/02.png?featherlight=false&width=60pc)

3. Define the model using the following code snippet:

```java
@DynamoDbBean
public class Product {
    private String id;
    private String productName;
    private String code;
    private float price;
    private String model;
    private String productUrl;

    @DynamoDbPartitionKey
    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getProductName() {
        return productName;
    }

    public void setProductName(String productName) {
        this.productName = productName;
    }

    public String getCode() {
        return code;
    }

    public void setCode(String code) {
        this.code = code;
    }

    public float getPrice() {
        return price;
    }

    public void setPrice(float price) {
        this.price = price;
    }

    public String getModel() {
        return model;
    }

    public void setModel(String model) {
        this.model = model;
    }

    public String getProductUrl() {
        return productUrl;
    }

    public void setProductUrl(String productUrl) {
        this.productUrl = productUrl;
    }
}

```
![Architect](/images/8/configDynamo/03.png?featherlight=false&width=60pc)

#### Tạo DynamoDB config class
   
#### Create DynamoDB Configuration Class

4. Create a new folder named **`config`**, and then create a new file named **`DynamoDBConfig.java`** inside this folder. Add the `@Configuration` annotation from Spring, which specifies that this class contains bean definitions for the Spring container. This class serves as a configuration source.

```java
@Configuration
public class DynamoDBConfig {
}
```

![Architect](/images/8/configDynamo/04.png?featherlight=false&width=60pc)

5. Create an instance variable to store the AWS region information, where its value is injected from the `aws.region` property in the **application.properties** configuration file.

```java
    @Value("${aws.region}")
    private String awsRegion;
```


![Architect](/images/8/configDynamo/05.png?featherlight=false&width=60pc)

6. Open the `application.properties` file to configure the region as `ap-southeast-1` (Singapore). Add the following line to `application.properties`:

```properties
aws.region=ap-southeast-1
```
![Architect](/images/8/configDynamo/06.png?featherlight=false&width=60pc)

7. Return to the **DynamoDBConfig** file and create a method named **dynamoDbAsyncClient** to instantiate and configure a **DynamoDbAsyncClient**, which is an asynchronous client for interacting with AWS DynamoDB.

```java
    @Bean
    @Primary
    public DynamoDbAsyncClient dynamoDbAsyncClient() {
        return DynamoDbAsyncClient.builder()
                .credentialsProvider(DefaultCredentialsProvider.create())
                .region(Region.of(awsRegion))
                .overrideConfiguration(ClientOverrideConfiguration.builder()
                        .addExecutionInterceptor(new TracingInterceptor())
                        .build())
                .build();
    }
```

![Architect](/images/8/configDynamo/07.png?featherlight=false&width=60pc)

8. Add a method **dynamoDbEnhancedAsyncClient** to instantiate and configure a **DynamoDbEnhancedAsyncClient**, which is an enhanced version of the asynchronous client for working with AWS DynamoDB.

```java
    @Bean
    @Primary
    public DynamoDbEnhancedAsyncClient dynamoDbEnhancedAsyncClient() {
        return DynamoDbEnhancedAsyncClient.builder()
                .dynamoDbClient(dynamoDbAsyncClient())
                .build();
    }
```

{{% notice info %}}
The DynamoDbEnhancedAsyncClient provides simpler and more user-friendly APIs compared to the regular DynamoDbAsyncClient, focusing on working with tables and data models in DynamoDB in a more abstracted and detailed way. Using this enhanced client reduces some complexity when interacting with DynamoDB, especially when performing CRUD (Create, Read, Update, Delete) operations on data.
{{% /notice %}}

![Architect](/images/8/configDynamo/08.png?featherlight=false&width=60pc)
