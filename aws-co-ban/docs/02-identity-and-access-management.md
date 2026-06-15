IAM (Identity and Access Management) là dịch vụ quản lý việc truy cập AWS một cách an toàn và bảo mật, kiểm soát đăng nhập và quyền sử dụng dịch vụ của từng danh tính. IAM cho phép tạo thêm các danh tính (identity) trong tài khoản AWS. Một IAM identity khi được tạo sẽ không có bất cứ quyền sử dụng bất cứ dịch vụ nào, nhưng có thể được cấp quyền tối đa gần tương đương với quyền của Root User. Có 3 loại identity có thể tạo trong IAM: IAM User, IAM Group, IAM Role. Nhưng trước đó, hãy tìm hiểu cách cấp quyền thông qua IAM Policy.

Trong bài này:
1. IAM Policy
- Explicit Deny
- Explicit Allow
- Implicit Deny
- ARN
- Inline Policy
- Managed Policy
2. IAM User
3. IAM Group
4. IAM Role
- Trust Policy
- Permission Policy
5. Resource Policy
Tài liệu tham khảo
