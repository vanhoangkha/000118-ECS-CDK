---
title : "Chạy Docker image trên máy local"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 2.5 </b> "
---

#### Chạy Docker image trên máy local

1.  Nhấp chuột phải vào image  **productsservice:1.0.0** chúng ta đã tạo trước đó và chọn **Create Container**.

![Architect](/images/1/createDocker/14.png?featherlight=false&width=60pc)

2. Pop up **Docker image configuration** hiện lên chúng ta chọn **Run** và terminal sẽ run 

![Architect](/images/1/runDocker/02.png?featherlight=false&width=60pc)

3. Cấu hình docker container

- Trong giao diên **services** chọn tab **Dashboard** và sau đó chọn **add** Ports

![Architect](/images/1/runDocker/03.png?featherlight=false&width=60pc)

4. Popup **Port bindings** hiện lên ta chọn **modify options** chọn **Host IP**

![Architect](/images/1/runDocker/04.png?featherlight=false&width=60pc)

5. Nhập **```0.0.0.0```** vào  **Host IP** 

![Architect](/images/1/runDocker/05.png?featherlight=false&width=60pc)

6. Tương tự như trên lần lượt cấu hình **host port** là **```8081```** và **protocol** là **TCP**
![Architect](/images/1/runDocker/06.png?featherlight=false&width=60pc)

7. Với **--publish** nhập  **```8081```** và chọn **Recreate container**

![Architect](/images/1/runDocker/07.png?featherlight=false&width=60pc)

#### Test ứng dụng trên postman
8. Chạy project và test trên postman bằng cách tạo request HTTP **GET** với đường dẫn **```http://localhost:8081/api/products```**

![Architect](/images/1/repareIntelli/12.png?featherlight=false&width=60pc)