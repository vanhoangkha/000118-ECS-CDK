---
title: " Creating X-Ray Inspector"
date: "`r Sys.Date()`"
weight: 2
chapter: false
pre: "<b>9.2</b>"
---

#### Creating X-Ray Inspector

1. First, to measure methods, add the **@XRayEnabled** annotation to the **ProductRepository** and **ProductsController** files.

   ![Architect](/images/9/createInspect/01.png/?featherlight=false&width=60pc)

   ![Architect](/images/9/createInspect/011.png/?featherlight=false&width=60pc)

2. In the **config** directory, create a new file named **XRayInspector**.

   ![Architect](/images/9/createInspect/02.png/?featherlight=false&width=60pc)


3. Add two annotations to the `XRayInspector` class:
   - **@Aspect**: This is a Spring AOP annotation that indicates the class contains cross-cutting concerns, meaning behaviors that can affect or be applied across different classes or methods. In this case, it involves adding monitoring information from AWS X-Ray.
   - **@Component**: This Spring Framework annotation marks the class as a "component," allowing Spring to automatically detect and manage it as a bean in the ApplicationContext.

   ![Architect](/images/9/createInspect/03.png/?featherlight=false&width=60pc)


4. Have the `XRayInspector` class inherit from `BaseAbstractXRayInterceptor`, an abstract class used to create interceptors for AWS X-Ray, where methods can be overridden to customize the behavior of subsegments within AWS X-Ray.

```java
   @Aspect
   @Component
   public class XRayInspector extends BaseAbstractXRayInterceptor {
       @Override
       protected void xrayEnabledClasses() {
           // Implement custom behavior here
       }
   }

```
![Architect](/images/9/createInspect/04.png/?featherlight=false&width=60pc)


5. Create the **generateMetadata()** method. This method overrides the method from the parent class (**BaseAbstractXRayInterceptor**). Its purpose is to generate metadata for a subsegment of AWS X-Ray based on the ongoing join point. **ProceedingJoinPoint** is a concept in AOP representing a point of execution where an interception can occur, and a **Subsegment** is part of a trace in AWS X-Ray, allowing additional information to be collected. In this case, the method simply calls the parent class method using **super.generateMetadata()**.

```java
   @Override
   protected Map<String, Map<String, Object>> generateMetadata(
           ProceedingJoinPoint proceedingJoinPoint, Subsegment subsegment
   ) {
       return super.generateMetadata(proceedingJoinPoint, subsegment);
   }

```

![Architect](/images/9/createInspect/05.png/?featherlight=false&width=60pc)

6. The **xrayEnabledClasses()** method is annotated with **@Pointcut**. This annotation specifies a pointcut expression in Spring AOP, describing where an advice should be applied. In this case, the pointcut expression **@within(com.amazonaws.xray.spring.aop.XRayEnabled)** indicates that the advice will be applied to any class annotated with **@XRayEnabled** provided by **com.amazonaws.xray.spring.aop**. The method itself has no content inside, as it is used solely to define the location where the advice should be applied.

```java
   @Override
   @Pointcut("@within(com.amazonaws.xray.spring.aop.XRayEnabled)")
   protected void xrayEnabledClasses() {}
```
![Architect](/images/9/createInspect/06.png/?featherlight=false&width=60pc)

#### Add X-Ray interceptor to the DynamoDB client

7. To add an X-Ray interceptor to the DynamoDB client, open the `DynamoDBConfig` file and modify the `dynamoDbAsyncClient` configuration as follows:

```java
.overrideConfiguration(ClientOverrideConfiguration.builder()
    .addExecutionInterceptor(new TracingInterceptor())
    .build())

```
![Architect](/images/9/createInspect/07.png/?featherlight=false&width=60pc)

8. Finally, add **@EnableAspectJAutoProxy** to **ProductsserviceApplication**.

   ![Architect](/images/9/createInspect/08.png/?featherlight=false&width=60pc)
