---
title : "Tạo product repository"
date :  "`r Sys.Date()`" 
weight : 7
chapter : false
pre : " <b> 8.7 </b> "
---

#### Tạo product repository

1. Trong thư mục products tạo một thư mục mới tên là **repositories**. Sau đó tạo một file mới tên là **ProductsRepository.java**

![Architect](/images/8/createRepositories/01.png?featherlight=false&width=60pc)

2. Dùng **@Repository** để ánh dấu lớp ProductsRepository là một bean repository trong Spring để quản lý truy xuất dữ liệu.

![Architect](/images/8/createRepositories/02.png?featherlight=false&width=60pc)

3. Thêm 2 trường là **DynamoDbEnhancedAsyncClient dynamoDbEnhancedAsyncClientt** và **DynamoDbAsyncTable<Product> productsTable** với:
   + **private final DynamoDbEnhancedAsyncClient dynamoDbEnhancedAsyncClient**: Đây là một trường (field) final để lưu trữ một instance của DynamoDbEnhancedAsyncClient, được sử dụng để tương tác bất đồng bộ với DynamoDB.
   + **private DynamoDbAsyncTable<Product> productsTable**: Đây là một trường (field) để lưu trữ một bảng DynamoDB cho các đối tượng Product dưới dạng bất đồng bộ.

```java
private final DynamoDbEnhancedAsyncClient dynamoDbEnhancedAsyncClient;

private DynamoDbAsyncTable<Product> productsTable;
```

![Architect](/images/8/createRepositories/03.png?featherlight=false&width=60pc)

4. Tạo constructor trong lớp ProductsRepository với
   + Thêm **@Autowired** là một annotation của Spring được sử dụng để tự động inject các dependency vào constructor của lớp ProductsRepository.
   + Tạo một tham chiếu **DynamoDbEnhancedAsyncClient dynamoDbEnhancedAsyncClient** được Spring inject vào constructor. Đối tượng này sẽ được sử dụng để tương tác với DynamoDB.
   + Tạo tham chiếu **@Value("${aws.productsddb.name}") String productsDdbName** là một annotation của Spring để inject giá trị của thuộc tính từ file cấu hình application.properties Trong trường hợp này, nó sẽ inject giá trị của thuộc tính có tên là "aws.productsddb.name" vào biến productsDdbName. Đây là tên của bảng DynamoDB mà ProductsRepository sẽ tương tác.


```java
    @Autowired
    public ProductsRepository(
            DynamoDbEnhancedAsyncClient dynamoDbEnhancedAsyncClient,
        @Value("${aws.productsddb.name}") String productsDdbName) {}
```
![Architect](/images/8/createRepositories/04.png?featherlight=false&width=60pc)

5. Khởi tại **dynamoDbEnhancedAsyncClient** để gán giá trị của dynamoDbEnhancedAsyncClient được inject từ Spring vào biến instance this.dynamoDbEnhancedAsyncClient.

```java
this.dynamoDbEnhancedAsyncClient = dynamoDbEnhancedAsyncClient;
```

![Architect](/images/8/createRepositories/05.png?featherlight=false&width=60pc)

6. Khởi tạo **productsTable** sử dụng dynamoDbEnhancedAsyncClient để khởi tạo productsTable. Phương thức table() của dynamoDbEnhancedAsyncClient được gọi với hai đối số:
   + **productsDdbName**: Đây là tên của bảng DynamoDB mà ProductsRepository sẽ tương tác.
   + **TableSchema.fromBean(Product.class)**: Đối số này tạo ra một schema cho bảng DynamoDB dựa trên cấu trúc của lớp Product. Schema này xác định cách DynamoDB lưu trữ và đọc dữ liệu từ các đối tượng Product.

```java
this.productsTable = dynamoDbEnhancedAsyncClient
                .table(productsDdbName, TableSchema.fromBean(Product.class));
```

![Architect](/images/8/createRepositories/06.png?featherlight=false&width=60pc)

7. Tiếp theo, tạo phương thức để lấy tất cả các mục trong bảng DynamoDB. **productsTable.scan()** là một hoạt động quét (scan) trên bảng DynamoDB, trả về tất cả các mục trong bảng

```java
public PagePublisher<Product> getAll() {
        //DO NOT DO THIS PRODUCTION
        return productsTable.scan();
    }
```


{{% notice warning %}}
Đừng dùng scan trong môi trường production vì việc quét toàn bộ bảng DynamoDB không được khuyến khích trong môi trường sản xuất do tiềm ẩn về hiệu suất và chi phí (cost) của việc này. Quét toàn bộ bảng có thể làm giảm hiệu suất hệ thống đối với các bảng lớn.
{{% /notice %}}

![Architect](/images/8/createRepositories/07.png?featherlight=false&width=60pc)

8. Tạo phương thức **getById** nhận vào một **productId** và trả về một CompletableFuture<Product>, đại diện cho kết quả của việc lấy một mục Product từ bảng DynamoDB dựa trên productId.

+ Với **productsTable.getItem()** là một hoạt động lấy mục (get item) từ bảng DynamoDB.
+ Với **Key.builder().partitionValue(productId).build()** xây dựng một Key với giá trị partition key (productId) để truy xuất mục tương ứng từ bảng.

```java
public CompletableFuture<Product> getById(String productId) {
        return productsTable.getItem(Key.builder()
                        .partitionValue(productId)
                .build());
    }
```

![Architect](/images/8/createRepositories/08.png?featherlight=false&width=60pc)

9. Tạo phương thức **create**,phương thức này nhận vào một đối tượng **Product** và trả về một **CompletableFuture<Void>**, đại diện cho kết quả của việc tạo mới một mục Product trong bảng DynamoDB.
 + Với **productsTable.putItem(product)** là một hoạt động thêm mục (put item) vào bảng DynamoDB. Phương thức này sẽ đưa product vào bảng và trả về một **CompletableFuture<Void>** để xử lý kết quả một cách bất đồng bộ.

```java
public CompletableFuture<Void> create(Product product) {
        return productsTable.putItem(product);
    }
```
![Architect](/images/8/createRepositories/09.png?featherlight=false&width=60pc)

10. Tạo phương thức **deleteById**, phương thức này nhận vào một **productId** và trả về một **CompletableFuture<Product>**, đại diện cho kết quả của việc xóa một mục Product từ bảng DynamoDB dựa trên productId.
+ Với **productsTable.deleteItem()** là một hoạt động xóa mục (delete item) từ bảng DynamoDB
+ Với **Key.builder().partitionValue(productId).build()** xây dựng một Key với giá trị partition key (productId) để xác định mục cần xóa từ bảng.

```java
public CompletableFuture<Product> deleteById(String productId) {
        return productsTable.deleteItem(Key.builder()
                .partitionValue(productId)
                .build());
    }
```
![Architect](/images/8/createRepositories/10.png?featherlight=false&width=60pc)

11. Cuối cùng tạo phương thức **update**, thức này được sử dụng để cập nhật một mục Product trong bảng DynamoDB.
 + Trước khi thực hiện cập nhật, **product.setId(productId)** được sử dụng để đặt lại ID của product thành **productId** được cung cấp.
 + Sử dụng **productsTable.updateItem()**, trong đó **UpdateItemEnhancedRequest.builder(Product.class).item(product)** xây dựng một yêu cầu cập nhật dữ liệu của product.
 + Điều kiện cập nhật được chỉ định bằng cách sử dụng **conditionExpression()**, trong đó chỉ ra rằng thuộc tính id của product phải tồn tại trước khi cập nhật có thể được thực hiện.
 + Phương thức trả về một **CompletableFuture<Product>** để xử lý kết quả cập nhật một cách bất đồng bộ.

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

