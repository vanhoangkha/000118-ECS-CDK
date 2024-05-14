---
title: " Preparing Productsservice Spring Boot for AWS X-Ray Usage"
date: "`r Sys.Date()`"
weight: 1
chapter: false
pre: "<b>9.1</b>"
---

#### Preparing Productsservice Spring Boot for AWS X-Ray Usage

1. In the **productsservice** project, open the **build.gradle** file and add the X-Ray SDK dependencies as follows:

   ```groovy
   implementation 'com.amazonaws:aws-xray-recorder-sdk-spring:2.14.0'
   implementation 'com.amazonaws:aws-xray-recorder-sdk-aws-sdk-v2:2.14.0'

   ```

![Architect](/images/9/repare/01.png/?featherlight=false&width=60pc)

#### Preparing Productsservice Spring Boot for AWS X-Ray Usage

2. Inside the **config** directory, create a new file named **XRayConfig.java**.

  ![Architect](/images/9/repare/02.png/?featherlight=false&width=60pc)

3. Add the **@Configuration** annotation to the **XRayConfig** class. This annotation in Spring indicates that this class is used to define beans for the Spring application context. Next, create a logger object to record information related to AWS X-Ray operations in the application.

```java
   @Configuration
   public class XRayConfig {
       private static final Logger LOG = LoggerFactory.getLogger(XRayConfig.class);
   }
```

![Architect](/images/9/repare/03.png/?featherlight=false&width=60pc)

4. Create a constructor **XRayConfig()** within the **XRayConfig** class. In this constructor, configure AWS X-Ray as follows:

   - **ruleFile**: Read the configuration file **xray-sampling-rules.json** from the resources. This file contains sampling rules for AWS X-Ray, defining how AWS X-Ray should collect data.
   - **AWSXRayRecorder**: Create an instance of **AWSXRayRecorder** with default configuration, including default plugins and sampling strategy from the specified file.
   - **AWSXRay.setGlobalRecorder(awsxRayRecorder)**: Set the newly created **AWSXRayRecorder** as the global recorder for AWS X-Ray.
   - **Trapping exceptions**: In case the configuration file is not found, catch a **FileNotFoundException** and log an error message using the logger.

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

5. Create the method **Bean TracingFilter**. This method defines a bean of type **Filter** using **AWSXRayServletFilter**.

   - **AWSXRayServletFilter**: This filter for servlets is used to trace HTTP requests coming into and leaving the application. "productsservice" is the service name used in AWS X-Ray reporting and analysis.

```java
   @Bean
   public Filter tracingFilter() {
       return new AWSXRayServletFilter("productsservice");
   }

```

![Architect](/images/9/repare/05.png/?featherlight=false&width=60pc)

6. Finally, create an **xray-sampling-rules.json** file as described earlier. Navigate to the **resources** directory, create a new folder named **xray**, and then create a JSON file named **xray-sampling-rules.json**.

   ![Architect](/images/9/repare/06.png/?featherlight=false&width=60pc)

7. Configure the **xray-sampling-rules.json** file as follows:

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
