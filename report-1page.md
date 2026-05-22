# FIT4012 Lab 7 - Báo cáo 1 trang: SHA-256

## 1. Mục tiêu / Objective

Mục tiêu của bài thực hành là hiểu quy trình SHA-256 và áp dụng vào 3 bài toán cơ bản: băm dữ liệu, kiểm tra toàn vẹn file, và xác thực mật khẩu. Ngoài ra, lab mở rộng bằng salted hash để thấy rõ vì sao cùng mật khẩu nhưng salt khác nhau sẽ cho kết quả hash khác nhau.

## 2. Cách làm / Approach

Tóm tắt cách nhóm/cá nhân đã thực hiện:

- Biên dịch và chạy `sha_procedure.cpp`.
- Kiểm thử SHA-256 bằng known answer test vector.
- Viết/chạy chương trình kiểm tra toàn vẹn file.
- Viết/chạy chương trình băm mật khẩu.
- Bổ sung salt để tránh việc hai người có cùng mật khẩu tạo ra cùng hash.

Môi trường chạy thực tế dùng Windows + PowerShell, nên thay vì `make`/`bash` thì biên dịch trực tiếp bằng `g++ -std=c++17 -Wall -Wextra -pedantic` và chạy từng executable để kiểm chứng kết quả.

## 3. Kết quả / Result

Minh chứng chính đã chạy:

- Hash của chuỗi `abc`:
  `ba7816bf8f01cfea414140de5dae2223b00361a396177a9cb410ff61f20015ad`
- Hash của file mẫu trước khi sửa:
  `ff24f6f89e1339e1f98043e985a1c3da1bacaf387d9ba0a7081936d6f1ebad19`
- Kết quả kiểm tra file sau khi sửa nội dung:
  `[FAIL] File was changed or expected hash is incorrect` (đúng kỳ vọng negative test)
- Kết quả đăng nhập với mật khẩu đúng:
  `[PASS] Login success`
- Kết quả đăng nhập với mật khẩu sai:
  `[FAIL] Login failed: wrong password`
- Hai bản ghi `salt:hash` của cùng một mật khẩu có giống nhau không?
  Không giống nhau.
  - `0750d912481dce06f32417f4d8eee9a6:ca529e8f6c9c01633d4d6823366406614e204c06a60aeca08e424fea06d12733`
  - `c2a3fff3d515c3aed37d7aae68fe99f0:52fd8916a26155350ebb780fd21c457c4cb2bdc5581e2fd1927f8eea18757fdb`

## 4. Kết luận / Conclusion

Nêu ngắn gọn điều rút ra:

- SHA-256 giúp phát hiện thay đổi dữ liệu như thế nào?
  SHA-256 tạo fingerprint 256-bit của dữ liệu. Chỉ cần thay đổi rất nhỏ (ví dụ thêm 1 dòng “tamper”), hash mới sẽ khác hoàn toàn, từ đó phát hiện file bị chỉnh sửa.
- Vì sao cần salt khi lưu hash mật khẩu?
  Salt làm cho cùng một mật khẩu không còn tạo ra cùng hash giữa các tài khoản/lần đăng ký, giúp giảm hiệu quả rainbow table và tránh lộ thông tin “2 người dùng cùng mật khẩu”.
- Vì sao SHA-256 demo trong lab chưa nên dùng trực tiếp cho hệ thống xác thực thật?
  Vì SHA-256 quá nhanh, dễ bị brute-force trên phần cứng mạnh. Hệ thống thật nên dùng thuật toán chuyên cho password hashing như Argon2id, bcrypt hoặc scrypt (kèm cấu hình cost phù hợp).
