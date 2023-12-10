## CSSRF là gì? ##
Cross-site request forgery (CSRF) là một lỗ hổng bảo mật web cho phép kẻ tấn công lợi dụng người dùng thực hiện những hành động không mong muốn. Để tấn công CSRF cần các điều kiện sau:
- Hành động có liên quan: Cần một hành động trong ứng dụng mà kẻ tấn công có lí do lợi dụng người dùng thực thi. Có thể là hành động đặc quyền (như sửa đổi quyền của người dùng khác) hay hành động liên quan đến trực tiếp người dùng(như thay đổi mật khẩu)
- Xử lý phiên dựa trên cookie: Khi người dùng gửi các HTTP request và ứng dụng chỉ dựa vào cookie session để xác định người dùng nào đã gửi request
- Không có các tham số request khó dự đoán: Các request thực hiện hành động không chứa bất kỳ tham số nào mà kẻ tấn công không thể xác định hoặc dự đoán
