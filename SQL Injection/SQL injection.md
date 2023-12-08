## SQL Injection là gì ##
SQL Injection là một kỹ thuật lợi dụng những lỗ hổng về câu truy vấn của các ứng dụng. Được thực hiện bằng cách chèn thêm một đoạn SQL để làm sai lệch đi câu truy vấn ban đầu, từ đó có thể khai thác dữ liệu từ database.
## Nguyên nhân ##
- Dữ liệu đầu vào từ người dùng hoặc từ các nguồn khác không được kiểm tra hoặc kiểm tra không kỹ lưỡng
- Ứng dụng sử dụng các câu lệnh SQL động, trong đó dữ liệu được kết nối với mã SQL gốc để tạo câu lệnh SQL hoàn chỉnh
## Hậu quả ##
SQL injection có thể cho phép kẻ tấn công:
- Vượt qua các khâu xác thực người dùng
- Chèn, xóa hoặc sửa đổi dữ liệu
- Đánh cắp các thông tin trong cơ sở dữ liệu
- Chiếm quyền điều khiển hệ thống
## Cách tấn công ##
