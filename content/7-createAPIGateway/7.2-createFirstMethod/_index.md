---
title: " Create Product Resource and First Method"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b>7.2</b>"
---

#### Create Product Resource and First Method

1. Create the method **createProductsResource** with two parameters: `RestApi` and `ApiStackProps`, and initialize it within the constructor.

   ```java
   private void createProductsResource(RestApi restApi, ApiStackProps apiStackProps) {
   }

```
![Architect](/images/7/2/01.png?featherlight=false&width=60pc)

2. Create a new resource in API Gateway with the path **/products`. Using `restApi.getRoot().addResource("products")** will create a new sub-resource with the path **/products** from the root of the API (`restApi`).

```java
//products
Resource productsResource = restApi.getRoot().addResource("products");
```
![Architect](/images/7/2/02.png?featherlight=false&width=60pc)

3. Add a GET method to productsResource using the **addMethod** method.

```java
productsResource.addMethod("GET");
```
![Architect](/images/7/2/03.png?featherlight=false&width=60pc)

4. Next, we define an **Integration** to handle GET requests to the endpoint **/products**.

```java
   productsResource.addMethod("GET", new Integration(
                   IntegrationProps.builder()
                           .build()));

```
![Architect](/images/7/2/04.png?featherlight=false&width=60pc)

5. Then, specify the **Integration** type as **HTTP_PROXY**, which means using an HTTP proxy to forward requests, and choose the Integration method type as GET.
```java
   .type(IntegrationType.HTTP_PROXY)
   .integrationHttpMethod("GET")
```

![Architect](/images/7/2/05.png?featherlight=false&width=60pc)

6. Set the URI of the backend that the API Gateway will forward requests to. Here, the DNS name of the Network Load Balancer is used to construct the URL, along with port 8081 and the API path `/api/products` being called.

```java
.uri("http://" + apiStackProps.networkLoadBalancer().getLoadBalancerDnsName() + ":8081/api/products")
```

![Architect](/images/7/2/06.png?featherlight=false&width=60pc)

7. Next, use options to further configure the **IntegrationOptions**.

```java
   .options(IntegrationOptions.builder()
           .build())
```
![Architect](/images/7/2/07.png?featherlight=false&width=60pc)

8. Specify the VpcLink to identify which VPC connection will be used for the Integration method and define the connection type as VPC_LINK, meaning connecting through an AWS VPC.

```java
.vpcLink(apiStackProps.vpcLink())
.connectionType(ConnectionType.VPC_LINK)
```

![Architect](/images/7/2/08.png?featherlight=false&width=60pc)


