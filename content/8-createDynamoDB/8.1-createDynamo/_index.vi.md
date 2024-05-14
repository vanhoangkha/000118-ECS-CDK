---
title : "Tạo DynamoDB với CDK"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 8.1 </b> "
---

#### Tạo DynamoDB với CDK

1. Trước tiên ta mở file **ProductsServiceStack.java** trong constuctor khởi tạo đối tượng **Table** với ID là **``ProductsDdb``**

```java
Table productsDdb = new Table(this, "ProductsDdb",
                TableProps.builder()
                        .build());
```
![Architect](/images/8/createDynamoDB/01.png?featherlight=false&width=60pc)

2. Xác định khóa phân vùng (partition key) của table. Trong workshop này, ta sẽ sử dụng thuộc tính id  làm partition key với kiểu dữ liệu là STRING.

```java
    .partitionKey(Attribute.builder()
    .name("id")
    .type(AttributeType.STRING)
    .build())
```
![Architect](/images/8/createDynamoDB/02.png?featherlight=false&width=60pc)

3. Đặt tên cho DynamoDB table là **``products``**.

```java
.tableName("products")
```

![Architect](/images/8/createDynamoDB/03.png?featherlight=false&width=60pc)

4. Tiếp theo ta cấu hình xoá table khi tài nguyên CDK stack của bạn bị xóa

```java
.removalPolicy(RemovalPolicy.DESTROY)
```
![Architect](/images/8/createDynamoDB/04.png?featherlight=false&width=60pc)

5. Tiếp theo chúng ta cần xác định chế độ thanh toán (billing mode) của table. Ở đây, chế độ PROVISIONED được sử dụng, nghĩa là bạn cần chỉ định đủ khả năng đọc và ghi (read và write capacity) cho table.

```java
.billingMode(BillingMode.PROVISIONED)
```
![Architect](/images/8/createDynamoDB/05.png?featherlight=false&width=60pc)

6. Cuối cùng,ta đặt capacity units (đơn vị khả năng) cho table. Trong trường hợp này, đặt read capacity và write capacity là 1, nghĩa là table sẽ có đủ tài nguyên để đọc và ghi ở mức rất thấp.

```java
.readCapacity(1)
.writeCapacity(1)
```
![Architect](/images/8/createDynamoDB/06.png?featherlight=false&width=60pc)

#### Cấp quyền để product service có thể truy cập vào dynamoDB

7. Tiếp theo, ta hiện việc cấp quyền cho vai trò của task definition trong Fargate để có thể đọc và ghi dữ liệu vào DynamoDB table productsDdb. Điều này đảm bảo rằng các task trong Fargate có thể tương tác với DynamoDB table này một cách an toàn và theo quy định của IAM (Identity and Access Management) trong AWS.

```java
productsDdb.grantReadWriteData(fargateTaskDefinition.getTaskRole());
```
![Architect](/images/8/createDynamoDB/07.png?featherlight=false&width=60pc)

8. Thêm 2 biến mối trường là table name và region cho task definition

```java
envVariables.put("AWS_PRODUCTSDDB_NAME", productsDdb.getTableName());
envVariables.put("AWS_REGION", this.getRegion());
```

![Architect](/images/8/createDynamoDB/08.png?featherlight=false&width=60pc)
