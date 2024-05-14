---
title : "Chuẩn bị Productsservice Spring Boot để sử dụng AWS X-Ray"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 9.1 </b> "
---

#### Chuẩn bị Productsservice Spring Boot để sử dụng AWS X-Ray

1. Trong **productsservice** project. Mở file **build.grade** thêm x-ray sdk như sau

```
implementation('com.amazonaws:aws-xray-recorder-sdk-spring:2.14.0')
implementation('com.amazonaws:aws-xray-recorder-sdk-aws-sdk-v2:2.14.0')
```

![Architect](/images/9/repare/01.png/?featherlight=false&width=60pc)

2. Trong thư mục **config** tạo một file mới tên là **XRayConfig.java**

![Architect](/images/9/repare/02.png/?featherlight=false&width=60pc)


3. Thêm Annotation @Configuration là một annotation của Spring, cho biết lớp này sử dụng để định nghĩa các bean cho context của Spring. Tiếp đó, tạo một đối tượng logger được tạo ra để ghi lại các thông tin liên quan đến hoạt động của AWS X-Ray trong ứng dụng.

```java
@Configuration
public class XRayConfig {
    private static final Logger LOG = LoggerFactory.getLogger(XRayConfig.class);
}
```

![Architect](/images/9/repare/03.png/?featherlight=false&width=60pc)

4. Tạo **Constructor XRayConfig()**. Trong constructor này, cấu hình cho AWS X-Ray được thiết lập như sau
+ **ruleFile**: Đọc file cấu hình xray-sampling-rules.json từ resources. File này chứa các quy tắc lấy mẫu cho AWS X-Ray, quy định cách AWS X-Ray nên thu thập dữ liệu.
+ **AWSXRayRecorder** Tạo một thể hiện của AWSXRayRecorder với cấu hình mặc định, bao gồm các plugin mặc định và chiến lược lấy mẫu từ file được chỉ định.
+ **AWSXRay.setGlobalRecorder(awsxRayRecorder)**: Thiết lập AWSXRayRecorder vừa tạo làm bộ ghi toàn cục cho AWS X-Ray.
+ **Trapping exceptions**: Trong trường hợp không tìm thấy file cấu hình, một exception FileNotFoundException sẽ được bắt và thông báo lỗi sẽ được ghi lại bằng logger.


```java
public XRayConfig() {
        try {
            URL ruleFile = ResourceUtils
                    .getURL("classpath:xray/xray-sampling-rules.json");
            
            AWSXRayRecorder awsxRayRecorder = AWSXRayRecorderBuilder.standard()
                    .withDefaultPlugins()
                    .withSamplingStrategy(new CentralizedSamplingStrategy(ruleFile))
                    .build();

            AWSXRay.setGlobalRecorder(awsxRayRecorder);            
        } catch (FileNotFoundException e) {
            LOG.error("XRay config file not found");
        }
    }
```

![Architect](/images/9/repare/04.png/?featherlight=false&width=60pc)

5. Tạo Phương thực **Bean TracingFilter**. Phương thức này định nghĩa một bean kiểu Filter sử dụng AWSXRayServletFilter

+ **AWSXRayServletFilter**: Là một filter cho servlet, được sử dụng để theo dõi các yêu cầu HTTP đến và đi từ ứng dụng. "productsservice" là tên dịch vụ được sử dụng trong báo cáo và phân tích của AWS X-Ray.

```java
@Bean
public Filter TracingFilter() {
    return new AWSXRayServletFilter("productsservice");
}
```

![Architect](/images/9/repare/05.png/?featherlight=false&width=60pc)

6. Cuối cùng, tạo một **xray-sampling-rules.json** đã khai báo trước đó. Vào thự mục **resource** tạo một thư mục mới tên **xray** và tạo một file json tên là **xray-sampling-rules.json**

![Architect](/images/9/repare/06.png/?featherlight=false&width=60pc)

7. Cấu hình file **xray-sampling-rules.json** như sau

```js
{
  "version": 2,
  "default":
  {
    "fixed_target": 0,
    "rate": 1
  },
  "rules": [
    {
      "fixed_target": 0,
      "rate": 0,
      "url_path": "/actuator/health",
      "http_method": "GET",
      "host": "*",
      "description": "Load balancer health check"
    }
  ]
}
```
![Architect](/images/9/repare/07.png/?featherlight=false&width=60pc)
