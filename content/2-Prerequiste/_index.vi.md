---
title : "Các bước chuẩn bị"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2. </b> "
---

#### Các bước chuẩn bị


1.  **Java Development Kit (JDK)**
-  Đầu tiên chúng ta cần cài đặt **Java Development Kit (JDK)**. Trong workshop nảy chúng ta sẽ dùng **JDK 21 LTS**. Bạn có thể tải tại [đường dẫn này](https://www.oracle.com/java/technologies/downloads/#java21)
-  Tiếp theo chúng ta cần cài đặt **maven**. Bạn có thể cài đặt theo [hướng dẫn](https://maven.apache.org/install.html)

1. **NodeJS**
- để cài dặt **AWS CDK CLI** chúng ta cần phải cài đặt NodeJS. Truy cập [đường dẫn](https://nodejs.org/en/download/). Tải về phiên bản LTS mới nhất
  
- Kiểm tra NodeJS đã được cài đặt đúng chưa, thực hiện lệnh sau trong terminal

```bash
 node -v
```

- Lệnh này sẽ show phiên bản NodeJS đã được cài đặt
  
```
 v18.15.0
```

- Kiểm tra phiên bản NPM đã được cài đặt, thực hiện lệnh sau trong terminal

```bash
 npm -v
```

- Command sẽ show phiên bản NPM đã được cài đặt

```
 9.6.2
```

3. **AWS CLI**
- Truy cập [liên kết](https://aws.amazon.com/vi/cli/) này và tải về phiên bản mới nhất
- Sau khi cài đặt hoàn thành mở termial và kiểm tra phiên bản AWS CLI vừa cài đặt, thông qua lênh sau
  
```bash
 aws --v
```

4. **AWS CDK**
- AWS Cloud Development Kit, or AWS CDK sẽ được sử dụng để xây dựng mã chịu trách nhiệm tạo
cơ sở hạ tầng ứng dụng sử dụng dịch vụ AWS.
- Sau khi cài đặt các gói được đề cập ở trên, hãy thực hiện lệnh sau trong cửa sổ terminal trên hệ điều hành của bạn để cài đặt AWS CDK:

```js
 npm install -g aws-cdk
```

  - Kiểm tra CDK đã được cài đặt đúng chưa, thực hiện lệnh sau trong terminal

```
 cdk --version
```

5. **Postman**
- Postman là một ứng dụng miễn phí rất hữu ích. Bằng cách sử dụng nó, bạn có thể gửi các yêu cầu tới các ứng dụng được phát triển trong workshop này, bất kể chúng có đang chạy trên máy tính của bạn hay đã được triển khai trên tài khoản AWS của bạn.
- Tải Postman dựa vào [chỉ dẫn](https://www.postman.com/) sau

6. **IntelliJ IDEA Community Edition**

- IDE mà sẽ được sử dụng là IntelliJ IDEA Community Edition, từ công ty JetBrains. Đây là một trong những IDE hiện đại nhất để phát triển trong Java và các ngôn ngữ lập trình khác.
- Tải IntelliJ IDEA Community Edition thông qua [đường dẫn](https://www.jetbrains.com/idea/download/?section=mac) này 

7. **Docker Desktop** 
- Docker Desktop sẽ được sử dụng để tạo image Docker của tất cả các ứng dụng sẽ được tạo bằng nó
workshop này trước khi tải image lên AWS
- Tải docker thông qua [đường dẫn](https://www.docker.com/products/docker-desktop/)

