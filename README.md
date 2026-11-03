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

Xuất Excel ra **1 sheet** “Biến động nhân sự”: bảng tổng hợp ở trên, sau đó 4 bảng (Ký hợp đồng, Tăng lương, Bổ nhiệm, Nghỉ việc) xếp lần lượt.

## Tab Đi muộn – Về sớm

Có 2 cách nạp dữ liệu (trang tự nhận diện file theo tiêu đề cột):

1. **Tính từ dữ liệu gốc**: kéo thả cùng lúc file **vân tay** (cột Mã nhân sự, Ngày, Giờ vào, Giờ ra) và file **đơn từ** `Exported.Timeoff…xls` (cột Mã nhân sự, @tên nhóm, Ca đề xuất, Trạng thái).
   - Đi muộn: giờ vào sau 08:00. Về sớm: giờ ra trước 17:00 (chỉ khi có giờ ra). Ngày chỉ chấm 1 lần: lần chấm được coi là giờ vào.
   - Quên chấm công: ngày làm việc không có lần chấm nào; chỉ xét nhân sự đã chấm ít nhất 1 lần trong file.
   - Bỏ Chủ nhật, Thứ 7 và các ngày lễ nhập tay.
   - **Bị tính**: không có đơn, hoặc chỉ có đơn “Giải trình đi muộn về sớm”. **Không tính**: trùng ngày với đơn khác (nghỉ phép, làm việc ngoài văn phòng…). Đơn “Đã hủy” bị bỏ qua.
   - Giờ chuẩn, Thứ 7, ngày lễ và trạng thái đơn chỉnh được ngay trên trang.
2. **Báo cáo đã xử lý**: 1 file có sheet **Dữ liệu gốc** (cột Tháng, Mã NV, Họ và tên, Khoa/Bộ phận, Loại vi phạm, Ngày, Giờ vào, Giờ ra, Muộn/Sớm (phút), Đơn từ liên quan, Kết quả).

Trang hiện: tóm tắt theo tháng và theo khoa / bộ phận, tổng hợp theo nhân sự, chi tiết lượt **BỊ TÍNH** và các lượt có đơn **KHÔNG TÍNH**.
Xuất Excel theo tháng đang chọn ra **1 sheet**: tóm tắt theo tháng, theo khoa / bộ phận, tổng hợp nhân sự, chi tiết bị tính, có đơn – không tính (tải lại file này vào tab là xem được ngay).

## Sử dụng

Mở `index.html` bằng trình duyệt (cần mạng để tải font và thư viện SheetJS), nhập mật khẩu, kéo thả 3 file và chọn tháng.
Có thể xuất toàn bộ 4 bảng ra Excel (theo tháng đang chọn hoặc tất cả các tháng).

## Đổi mật khẩu

Mật khẩu được lưu dưới dạng SHA-256 của chuỗi `hr-bdns:` + mật khẩu, ở hằng `PW_HASH` trong `index.html`.
Đây chỉ là lớp chặn trên giao diện, không phải cơ chế bảo mật thật.
