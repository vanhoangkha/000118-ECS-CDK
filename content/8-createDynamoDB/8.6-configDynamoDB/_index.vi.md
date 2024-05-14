---
title : "Cấu hình DynamoDB"
date :  "`r Sys.Date()`" 
weight : 6
chapter : false
pre : " <b> 8.6 </b> "
---

#### Thêm dependencies SDK AWS vào project ProductsService

1. Mở file build.gradle và thêm dependencies SDK AWS như sau

```java
implementation(platform("software.amazon.awssdk:bom:2.21.15"))
implementation("software.amazon.awssdk:dynamodb")
implementation("software.amazon.awssdk:dynamodb-enhanced")
```
![Architect](/images/8/configDynamo/01.png?featherlight=false&width=60pc)

#### Tạo products model để hiển thị nó trong bảng DynomoDB mới

2. Trong thư mục **products** tạo một thư mục mới tên là **Models** và sau đó tạo file java mới tên **Product.java**

![Architect](/images/8/configDynamo/02.png?featherlight=false&width=60pc)

3. Tạo model với đoạn code bên dưới

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
   
4. Tạo một folder mới tên **config** sau đó tạo một file mới tên là **DynamoDBConfig.java** và thêm **@Configuration** một annotation của Spring, dùng để chỉ định rằng class này chứa các bean definition cho container của Spring. Class này được xem như một nguồn cấu hình.

```java
@Configuration
public class DynamoDBConfig {
}
```

![Architect](/images/8/configDynamo/04.png?featherlight=false&width=60pc)

5. Tạo Một biến instance để lưu trữ thông tin vùng (region) của AWS, giá trị của nó được inject vào từ property aws.region trong file cấu hình application.properties 

```java
    @Value("${aws.region}")
    private String awsRegion;
```

![Architect](/images/8/configDynamo/05.png?featherlight=false&width=60pc)

6. Mở file application.properties để cấu hình khu vực là singgapore

```java
aws.region=ap-southeast-1
```
![Architect](/images/8/configDynamo/06.png?featherlight=false&width=60pc)

7. Quay trở lại file **DynamoDBConfig** tạo phương thức **dynamoDbAsyncClient** để tạo và cấu hình một DynamoDbAsyncClient, một client không đồng bộ để tương tác với AWS DynamoDB

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

8. Thêm phương thức **dynamoDbEnhancedAsyncClient** để tạo và cấu hình một **DynamoDbEnhancedAsyncClient**, là phiên bản nâng cao của client không đồng bộ để làm việc với AWS DynamoDB.

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
DynamoDbEnhancedAsyncClient cung cấp các API đơn giản hơn và dễ sử dụng hơn so với DynamoDbAsyncClient thông thường, tập trung vào việc làm việc với các bảng và mô hình dữ liệu trong DynamoDB theo cách trừu tượng hơn và chi tiết hơn. Sử dụng client nâng cao này giúp giảm bớt phần nào độ phức tạp khi tương tác với DynamoDB, đặc biệt là khi làm việc với các hoạt động CRUD (Tạo, Đọc, Cập nhật, Xóa) trên dữ liệu.
{{% /notice %}}

![Architect](/images/8/configDynamo/08.png?featherlight=false&width=60pc)
