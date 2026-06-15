1. Tổng quan
2. Root User
3. AWS Organizations
- Tài khoản quản lý (Management Account)
- Tài khoản thành viên (Member Account)
- Đơn vị tổ chức (Organizational Unit - OU)
- Service Control Policy - SCP
4. Tài liệu tham khảo

======================================================================

1. Tổng quan

<img width="379" height="482" alt="image" src="https://github.com/user-attachments/assets/d17ca70e-9df7-4f2d-b79a-eaf27c5617ab" />

Một cách ngắn gọn, có thể hiểu tài khoản AWS như một "container" chứa các thực thể được tạo bởi tài khoản (IAM User, IAM Group, IAM Role) và tài nguyên AWS được cung cấp cho các thực thể đó. Tất cả chi phí phát sinh trong quá trình sử dụng các dịch vụ, bất kể được dùng bởi thực thể nào sẽ được tính vào chi phí của tài khoản và được khấu trừ theo phương thức thanh toán.

2. Root User

Khi tạo tài khoản AWS, bạn cung cấp email, tên tài khoản, và cách thức thanh toán (thẻ tín dụng/ ghi nợ). Root User là tài khoản chính được tạo sau khi đăng ký

Root User có tất cả quyền đối với tài khoản, có thể truy cập đầy đủ vào tất cả dịch vụ và tài nguyên, và không bị hạn chế (có nghĩa là một IAM User có quyền AdministratorAccess cũng không thể xóa quyền của Root User, ngược lại Root User có thể xóa quyền của IAM User admin đó). 

Do đó, để tránh các vấn đề bảo mật, nên dùng Root User tạo một IAM User với quyền AdministratorAccess, rồi đăng nhập vào IAM User Admin này để quản lý công việc.

3. AWS Orgainzations

Ở trên ta đã tìm hiểu về tài khoản AWS. Một công ty thường sẽ có nhiều tài khoản AWS: tài khoản theo môi trường (Development, Staing, Production), tài khoản theo phòng ban ...Khi đó sẽ có nhiều vấn đề phát sinh liên quan đến bảo mật, cấp quyền, tối ưu chi phí. AWS Organizations sẽ giúp ta giải quyết các vấn đề này.

Organizations bản chất là một tập hợp các tài khoản AWS được quản lý tập trung, có cấu trúc phân tầng hình cây như sau:
<img width="608" height="379" alt="image" src="https://github.com/user-attachments/assets/2786f6f5-1cb1-4b60-8a73-7d63806a9b20" />

