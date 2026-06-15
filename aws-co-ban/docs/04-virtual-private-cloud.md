Trong bài này:
1. Kết nối tới AWS
1.1. Public Network
1.2. Private Network
2. Các khái niệm liên quan về mạng máy tính
2.1. CIDR
2.2. Subnet
3. Virtual Private Cloud (VPC)
3.1. VPC Mặc định
3.2. VPC Subnet
Tài liệu tham khảo

=============================

1. Kết nối tới AWS

Dưới góc độ kết nối mạng, có thể phân loại các dịch vụ AWS trong 2 vùng chính:

<img width="771" height="503" alt="image" src="https://github.com/user-attachments/assets/f48a20bd-6f44-4d79-b6e2-d7a95d751e42" />

1.1. Public Network

Đây là vùng mạng có kết nối với Internet. Các dịch vụ trong vùng này có public endpoint và DNS, có thể truy cập từ Internet. Ví dụ: S3 (lưu trữ), API Gateway (quản lý API), CloudFront (CDN), sẽ được trình bày trong các bài sau.

Lưu ý rằng, mặc dù gọi là “public” (mang ý nghĩa có kết nối với mạng Internet công khai), nhưng chỉ các danh tính được cấp quyền mới có thể truy cập (xem lại bài IAM).

1.2. Private Network

Đây là vùng mạng chính thường được sử dụng trong AWS, bao gồm một hoặc nhiều mạng nội bộ ảo của AWS, gọi là VPC (Virtual Private Cloud).

Mặc định, một VPC không có bất cứ kết nối gì với các VPC khác (kể cả của cùng một tài khoản và trong cùng một vùng). Chúng cũng hoàn toàn cô lập với bên ngoài, trừ khi được gán thêm Internet Gateway và cấu hình định tuyến phù hợp để kết nối với các dịch vụ trong Public Network và Internet. Nó giống như việc bạn có thể thiết lập một mạng nội bộ kết nối các thiết bị tại nhà, nhưng để kết nối với Internet, bạn cần Modem của nhà cung cấp dịch vụ Internet, và Router để gán địa chỉ IP và định tuyến đến các thiết bị.

Các dịch vụ trong một VPC chỉ có thể truy cập từ các dịch vụ khác trong cùng VPC, trong một mạng khác kết nối với VPC đó (có thể là một VPC khác hoặc từ mạng on-premise kết nối với VPC bằng VPN hoặc Direct Connect).

2. Các khái niệm liên quan về mạng máy tính

Vậy là ta đã biết vị trí của VPC trong bức tranh tổng thể, trước khi đi vào chi tiết hơn, hãy cùng ôn lại một số khái niệm mạng máy tính quan trọng có liên quan.

2.1. CIDR

CIDR (Classless Inter-Domain Routing, tạm dịch Định tuyến liên miền không phân lớp", phiên âm ˈ/saɪdər, ˈsɪ-/, việt hoá đọc là sai-đơ) là một phương pháp phân phối địa chỉ IP hiệu quả.

Công thức tính số địa chỉ IP trong một cụm CIDR là: 2 ^ (32 - prefix)

Với prefix là số sau dấu /. Ví dụ, với CIDR sau:
```
172.31.0.0/8
```

Phần trước dấu / (tức 172.31.0.0) là địa chỉ bắt đầu của cụm CIDR. Mỗi chữ số trong địa chỉ IP gồm 8 bit, có giá trị từ 0 đến 255, cách nhau bằng dấu chấm. Địa chỉ IP gồm 4 phần như vậy, tổng cộng 32 bit.

Phần /8 gọi là tiền tố (prefix), cho biết số bit cố định từ bên trái của địa chỉ IP, phần còn lại có thể thay đổi để tạo ra các địa chỉ IP khác trong cụm CIDR đó. Trong ví dụ này, /8 nghĩa là 8 bit đầu tiên cố định (172 = 10101100), còn lại (32 - 8 = 24) bit có thể thay đổi (từ 0.0.0 đến 255.255.255, thay toàn bộ bit 0 bằng bit 1).

24 bit có thể thay đổi, mỗi bit có hai giá trị khả dĩ là 0 hoặc 1, nên tổng số địa chỉ IP trong cụm CIDR này là 2^24 = 16 777 216 địa chỉ IP, từ 172.0.0.0 đến 172.255.255.255.

Dựa vào tiền tố, ta có thể xác định kích thước của cụm CIDR. Tiền tố càng lớn, số bit cố định càng nhiều, cũng tức là số bit có thể thay đổi càng ít, dẫn đến số địa chỉ IP càng ít.

2.2. Subnet

Subnet (subnetwork, tạm dịch “mạng con”) là một phân đoạn mạng nhỏ hơn được chia từ một mạng lớn hơn. Mỗi subnet được xác định bởi một cụm CIDR con (sub-CIDR) của cụm CIDR lớn hơn. Ví dụ, từ cụm CIDR 172.31.0.0/16, nếu muốn chia thành các subnet nhỏ hơn với kích thước mỗi subnet là 256 địa chỉ IP, tương ứng với CIDR con có tiền tố là /24 (vì 256 = 2^8, ta cần 8 bit thay đổi, tức 32 - 8 = 24 bit cố định), ta có thể tạo ra tối đa 2^(24-16) = 256 subnet con như sau:
- Subnet 1: 172.31.0.0/24
- Subnet 2: 172.31.1.0/24
- Subnet 3: 172.31.2.0/24
…
- Subnet 256: 172.31.255.0/24

Mỗi subnet sẽ có 256 địa chỉ IP khả dụng (từ 172.31.x.0 đến 172.31.x.255).

Giờ hãy quay lại tìm hiểu kỹ hơn về VPC.


