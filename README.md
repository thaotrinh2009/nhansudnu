# Biến động nhân sự theo tháng

Trang web đọc 3 file Excel xuất từ hệ thống nhân sự và trả ra, theo tháng được chọn:

1. Nhân sự ký hợp đồng trong tháng (loại HĐ, ngày bắt đầu/kết thúc, lương theo HĐ nếu có)
2. Nhân sự tăng lương (lương cũ → lương mới, chênh lệch)
3. Nhân sự giảm lương: QĐ “Giảm lương”, và QĐ “Tăng lương” / “Điều chỉnh lương” có lương mới thấp hơn lương cũ
4. Nhân sự được bổ nhiệm (QĐ chính thức, thử việc, bổ nhiệm, kiêm nhiệm)
5. Nhân sự nghỉ việc (tháng M gồm cả người nghỉ vào ngày cuối của tháng M-1, VD tháng 10 có cả người nghỉ ngày 30/09)

Lương cũ khi tăng/giảm lương = quyết định nhân sự gần nhất trước ngày hiệu lực có ghi lương (bỏ QĐ không ghi lương, QĐ bị từ chối/huỷ); nếu không có thì lấy lương hợp đồng gần nhất.

## File đầu vào

| File | Nhận diện theo cột |
|---|---|
| `hop-dong.report…xlsx` | Loại hợp đồng / Mã hợp đồng |
| `su-nghiep.general.report…xlsx` | Phân loại quyết định |
| `quyet-dinh-thoi-viec.report…xlsx` | Ngày nghỉ việc |

File chỉ được đọc trong trình duyệt, không gửi lên máy chủ nào.

Xuất Excel ra **1 sheet** “Biến động nhân sự”: bảng tổng hợp ở trên, sau đó 5 bảng (Ký hợp đồng, Tăng lương, Giảm lương, Bổ nhiệm, Nghỉ việc) xếp lần lượt.

## Tab Đi muộn - về sớm - quên chấm công

Có 2 cách nạp dữ liệu (trang tự nhận diện file theo tiêu đề cột):

1. **Tính từ dữ liệu gốc**: kéo thả cùng lúc file **vân tay** (cột Mã nhân sự, Ngày, Giờ vào, Giờ ra) và file **đơn từ** `Exported.Timeoff…xls` (cột Mã nhân sự, @tên nhóm, Ca đề xuất, Trạng thái).
   - Đi muộn: giờ vào sau 08:00. Về sớm: giờ ra trước 17:00 (chỉ khi có giờ ra). Ngày chỉ chấm 1 lần: lần chấm được coi là giờ vào.
   - Quên chấm công: ngày làm việc không có lần chấm nào. Chỉ tính nhân sự có chấm công: tháng nào một người không chấm lần nào (VD: giảng viên) thì bỏ qua người đó trong cả tháng.
   - Bỏ Chủ nhật, Thứ 7 và các ngày lễ nhập tay.
   - **Bị tính**: không có đơn, hoặc chỉ có đơn “Giải trình đi muộn về sớm”. **Không tính**: trùng ngày với đơn khác (nghỉ phép, làm việc ngoài văn phòng…). Đơn “Đã hủy” bị bỏ qua.
   - Giờ chuẩn, Thứ 7, ngày lễ và trạng thái đơn chỉnh được ngay trên trang.
2. **Báo cáo đã xử lý**: file DS vi phạm do trang xuất ra (sheet Chi tiết BỊ TÍNH + Có đơn - Không tính), hoặc 1 file có sheet **Dữ liệu gốc** (cột Tháng, Mã NV, Họ và tên, Khoa/Bộ phận, Loại vi phạm, Ngày, Giờ vào, Giờ ra, Muộn/Sớm (phút), Đơn từ liên quan, Kết quả).

Trang hiện: tóm tắt theo tháng và theo khoa / bộ phận, tổng hợp theo nhân sự, chi tiết lượt **BỊ TÍNH** và các lượt có đơn **KHÔNG TÍNH**.
Xuất Excel theo tháng đang chọn ra file **DS vi phạm chấm công** (theo mẫu của phòng nhân sự), gồm 6 sheet:

1. **Tổng hợp**: mỗi nhân sự có vi phạm bị tính (kể cả người chỉ quên chấm công) một dòng, xếp theo Khoa/Bộ phận rồi họ tên; mỗi tháng 3 cột Đi muộn / Về sớm / Quên CC, cộng tổng và TỔNG VI PHẠM, dòng TỔNG cuối bảng.
2. **Chi tiết BỊ TÍNH**: STT, Tháng (T4-2026), Ngày, Mã NV, Họ và tên, Khoa/Bộ phận, Giờ vào, Giờ ra, Loại vi phạm, Muộn/Sớm (phút) (“-” với quên chấm công), Đơn từ liên quan.
3. **Có đơn - Không tính**: cùng cột, để tham khảo.
4. **Tóm tắt theo tháng**, 5. **Theo Khoa - Bộ phận**, 6. **Ghi chú - Quy tắc** (giờ chuẩn, ngày loại trừ, số nhân sự phân tích lấy từ cấu hình trên trang).

Các sheet có cố định dòng tiêu đề, bật lọc, ẩn đường lưới. Tải lại file này (hoặc file mẫu cùng cấu trúc) vào tab là xem được ngay: trang đọc sheet “Chi tiết BỊ TÍNH” và “Có đơn - Không tính”.

## Định dạng file Excel xuất ra

Tab Biến động nhân sự xuất 1 sheet: tiêu đề lớn, khối Kỳ báo cáo / Ngày xuất, mỗi bảng có thanh tiêu đề mục màu xanh nhạt, dòng tiêu đề cột nền xanh chữ trắng, viền mảnh, dòng xen kẽ màu, số tiền có dấu phân cách hàng nghìn, chữ dài tự xuống dòng. Bảng Biến động nhân sự có thêm cột STT.

## Sử dụng

Mở `index.html` bằng trình duyệt (cần mạng để tải font và thư viện xlsx-js-style – bản SheetJS có hỗ trợ định dạng ô), nhập mật khẩu, kéo thả 3 file và chọn tháng.
Có thể xuất toàn bộ 4 bảng ra Excel (theo tháng đang chọn hoặc tất cả các tháng).

## Đổi mật khẩu

Mật khẩu được lưu dưới dạng SHA-256 của chuỗi `hr-bdns:` + mật khẩu, ở hằng `PW_HASH` trong `index.html`.
Đây chỉ là lớp chặn trên giao diện, không phải cơ chế bảo mật thật.
