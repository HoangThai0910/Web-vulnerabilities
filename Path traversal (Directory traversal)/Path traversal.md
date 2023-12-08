## Path Traversal là gì? ##
Lỗ hổng web Path Traversal (còn được gọi là Directory Traversal) là một lỗ hổng bảo mật phổ biến trên các ứng dụng web. Lỗ hổng này cho phép kẻ tấn công truy cập vào các tệp tin và thư mục trên máy chủ web mà không được phép truy cập. Nó dẫn đến việc bị lộ thông tin nhạy cảm của ứng dụng như thông tin đăng nhập , một số file hoặc thư mục của hệ điều hành

Kẻ tấn công sử dụng ký tự dot-dot-slash (“../”) để truy cập vào các thư mục cha của thư mục hiện tại. Khi kẻ tấn công thành công trong việc truy cập vào các thư mục cha, họ có thể tiếp tục truy cập vào các thư mục khác và thậm chí có thể truy cập vào các tệp tin quan trọng trên máy chủ web.

![image](https://github.com/HoangThai0910/Web-vulnerabilities/assets/108949637/1cbe9545-dd46-44bc-9fd8-f7c33920dd76)

## Nguyên nhân ##
Lỗ hổng File Path Traversal xuất hiện trong các chức năng liên quan tới việc xử lý các File hoặc Directory. Lập trình viên cho phép người dùng hoặc ứng dụng đưa tên file hoặc đường dẫn mà không loại bỏ các dấu chấm và gạch chéo “/”, “.”. Các ký tự này sẽ giúp chúng ta di chuyển qua lại các thư mục.

Các chức năng Web dễ xuất hiện Path traversal: Load resources, Load page(?menu=home, ?page=home,...), Download file,...

## Một số cách phòng chống ##
- Lọc input của người dùng (Một số cách filter vẫn có thể bị hacker bypass)
- Sử dụng whitelist cho những giá trị được cho phép
- Tên file chỉ nên chứa ký tự chữ hoặc số, không nên chứa các ký tự đặc biệt
