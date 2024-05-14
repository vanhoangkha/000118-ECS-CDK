---
title: " Create DynamoDB with CDK"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b>8.1</b>"
---

#### Create DynamoDB with CDK

1. First, open the file **ProductsServiceStack.java**. In the constructor, initialize a **Table** object with ID **`ProductsDdb`**.

```java
   Table productsDdb = new Table(this, "ProductsDdb",
                   TableProps.builder()
                           .build());
```
![Architect](/images/8/createDynamoDB/01.png?featherlight=false&width=60pc)

2. Define the partition key for the table. In this workshop, we will use the attribute **id** as the partition key with a data type of STRING.

```java
   .partitionKey(Attribute.builder()
       .name("id")
       .type(AttributeType.STRING)
       .build())
```
![Architect](/images/8/createDynamoDB/02.png?featherlight=false&width=60pc)

3. Set the name for the DynamoDB table to **`products`**.

```java
.tableName("products")
```

![Architect](/images/8/createDynamoDB/03.png?featherlight=false&width=60pc)

4. Next, configure the table to be deleted when your CDK stack resources are deleted.

```java
   .removalPolicy(RemovalPolicy.DESTROY)
```
![Architect](/images/8/createDynamoDB/04.png?featherlight=false&width=60pc)

5. Next, we need to specify the billing mode for the table. Here, the **PROVISIONED** mode is used, meaning you need to specify the read and write capacity units (RCUs and WCUs) for the table.

```java
.billingMode(BillingMode.PROVISIONED)
```
![Architect](/images/8/createDynamoDB/05.png?featherlight=false&width=60pc)

6. Finally, we set the capacity units for the table. In this case, we set the read capacity and write capacity to 1, meaning the table will have minimal resources for read and write operations.
```java
   .readCapacity(1)
   .writeCapacity(1)

```
![Architect](/images/8/createDynamoDB/06.png?featherlight=false&width=60pc)

#### Grant Permissions for ProductService to Access DynamoDB

7. Next, we grant permissions to the role of the task definition in Fargate to read and write data into the DynamoDB table `productsDdb`. This ensures that tasks in Fargate can interact with this DynamoDB table safely and according to the IAM (Identity and Access Management) policies in AWS.

```java
productsDdb.grantReadWriteData(fargateTaskDefinition.getTaskRole());
```
![Architect](/images/8/createDynamoDB/07.png?featherlight=false&width=60pc)

8. Add two environment variables (`AWS_PRODUCTSDDB_NAME` and `AWS_REGION`) to the task definition.

```java
envVariables.put("AWS_PRODUCTSDDB_NAME", productsDdb.getTableName());
envVariables.put("AWS_REGION", this.getRegion());
```

![Architect](/images/8/createDynamoDB/08.png?featherlight=false&width=60pc)
