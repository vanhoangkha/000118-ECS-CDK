---
title: " Create First CloudFormation Stack"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b>3.1</b>"
---

#### Prepare for CDK Project

1. Delete the **Fcj2024CdkStack** file that was previously created.

![Architect](/images/3/createfirststack/01.png?featherlight=false&width=60pc)

2. Remove unnecessary comments from the file.

![Architect](/images/3/createfirststack/02.png?featherlight=false&width=60pc)

3. Create a new stack by creating a new Java file named **EcrStack.java**.

![Architect](/images/3/createfirststack/03.png?featherlight=false&width=60pc)

{{% notice tip %}}
Using CDK, you can write code to create stacks and CloudFormation resources. Your code will be compiled into corresponding CloudFormation templates, which can then be deployed by CloudFormation as usual.
{{% /notice %}}

4. Once the file is created, have the `EcrStack` class inherit from the `Stack` class provided by the AWS CDK library.

![Architect](/images/3/createfirststack/04.png?featherlight=false&width=60pc)

5. Create the stack constructor.

![Architect](/images/3/createfirststack/05.png?featherlight=false&width=60pc)

6. Create an AWS resource through a repository:
   - First, declare the **productsServiceRepository** property with the following line:
  
     ```java
     private final Repository productsServiceRepository;
     ```
  
   - Then initialize the **productsServiceRepository** property in the constructor:
  
     ```java
     this.productsServiceRepository = new Repository(this, "ProductsService", RepositoryProps.builder().build());
     ```

![Architect](/images/3/createfirststack/06.png?featherlight=false&width=60pc)

7. To initialize the **Repository**, add the following parameters:
   - **this**: Refers to the current class object (usually a CDK stack).
   - **"ProductsService"**: This is the unique name (ID) of the resource being created. This name must be unique within the scope of a CDK stack.
   - Then create the ECR Repository properties using **RepositoryProps.builder().build()**:
     - `builder()` is a static method of the `RepositoryProps` class to initialize a `Builder` object. This is a preparation step to set properties for `RepositoryProps`.
     - The `build()` method on the `Builder` object is called to finalize the construction process and return a complete `RepositoryProps` object with the set properties.

![Architect](/images/3/createfirststack/07.png?featherlight=false&width=60pc)

8. Specify the ECR repository **name** by adding ```.repositoryName("productsservice")``` before `build()`.

![Architect](/images/3/createfirststack/08.png?featherlight=false&width=60pc)

9. Add **.removalPolicy(RemovalPolicy.DESTROY)** to ensure that when the CloudFormation stack is deleted, our resources will also be deleted.

![Architect](/images/3/createfirststack/09.png?featherlight=false&width=60pc)

10. Allow overriding of images in the ECR repository by enabling **IMMUTABLE** with **imageTagMutability(TagMutability.IMMUTABLE)**.

![Architect](/images/3/createfirststack/10.png?featherlight=false&width=60pc)

11. Enable automatic image deletion when the ECR is deleted by adding **autoDeleteImages(true)**.

![Architect](/images/3/createfirststack/11.png?featherlight=false&width=60pc)

12. Create a getter method:
    ```java
    public Repository getProductsServiceRepository() {
        return productsServiceRepository;
    }
    ```

![Architect](/images/3/createfirststack/12.png?featherlight=false&width=60pc)
