---
title : "Giới thiệu"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 1. </b> "
---

#### Workshop này sẽ bao gồm các tài nguyên và công cụ AWS sau đây: 

+ **AWS ECS**: Dịch vụ Elastic Container Service là dịch vụ điều phối container của AWS. Với dịch vụ này, bạn có thể quản lý việc thực thi các container dịch vụ nhỏ dựa trên Docker một cách mạnh mẽ và có khả năng mở rộng. Với AWS Fargate, dịch vụ tính toán không máy chủ dành cho các container từ Amazon Web Services, bạn không cần tạo các máy EC2, giảm chi phí vận hành của các ứng dụng dựa trên container.

+ **AWS ECR**: Với Elastic Container Registry (ECR) từ AWS, bạn có thể tạo các kho lưu trữ riêng để lưu trữ các hình ảnh Docker của các dịch vụ nhỏ.
  
+ **AWS VPC**: Với Virtual Private Cloud (VPC) từ AWS, bạn có thể bảo vệ cơ sở hạ tầng bằng các mạng con riêng và các chính sách bảo mật mạng cho các quy tắc lưu lượng đi vào và đi ra.

+ **AWS ALB**: Application Load Balancer từ AWS cho phép cân bằng lưu lượng HTTP đến giữa tất cả các phiên bản ứng dụng có sẵn, và với các nhóm mục tiêu tích hợp, mỗi phiên bản có thể được giám sát để chỉ nhận lưu lượng nếu nó đang hoạt động bình thường.

+ **API Gateway REST**: Với AWS API Gateway, bạn có thể bảo vệ API REST của ứng dụng, cũng như thực hiện kiểm tra hợp lệ của các tham số query string và nội dung yêu cầu.

+ **CloudWatch Logs**: Trách nhiệm của nó là tập trung các nhật ký ứng dụng và các chỉ số của chúng. Các ứng dụng sẽ được tạo ra trong khóa học này sẽ tạo ra các nhật ký vào CloudWatch Logs theo định dạng JSON, sử dụng thư viện log4j2. Điều này cho phép chúng ta chèn các tham số vào các nhật ký, để được sử dụng trong các truy vấn trong bảng điều khiển AWS CloudWatch Logs Insights.

+ **CloudWatch Alarms**: Với các cảnh báo từ CloudWatch, bạn có thể theo dõi các sự kiện bất thường từ các ứng dụng và tài nguyên AWS.

+ **CloudWatch Logs**: Trách nhiệm của nó là tập trung các nhật ký ứng dụng và các chỉ số của chúng. Các ứng dụng sẽ được tạo ra trong khóa học này sẽ tạo ra các nhật ký vào CloudWatch Logs theo định dạng JSON, sử dụng thư viện log4j2. Điều này cho phép chúng ta chèn các tham số vào các nhật ký, để được sử dụng trong các truy vấn trong bảng điều khiển AWS CloudWatch Logs Insights.

+ **DynamoDB**: DynamoDB là một dịch vụ quản lý cơ sở dữ liệu NoSQL mạnh mẽ và phi quan hệ. Khóa học này giới thiệu việc sử dụng DynamoDB enhanced client từ AWS SDK V2 cho Java, đây là một thư viện cấp cao cho phép ánh xạ các lớp phía máy khách với các bảng DynamoDB.

+ **SQS**: SQS là một dịch vụ hàng đợi cho phép giao tiếp bất đồng bộ giữa các ứng dụng, nhằm trao đổi các thông điệp và sự kiện.

