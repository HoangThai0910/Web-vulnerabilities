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
Giả sử ta có form đăng nhập như sau:

![image](https://github.com/HoangThai0910/Web-vulnerabilities/assets/108949637/b8776662-57b0-41af-bcae-132907e8788c)

Khi người dùng nhập thông tin đăng nhập của họ và nhấn vào nút submit, thông tin sẽ được gửi lại cho máy chủ web, ở đó nó sẽ được kết hợp với một lệnh SQL. Nếu tồn tại người dùng với userid và password sẽ cho phép đăng nhập. Nếu không tồn tại người dùng với userid và password sẽ từ chối đăng nhập và báo lỗi. Ví dụ khi ta nhập `admin` làm userid và `123` làm password sẽ chuyển thành lệnh sau:

`SELECT * FROM user WHERE userid=’admin’ AND password=’123’`

Nhưng sẽ ra sao nếu ta nhập như sau:

![image](https://github.com/HoangThai0910/Web-vulnerabilities/assets/108949637/b47a2e43-0749-4cd2-822b-9d8a41297b05)

Kết quả trả về là thông tin của admin mà không cần mật khẩu chính xác

![image](https://github.com/HoangThai0910/Web-vulnerabilities/assets/108949637/61731106-d833-4678-bb78-9552770e0807)

Nguyên nhân là vì câu truy vấn đã trở thành `SELECT * FROM user WHERE userid=’admin’-- AND password=’abcd’`. Phần kiểm tra mật khẩu đã bị loại bỏ bởi ký tự "--" (phần lệnh sau ký hiệu -- được coi là comment và sẽ không được thực hiện)

Đây chỉ là một trong những hình thức đơn giản của SQL injection. Ngoài ra kẻ tấn công có thể sử dụng các lệnh như INSERT, DROP, DELETE, UPDATE để chèn, sửa hoặc xóa dữ liệu

Trong các trường hợp nghiêm trọng hơn, khi kết nối với máy chủ cơ sở dữ liệu được thực hiện thông qua tài khoản quản trị, kẻ tấn công có thể đi sâu vào hệ điều hành của máy chủ. Kẻ tấn công sử dụng lỗ hổng SQL injection để cùng lúc tạo tài khoản người dùng trên máy chủ bị xâm nhập, kích hoạt tính năng Remote Desktop, cài đặt thư mục chia sẻ SMB và tải phần mềm độc hại 
## Một số cách phòng chống ##
- Tham số hóa truy vấn: Ví dụ nếu người dùng nhập `12345’ or 1=1 --` vào trường user_name thì truy vấn tham số hóa sẽ tìm kiếm trong CSDL để khớp với toàn chuỗi `12345’ or 1=1 -- `. Điều đó sẽ ngăn ngừa cấu trúc câu truy vấn bị thay đổi bởi bất kỳ đầu vào nào
- Sử dụng các thủ tục trong cơ sở dữ liệu (stored procedures): Các thủ tục được lưu trữ sẽ thêm một lớp bảo mật bổ sung vào CSDL bên cạnh sử dụng truy vấn tham số. Nó thực hiện giúp cho ứng dụng xử lý dữ liệu đầu vào dưới dạng thủ tục được xây dựng từ trước thay vì thực thi câu lệnh SQL trực tiếp
- Xác thực chuỗi input: Xác nhận là quá trình đảm bảo dữ liệu người dùng nhập vào là hợp lệ và vô hiệu hóa bất kỳ lệnh độc hại tiềm ẩn nào có thể được nhúng trong chuỗi nhập. Ví dụ, trong PHP, bạn có thể sử dụng mysqli_real_escape_string() để thoát các ký tự có thể thay đổi bản chất của lệnh SQL
- Phân định rõ ràng kiểu input: Với những kiểm soát đơn giản như kiểm soát về kiểu dữ liệu cũng có thể hạn chế đáng kể các cuộc tấn công. Ví dụ những trường dữ liệu kiểu số khi nhận đầu vào là các ký tự không phải số sẽ được loại bỏ
