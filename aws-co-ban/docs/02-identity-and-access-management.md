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
6. Tài liệu tham khảo

======================================================================

1. IAM Policy

IAM Policy xác định một identity được phép thực hiện (Allow) hoặc bị chặn (Deny) những hành động gì, trên những dịch vụ nào của AWS. Policy có thể là Identity‑based (gắn vào IAM User/Group/Role), hoặc Resource‑based (gắn trực tiếp vào tài nguyên như, ví dụ như S3 bucket policy).

QUAN TRỌNG: độ ưu tiên khi xác định quyền trong policy, trong trường hợp có xung đột, từ cao xuống thấp như sau:
- Explicit Deny (từ chối rõ ràng): đây là mức độ ưu tiên cao nhất. Khi trong policy có một luật “Deny” một hành động trên một tài nguyên bất kỳ, identity được/bị gắn policy này sẽ không được phép thực hiện nó, kể cả khi trong cùng policy có một luật khác “Allow” hành động đó trên tài nguyên đó.
- Explicit Allow (cho phép rõ ràng): mức độ ưu tiên tiếp theo, cho phép identity thực hiện hành động, trừ khi có một lệnh Explicit Deny vô hiệu hoá nó (Deny có thể trên cùng hoặc khác policy với Allow, do một identity có thể được gán nhiều policy).
- Implicit Deny (ngầm từ chối): mức độ ưu tiên thấp nhất. Đây là khi trong policy không có lệnh nào đề cập đến một hành động, hành động đó sẽ bị ngầm từ chối.

Dễ thấy rằng thiết kế này của AWS tập trung vào vấn đề bảo mật. Hãy phân tích policy dưới đây (lưu ý, cú pháp JSON không hỗ trợ comment, nếu bạn có dùng policy này để chỉnh sửa, hãy bỏ comment đi):
```
{
    "Version": "2012-10-17",
    "Statement": [
        {   
            "Sid": "S3 Full Access",
            "Effect": "Allow", // Explicit Allow
            "Action": [
                "s3:*",
            ],
            "Resource": [
                "*"
            ]
        },
        {
            "Sid": "Deny access to a bucket",
            "Effect": "Deny", // Explicit Deny
            "Action": [
                "s3:*",
            ],
            "Resource": [
                "arn:aws:s3:::denied-bucket",
                "arn:aws:s3:::denied-bucket/*" // Tất cả object trong bucket
            ]

        }
    ]
}
```

Các trường chính:

- Version: phiên bản (không phải ngày tạo). Giá trị phổ biến: “2012-10-17”.
- Statement (bắt buộc): tập hợp các luật xác định quyền của identity. Mỗi luật gồm có:
- Sid (tuỳ chọn): dùng để đặt tên dễ hình dung cho luật.
- Effect (bắt buộc): “Allow” hoặc “Deny”.
- Action: các hành động bị ảnh hưởng bởi luật.
- Resource: các tài nguyên bị ảnh hưởng bởi luật.
- Ngoài ra còn có thể có Principal (đối tượng áp dụng policy, chỉ dùng cho resource‑based policy), Condition (điều kiện áp dụng), hoặc các trường phủ định như NotAction, NotResource để loại trừ các hành động hoặc tài nguyên không bị luật ảnh hưởng. Đọc thêm tại tài liệu chính thức.

Trong ví dụ trên, luật đầu tiên cho phép (Explicit Allow) thực hiện tất cả hành động trên S3 (lấy danh sách các bucket trên S3, thêm, sửa, xoá S3 object, v.v.) trên tất cả tài nguyên (thực chất là tất cả các bucket và object trên S3, do các hành động trên S3 không thể áp dụng cho các dịch vụ khác).

Luật thứ hai từ chối rõ ràng (Explicit Deny) việc thực hiện tất cả hành động trên bucket denied-bucket và tất cả các object trong bucket đó (xác định bởi denied-bucket/*). Do đó, dù luật đầu tiên rõ ràng cho phép các hoạt động trên bucket này, nhưng luật thứ hai, do là Explicit Deny có độ ưu tiên cao hơn, sẽ vô hiệu hoá tất cả các quyền đó.

Các hành động và dịch vụ khác không được đề cập sẽ bị ngầm từ chối (Implicit Deny).

Tổng hợp lại, identity được/bị gán policy này sẽ có quyền làm mọi thứ trên tất cả S3 bucket, ngoại trừ bucket denied-bucket và các object trong đó.
