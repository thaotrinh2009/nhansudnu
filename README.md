# Biến động nhân sự theo tháng

Trang web đọc 3 file Excel xuất từ hệ thống nhân sự và trả ra, theo tháng được chọn:

1. Nhân sự ký hợp đồng trong tháng (loại HĐ, ngày bắt đầu/kết thúc, lương theo HĐ nếu có)
2. Nhân sự tăng lương (lương cũ → lương mới, chênh lệch)
3. Nhân sự được bổ nhiệm (QĐ chính thức, thử việc, bổ nhiệm, kiêm nhiệm)
4. Nhân sự nghỉ việc

## File đầu vào

| File | Nhận diện theo cột |
|---|---|
| `hop-dong.report…xlsx` | Loại hợp đồng / Mã hợp đồng |
| `su-nghiep.general.report…xlsx` | Phân loại quyết định |
| `quyet-dinh-thoi-viec.report…xlsx` | Ngày nghỉ việc |

File chỉ được đọc trong trình duyệt, không gửi lên máy chủ nào.

## Sử dụng

Mở `index.html` bằng trình duyệt (cần mạng để tải font và thư viện SheetJS), nhập mật khẩu, kéo thả 3 file và chọn tháng.
Có thể xuất toàn bộ 4 bảng ra Excel (theo tháng đang chọn hoặc tất cả các tháng).

## Đổi mật khẩu

Mật khẩu được lưu dưới dạng SHA-256 của chuỗi `hr-bdns:` + mật khẩu, ở hằng `PW_HASH` trong `index.html`.
Đây chỉ là lớp chặn trên giao diện, không phải cơ chế bảo mật thật.
