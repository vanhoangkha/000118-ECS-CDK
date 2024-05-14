---
title : "Tạo X-Ray inspector"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 9.2 </b> "
---


#### Tạo X-Ray inspector

1. Trước tiên để đo lường các phương thức ta cần thêm Annotation **@XRayEnabled** ở file **ProductRepository** và **ProductsController**

![Architect](/images/9/createInspect/01.png/?featherlight=false&width=60pc)

![Architect](/images/9/createInspect/011.png/?featherlight=false&width=60pc)

2. Trong thư mục config tạo một file mới có tên là **XRayInspector**

![Architect](/images/9/createInspect/02.png/?featherlight=false&width=60pc)

3. Thêm 2 annotation vào class XRayInspector với
+ **@Aspect**: Đây là một annotation của Spring AOP. Nó chỉ ra rằng lớp này sẽ chứa logic cross-cutting, nghĩa là các hành vi (behavior) mà có thể ảnh hưởng hoặc được áp dụng trên nhiều lớp hoặc phương thức khác nhau. Trong trường hợp này, đó là việc thêm các thông tin giám sát từ AWS X-Ray
+ **@Component**: Annotation này của Spring Framework đánh dấu lớp như một "component", cho phép Spring tự động phát hiện và quản lý nó như một bean trong ApplicationContext.

![Architect](/images/9/createInspect/03.png/?featherlight=false&width=60pc)

4. cho **Class XRayInspector** kế thừa từ BaseAbstractXRayInterceptor, một lớp trừu tượng được sử dụng để tạo ra các interceptor cho AWS X-Ray, nơi các phương thức có thể được ghi đè để tùy chỉnh hành vi của các subsegment trong AWS X-Ray.

```java
@Aspect
@Component
public class XRayInspector extends BaseAbstractXRayInterceptor {
    @Override
    protected void xrayEnabledClasses() {

    }
}
```
![Architect](/images/9/createInspect/04.png/?featherlight=false&width=60pc)


5. Tạo phương thức Method generateMetadata(). Phương thức này được ghi đè từ lớp cha (BaseAbstractXRayInterceptor). Nó nhằm mục đích tạo ra metadata cho một subsegment của AWS X-Ray dựa trên điểm nối (join point) đang được tiến hành. ProceedingJoinPoint là một khái niệm trong AOP đại diện cho một điểm thực thi mà tại đó có thể xảy ra một quá trình chèn (interception) và Subsegment là một phần của trace trong AWS X-Ray, cho phép thu thập thêm thông tin. Trong trường hợp này, phương thức chỉ đơn giản gọi phương thức của lớp cha bằng super.generateMetadata().

```java
@Override
    protected Map<String, Map<String, Object>> generateMetadata(
            ProceedingJoinPoint proceedingJoinPoint, Subsegment subsegment
    ) {
        return super.generateMetadata(proceedingJoinPoint, subsegment);
    }

```

![Architect](/images/9/createInspect/05.png/?featherlight=false&width=60pc)

6. Phương thức xrayEnabledClasses() là một phương thức được đánh dấu với annotation @Pointcut. Annotation này chỉ định một biểu thức pointcut trong Spring AOP, mô tả tại đâu một advice (lời khuyên) nên được áp dụng. Trong trường hợp này, biểu thức pointcut @within(com.amazonaws.xray.spring.aop.XRayEnabled) chỉ ra rằng advice sẽ được áp dụng cho bất kỳ lớp nào được đánh dấu với annotation @XRayEnabled do com.amazonaws.xray.spring.aop cung cấp. Phương thức không có nội dung bên trong, vì nó chỉ dùng để xác định vị trí áp dụng advice

```java
@Override
    @Pointcut("@within(com.amazonaws.xray.spring.aop.XRayEnabled)")
    protected void xrayEnabledClasses() {}
```

![Architect](/images/9/createInspect/06.png/?featherlight=false&width=60pc)

#### Thêm X-Ray interceptor vào DynamoDB Client

7. Mở file **DynamoDBConfig** ta thêm interceptor vào dynamoDbAsyncClient như sau

```java
.overrideConfiguration(ClientOverrideConfiguration.builder()
                        .addExecutionInterceptor(new TracingInterceptor())
                        .build())
```
![Architect](/images/9/createInspect/07.png/?featherlight=false&width=60pc)

8. Cuối cùng thêm **@EnableAspectJAutoProxy** vào **ProductsserviceApplication**

![Architect](/images/9/createInspect/08.png/?featherlight=false&width=60pc)
