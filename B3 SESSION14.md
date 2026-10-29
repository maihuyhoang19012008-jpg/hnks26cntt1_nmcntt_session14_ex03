


# Phần 1: Phân tích Lỗ hổng Bảo mật

## 1. Xác định nguồn gốc rò rỉ dữ liệu người dùng

CẤP SAI QUYỀN CHO NGƯỜI KHÔNG CÓ THẨM QUYỀN 


## 2. Giải thích: 
 Tại sao "Email người lạ" truy cập được `campuspay_api_keys.txt`?

File được đặt là `Restricted — Email B`, nhưng vẫn có người lạ truy cập. Dưới đây là **3 giả thuyết có thể xảy ra**:

### Giả thuyết 1: Người được cấp quyền đã chia sẻ tiếp

-   Quyền `Viewer` cho phép người nhận **tạo bản sao** của file; 
-    Google Drive chỉ kiểm tra quyền truy cập vào file gốc, không chặn được việc chuyển link thủ công.

### Giả thuyết 2: Link bị rơi vào tay người khác qua lộ trình bên ngoài

-   Email B đăng nhập trên thiết bị không an toàn, bị đánh cắp phiên đăng nhập, hoặc để mở link ở nơi công cộng  
-   Link được lưu trong trình duyệt, lịch sử, hoặc ghi chú bị chia sẻ không chủ ý.

### Giả thuyết 3: Sai sót trong quản lý quyền hoặc kế thừa thư mục

-   File nằm trong thư mục có quyền chia sẻ rộng hơn → **quyền thư mục ghi đè lên quyền file**; cài đặt `Restricted` chỉ áp dụng cho file nhưng thư mục cha vẫn mở rộng.
-   Hoặc đã từng chia sẻ rộng trước đó, sau đó sửa thành `Restricted` nhưng hệ thống chưa cập nhật ngay, hoặc có link cũ còn hiệu lực do chưa thu hồi.

----------
# Phần 2: Sửa lỗi & Nghiệm thu Bảo mật

### 3. Bảng cấu hình quyền chia sẻ đúng chuẩn



**Viewer**

Restricted  Chỉ email thành viên cần thiết

Chứa PII (thông tin cá nhân nhạy cảm) + số dư tài khoản => chỉ đọc, cấm chia sẻ tiếp; giới hạn chính xác người cần dùng. Tuyệt đối KHÔNG dùng `Anyone with the link`.

`campuspay_api_keys.txt`

**Viewer**

Restricted  Chỉ email người quản lý hệ thống

Khóa API là thông tin bảo mật cấp cao => tối thiểu người biết, chỉ đọc, không được lưu/tải ra ngoài.

`slide_pitch_v8.pptx`

**Commenter** hoặc **Viewer**

Restricted  Thành viên nhóm dự án

Tài liệu giao tiếp, không chứa dữ liệu nhạy cảm; `Commenter` đủ để góp ý mà không thay đổi bản gốc. Nếu cần công khai → đặt riêng bản không chứa thông tin nội bộ.

#### Nguyên tắc áp dụng chung:

-   Tuân thủ **Quy tắc Cờ Đỏ**: Không bao giờ kết hợp `Editor` + `Anyone with the link`.
-   Nguyên tắc **Quyền tối thiểu**: Ai chỉ cần xem cho chế độ `Viewer`; ai cần góp ý cho `Commenter`; chỉ người thực sự chịu trách nhiệm biên tập mới được `Editor` 
----------

### 4. Thu hồi link cũ & Đặt Ngày hết hạn

#### Tính năng vô hiệu hóa tất cả link cũ nhanh nhất:

> **Tắt quyền "Người có link" trên toàn bộ file** — vào Cài đặt chia sẻ => Phạm vi truy cập => Chọn **"Thu hồi quyền truy cập của mọi người có link"** / 
