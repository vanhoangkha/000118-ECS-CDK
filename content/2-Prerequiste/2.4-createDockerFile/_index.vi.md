---
title : "Tạo Dockerfile và generate docker image"
date :  "`r Sys.Date()`" 
weight : 4
chapter : false
pre : " <b> 2.4 </b> "
---

#### Tạo task
1. Trước tiên bạn cần phải tạo task trên file **build.grade** bằng cách copy đoạn code sau

```
tasks.register("unpack", Copy) {
	dependsOn bootJar
	from(zipTree(tasks.bootJar.outputs.files.singleFile))
	into("build/libs")
}
```

{{% notice tip %}}
 Đoạn mã này là một đoạn mã sử dụng Gradle để định nghĩa một task mới có tên là "unpack". Hành động của task này là sao chép (copy) các file từ một tệp nén (ZIP) của task "bootJar" và giải nén chúng vào thư mục "build/libs".
{{% /notice %}}

![Architect](/images/1/createDocker/01.png?featherlight=false&width=60pc)


#### Tạo file .dockerignore 
2. Tạo file .dockerignore để chỉ định những file và thư mục mà Docker sẽ bỏ qua (ignore) khi bạn đóng gói (build) một image Docker từ các thành phần của project.

![Architect](/images/1/createDocker/02.png?featherlight=false&width=60pc)

3. Copy đoạn code bên dưới
```
.git
.gitignore
.dockerignore
.gradle
.idea

Dockerfile
README.md
```
![Architect](/images/1/createDocker/03.png?featherlight=false&width=60pc)

#### Tạo file Dockerfile

4. Tạo file Dockerfile và copy đoạn code bên dưới
```
FROM eclipse-temurin:21-jdk-alpine
VOLUME /tmp
ARG DEPENDENCY=build/libs
COPY ${DEPENDENCY}/BOOT-INF/lib /app/lib
COPY ${DEPENDENCY}/META-INF /app/META-INF
COPY ${DEPENDENCY}/BOOT-INF/classes /app
EXPOSE 8081
ENTRYPOINT ["java", "-cp", "app:app/lib/*", "com.firstcloudjourney.productsservice.ProductsserviceApplication"]
```
![Architect](/images/1/createDocker/04.png?featherlight=false&width=60pc)

#### Generate docker image
5. Tiếp theo chúng ta sẽ cấu hình trên intelliJ để generate Docker image
   - Nhấn vào biểu tượng **Run**, chọn **new run configuration**

![Architect](/images/1/createDocker/05.png?featherlight=false&width=60pc)

6. Một popup hiện lên chúng ta sẽ cấu hình như sau
- Với **Image Tag**, nhập **```productsservice:1.0.0```**
- Nhấn **Modify Opition** chọn **Build options**
- Khi **Build options** hiên ra, nhập **```--platform linux/amd64```**

![Architect](/images/1/createDocker/06.png?featherlight=false&width=60pc)

7. Tiếp theo chúng ta cần cấu hình **before launch** 
- Nhấn vào icon **+** và chọn **run gradle task**
- Sau đó 1 pop up hiên lên như bên dưới

![Architect](/images/1/createDocker/07.png?featherlight=false&width=60pc)

8. Trong pop up **select gradle task**
   - Với **Gradle project** chọn **productsservice**
   - Với **Task** chọn **unpack** task chúng ta đã thêm trước đó
   - Chọn **Ok**

![Architect](/images/1/createDocker/08.png?featherlight=false&width=60pc)

9. Sau khi trở lại pop up **edit configuration** nhấn **apply**

![Architect](/images/1/createDocker/09.png?featherlight=false&width=60pc)

#### build ứng dụng sử dụng bootjar
10. Tiếp theo chúng ta sẽ build ứng dụng bằng cách sử dụng bootjar từ gradle
- Chọn biểu tượng **gradle** 
- Chọn **task** theo sau đó là **build** và nhấn **bootjar** để build ứng dụng

![Architect](/images/1/createDocker/10.png?featherlight=false&width=60pc)

11. Kiểm tra file ứng dụng chúng ta đã buid theo đường dẫn build/libs

![Architect](/images/1/createDocker/11.png?featherlight=false&width=60pc)

#### Gennerate docker image

12. Bây giờ chúng ta sẽ tạo Docker image từ ứng dụng chúng ta đã build
 - Nhấn vào biểu tượng Run ở DockerFile và chọn **Build image for 'DockerFile'**

{{% notice tip %}}
  Hãy đảm bảo bạn đã mở docker desktop để có thể build image thành công
{{% /notice %}}

![Architect](/images/1/createDocker/12.png?featherlight=false&width=60pc)

13. Sau khi tạo image thành công chúng ta có thể thấy **productsservice:1.0.0** đã được tạo ra

![Architect](/images/1/createDocker/13.png?featherlight=false&width=60pc)

