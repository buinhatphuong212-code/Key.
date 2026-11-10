# RED KEY STORE
Website bán key nhiều trang: HTML/CSS/JS + GitHub Pages + Supabase.

## Quan trọng trước khi mở bán thật
- GitHub Pages chỉ host frontend tĩnh; Supabase xử lý đăng nhập, database, ví và giao key.
- Thanh toán trong bộ khung này là **chuyển khoản và admin đối soát thủ công**. Không có tích hợp ngân hàng tự động.
- Không có hệ thống nào bảo đảm tuyệt đối không lỗ hoặc không bị tấn công. Hãy chạy thử với dữ liệu/test account trước khi nhận tiền thật.
- Chỉ bán key/phần mềm bạn có quyền phân phối; tuân thủ quy định và điều khoản dịch vụ liên quan.

## 1. Tạo Supabase
1. Tạo project tại https://supabase.com/ .
2. Vào SQL Editor, mở `sql/schema.sql`, dán toàn bộ và chạy.
3. Vào Project Settings → API, sao chép Project URL và `anon` public key.
4. Mở `assets/config.js`, thay `YOUR_PROJECT_ID` và `YOUR_SUPABASE_ANON_PUBLIC_KEY`.
5. Điền thông tin ngân hàng trong config. Chỉ điền thông tin tài khoản nhận tiền mà bạn được phép sử dụng.
6. Authentication → URL Configuration: đặt Site URL là URL GitHub Pages của bạn; thêm URL trang GitHub Pages vào Redirect URLs.
7. Đăng ký tài khoản đầu tiên. Xác nhận email nếu được yêu cầu.
8. Trong SQL Editor, chạy câu lệnh cuối README/schema.sql để cấp quyền admin cho email của bạn. Thay email mẫu bằng email tài khoản của bạn.

## 2. Đưa lên GitHub Pages bằng Android
1. Tạo repository GitHub mới, ví dụ `red-key-store`.
2. Tải ZIP này, giải nén bằng ứng dụng quản lý tệp trên Android.
3. Đưa toàn bộ file và thư mục bên trong `red-key-store` lên repository (không tải nguyên thư mục lồng thêm một cấp).
4. GitHub → repository → Settings → Pages.
5. Build and deployment: chọn Deploy from a branch; branch `main`, folder `/ (root)`; Save.
6. Mở URL GitHub Pages được GitHub hiển thị.

## 3. Vận hành
- Tạo sản phẩm trong Admin → Thêm sản phẩm.
- Thêm từng key tồn kho ở Admin → Thêm key; ID sản phẩm lấy từ bảng `products` trong Supabase Table Editor.
- Người mua đăng ký, gửi yêu cầu nạp và chuyển khoản theo thông tin ngân hàng.
- Admin phải kiểm tra giao dịch thực nhận trong ứng dụng ngân hàng trước khi duyệt. Không duyệt chỉ dựa trên ảnh chụp màn hình/ lời nhắn của khách.
- Khi số dư đã được cộng, khách mua sản phẩm. RPC `buy_product` khóa tồn kho và trừ ví trong một transaction; key chỉ trả về sau khi mua thành công.

## Bảo mật
- Tuyệt đối không đưa `service_role` key, mật khẩu database, mã OTP hoặc thông tin đăng nhập ngân hàng lên GitHub hay `config.js`.
- Không cấp quyền ghi trực tiếp cho client vào `profiles`, `wallet_ledger`, `deposits`, `orders` hoặc `inventory_keys`.
- Bật xác nhận email, dùng mật khẩu mạnh, bật MFA cho tài khoản admin, giới hạn người có quyền repository.
- Sao lưu database thường xuyên; kiểm tra log, đối soát tiền và tồn kho.
- Trước khi kinh doanh thật, nhờ người có kinh nghiệm kiểm thử bảo mật SQL/RLS và quy trình thanh toán. Bộ khung này không phải chứng nhận an toàn hay tư vấn pháp lý/thuế.

## Cấu trúc
- `index.html`: cửa hàng
- `login.html`, `register.html`: tài khoản
- `wallet.html`: số dư và yêu cầu nạp
- `history.html`: đơn hàng, nạp tiền, sổ cái
- `admin.html`: duyệt nạp, tạo sản phẩm, thêm key
- `sql/schema.sql`: bảng, RLS và RPC giao dịch
