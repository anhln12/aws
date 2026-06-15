Bài này giới thiệu tổng quan về hạ tầng toàn cầu của AWS, các khái niệm quan trọng như Region, Availability Zone, thế nào là một ứng dụng đáp ứng yêu cầu High Availability, Fault Tolerance. Ta cũng sẽ tìm hiểu mô hình chia sẻ trách nhiệm giữa AWS và khách hàng.

Trong bài này:
1. Hạ tầng toàn cầu của AWS
1.1. Khu vực (Region)
1.2. Vùng khả dụng (Availability Zone)
1.3. Edge Location
1.4. Local Zone
1.5. Outpost
2. High Availability, Fault Tolerance, Disaster Recovery
2.1. High Availability (HA)
2.2. Fault Tolerance (FT)
2.3. Disaster Recovery (DR)
2.4. Khả năng phục hồi của dịch vụ AWS
3. Mô hình chia sẻ trách nhiệm
Tài liệu tham khảo

=====================================================================

1. Hạ tầng toàn cầu của AWS
1.1. Khu vực (Region)
AWS triển khai hạ tầng tại các địa điểm vật lý khác nhau trên toàn cầu, gọi là Region. Mỗi Region được đặt các trung tâm dữ liệu (data center) để cung cấp dịch vụ cho khách hàng. Các Region hoạt động độc lập với nhau, và có thể lựa chọn Region phù hợp nhất khi triển khai dịch vụ AWS để giảm độ trễ, tăng khả năng chịu lỗi, hay tuân thủ các yêu cầu về dữ liệu. Ví dụ, nếu phục vụ khách hàng tại Việt Nam, có thể chọn Region Singapore (ap-southeast-1), Indonesia (ap-southeast-3), hay Malaysia (ap-southeast-5) để giảm độ trễ, nếu dịch vụ AWS cần dùng khả dụng tại các Region này.
