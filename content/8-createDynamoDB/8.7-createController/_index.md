---
title: " Create Product Repository"
date: "`r Sys.Date()`"
weight: 7
chapter: false
pre: "<b>8.7</b>"
---

#### Create Product Repository

1. Inside the `products` directory, create a new directory named **`repositories`**. Then, create a new file named **`ProductsRepository.java`**.

   ![Architect](/images/8/createRepositories/01.png?featherlight=false&width=60pc)

2. Use `@Repository` to annotate the `ProductsRepository` class as a Spring repository bean for managing data access.

   ![Architect](/images/8/createRepositories/02.png?featherlight=false&width=60pc)

3. Add two fields to the `ProductsRepository` class:

   + **private final DynamoDbEnhancedAsyncClient dynamoDbEnhancedAsyncClient;**: This is a final field to store an instance of **DynamoDbEnhancedAsyncClient**, used for asynchronous interaction with DynamoDB.

   +  **private DynamoDbAsyncTable<Product> productsTable;**: This is a field to store a DynamoDB table for `Product` objects in an asynchronous manner.


```java
private final DynamoDbEnhancedAsyncClient dynamoDbEnhancedAsyncClient;

private DynamoDbAsyncTable<Product> productsTable;
```

![Architect](/images/8/createRepositories/03.png?featherlight=false&width=60pc)

4. Create a constructor in the ProductsRepository class with the following annotations and parameters:

   + Add **@Autowired**, which is a Spring annotation used to automatically inject dependencies into the `ProductsRepository` class constructor.

   + Create a reference **DynamoDbEnhancedAsyncClient dynamoDbEnhancedAsyncClient** that Spring will inject into the constructor. This object will be used to interact with DynamoDB.

   + Create a reference **@Value("${aws.productsddb.name}") String productsDdbName** annotated with Spring's **@Value** annotation to inject the value of a property from the **application.properties** configuration file. In this case, it injects the value of the property named "aws.productsddb.name" into the **productsDdbName** variable. This property represents the name of the DynamoDB table that **ProductsRepository** will interact with.

```java
    @Autowired
    public ProductsRepository(
            DynamoDbEnhancedAsyncClient dynamoDbEnhancedAsyncClient,
        @Value("${aws.productsddb.name}") String productsDdbName) {}
```
![Architect](/images/8/createRepositories/04.png?featherlight=false&width=60pc)

5. Initialize the **dynamoDbEnhancedAsyncClient** field in the ProductsRepository constructor to assign the value of **dynamoDbEnhancedAsyncClient** injected by Spring to the instance variable **this.dynamoDbEnhancedAsyncClient**.

=
```java
this.dynamoDbEnhancedAsyncClient = dynamoDbEnhancedAsyncClient;
```

![Architect](/images/8/createRepositories/05.png?featherlight=false&width=60pc)

6. Initialize the **productsTable** field using the dynamoDbEnhancedAsyncClient to create the productsTable. The table()`method of dynamoDbEnhancedAsyncClient is called with two arguments:

   + **productsDdbName**: This is the name of the DynamoDB table that ProductsRepository will interact with.

   + **TableSchema.fromBean(Product.class)**: This argument creates a schema for the DynamoDB table based on the structure of the `Product` class. The schema defines how DynamoDB stores and reads data from Product objects.

```java
   this.productsTable = dynamoDbEnhancedAsyncClient.table(productsDdbName, TableSchema.fromBean(Product.class));

```

![Architect](/images/8/createRepositories/06.png?featherlight=false&width=60pc)

7. Next, create a method to retrieve all items from the DynamoDB table. Use `productsTable.scan()` to perform a scan operation on the DynamoDB table, which returns all items in the table.


```java
public PagePublisher<Product> getAll() {
        //DO NOT DO THIS PRODUCTION
        return productsTable.scan();
    }
```

{{% notice warning %}}
Avoid using scan() in a production environment because scanning an entire DynamoDB table is not recommended in production due to potential performance and cost implications. Scanning the entire table can degrade system performance, especially for large tables.
{{% /notice %}}


![Architect](/images/8/createRepositories/07.png?featherlight=false&width=60pc)

8. Tạo phương thức **getById** nhận vào một **productId** và trả về một CompletableFuture<Product>, đại diện cho kết quả của việc lấy một mục Product từ bảng DynamoDB dựa trên productId.

+ Với **productsTable.getItem()** là một hoạt động lấy mục (get item) từ bảng DynamoDB.
+ Với **Key.builder().partitionValue(productId).build()** xây dựng một Key với giá trị partition key (productId) để truy xuất mục tương ứng từ bảng.

8. Create a method **getById** that takes a **productId** as input and returns a **CompletableFuture<Product>**, representing the result of retrieving a Product item from the DynamoDB table based on the productId.

   + Use **productsTable.getItem()** to perform a get item operation on the DynamoDB table.

   + Use **Key.builder().partitionValue(productId).build()** to construct a `Key` with the partition key value (`productId`) to access the corresponding item from the table.

```java
public CompletableFuture<Product> getById(String productId) {
        return productsTable.getItem(Key.builder()
                        .partitionValue(productId)
                .build());
    }
```

![Architect](/images/8/createRepositories/08.png?featherlight=false&width=60pc)

9. Tạo phương thức **create**,phương thức này nhận vào một đối tượng **Product** và trả về một **CompletableFuture<Void>**, đại diện cho kết quả của việc tạo mới một mục Product trong bảng DynamoDB.
 + với **productsTable.putItem(product)** là một hoạt động thêm mục (put item) vào bảng DynamoDB. Phương thức này sẽ đưa product vào bảng và trả về một **CompletableFuture<Void>** để xử lý kết quả một cách bất đồng bộ.

9. Create a **create** method in the ProductsRepository class that takes a **Product** object as input and returns a **CompletableFuture<Void>** representing the result of creating a new Product item in the DynamoDB table.

   + Use **productsTable.putItem(product)** to perform a put item operation on the DynamoDB table. This method adds the product to the table and returns a **CompletableFuture<Void>** to handle the result asynchronously.

```java
public CompletableFuture<Void> create(Product product) {
        return productsTable.putItem(product);
    }
```
![Architect](/images/8/createRepositories/09.png?featherlight=false&width=60pc)


10.  Create a  **deleteById** method in the **ProductsRepository** class that takes a **productId** as input and returns a **CompletableFuture<Product>** representing the result of deleting a Product item from the DynamoDB table based on the productId.

   + Use **productsTable.deleteItem()** to perform a delete item operation on the DynamoDB table. This method deletes the item identified by the specified productId from the table.

   + Use **Key.builder().partitionValue(productId).build()** to construct a Key with the partition key value (`productId`) to identify the item to be deleted from the table.


```java
public CompletableFuture<Product> deleteById(String productId) {
        return productsTable.deleteItem(Key.builder()
                .partitionValue(productId)
                .build());
    }
```
![Architect](/images/8/createRepositories/10.png?featherlight=false&width=60pc)

11. Create an **update** method in the ProductsRepository class to update a Product item in the DynamoDB table.

   + Use **product.setId(productId)** to set the ID of the product to the provided productId before performing the update.

   + Use **productsTable.updateItem()** with **UpdateItemEnhancedRequest.builder(Product.class).item(product)** to build an update request for the product data.

   + Specify the update condition using **conditionExpression()** to ensure that the id attribute of the product must exist before the update can be performed.

   + The method returns a **CompletableFuture<Product>** to handle the result of the update operation asynchronously.


```java
public CompletableFuture<Product> update(Product product, String productId) {
        product.setId(productId);
        return productsTable.updateItem(
                UpdateItemEnhancedRequest.builder(Product.class)
                        .item(product)
                        .conditionExpression(Expression.builder()
                                .expression("attribute_exists(id)")
                                .build())
                        .build()
        );
    }
```

![Architect](/images/8/createRepositories/11.png?featherlight=false&width=60pc)

