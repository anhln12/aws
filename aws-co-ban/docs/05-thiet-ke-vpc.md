VPC Mặc định được AWS cấu hình sẵn để người dùng nhanh chóng bắt đầu triển khai ứng dụng, tuy nhiên các subnet trong đó đều là public, không phù hợp với nhiều ứng dụng phân tầng cần bảo mật dữ liệu và logic tính toán. Trong các trường hợp này, tự thiết kế VPC là yêu cầu bắt buộc.

Trong bài này:
1. Chọn CIDR
2. Thiết kế VPC cho ứng dụng đa tầng
2.1. Cấu trúc đa tầng
2.2. Cấu trúc đa vùng, đa giai đoạn
2.3. Ví dụ
Tài liệu tham khảo

==================

1. Chọn CIDR

Khi tạo VPC, ta cần chỉ rõ IPv4 CIDR. IPv4 chỉ gồm 32 bit, tức tổng cộng 2^32 ~ hơn 4 tỷ địa chỉ IPv4, rõ ràng là không đủ nếu tất cả các thiết bị kết nối trên toàn cầu đều cần có IP duy nhất. Thực tế, không cần cấp phát IP duy nhất cho tất cả thiết bị. Trong một tổ chức, rất nhiều thiết bị chỉ cần kết nối nội bộ, không cần kết nối với thiết bị trong tổ chức khác hoặc với Internet, nên chỉ cần IP là duy nhất trong phạm vi tổ chức là đủ. Do đó, các tổ chức có thể sử dụng chung IP, miễn là không kết nối với nhau. Nghe rất quen, phải không? VPC chính là một ví dụ khác (xem lại bài trước): 2 VPC cô lập (không kết nối với nhau) có thể có CIDR trùng nhau, tức chung nhiều IP.

Ý tưởng này được đề xuất trong RFC 1918 để giải quyết vấn đề cạn kiệt địa chỉ IPv4. Họ đề xuất dành 3 khối IP cho nhu cầu kết nối nội bộ (private address space), không dùng làm IP công cộng:
- 10.0.0.0/8: từ 10.0.0.0 đến 10.255.255.255
- 172.16.0.0/12: từ 172.16.0.0 đến 172.31.255.255
- 192.168.0.0/16: từ 192.168.0.0 đến 192.168.255.255

Tổng cộng, 2^24 + 2^20 + 2^16 tức gần 18 triệu địa chỉ IP được dành riêng cho kết nối nội bộ. AWS cũng khuyến nghị chọn CIDR nằm trong 3 dải địa chỉ này khi tạo VPC, cũng như một vài dải địa chỉ cần tránh, xem thêm tại đây.

Kích thước VPC (và subnet) tối thiểu là /28 (16 IP), tối đa là /16 (65,536 IP), sau đó trừ đi tổng số IP không thể sử dụng trong mỗi CIDR (mỗi subnet trong VPC có 5 IP không thể sử dụng, xem lại bài trước).

Dưới đây là bảng tổng hợp kích thước VPC và subnet phổ biến:
|Kích thước VPC|Số subnet trong VPC|Kích thước Subnet|Số IP khả dụng mỗi subnet|Tổng số IP khả dụng trong VPC|
-----
|/24|8|/27|27|216|
