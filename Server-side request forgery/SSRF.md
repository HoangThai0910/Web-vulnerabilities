## SSRF là gì? ##
Server-side request forgery(SSRF), hay còn gọi là Tấn công giả mạo yêu cầu từ phía máy chủ, là một lỗ hổng an ninh trên web cho phép kẻ tấn công khiến ứng dụng phía máy chủ thực hiện các request đến một domain tùy ý của kẻ tấn công.
Trong SSRF, kẻ tấn công có thể khiến máy chủ kết nối đến các dịch vụ chỉ có thể truy cập từ bên trong hạ tầng tổ chức. Ngoài ra, họ có thể buộc máy chủ kết nối đến các hệ thống bên ngoài tùy ý. Điều này có thể dẫn đến rò rỉ thông tin nhạy cảm, như thông tin xác thực.
## Hậu quả ##
Một cuộc tấn công SSRF thành công thường dẫn đến các hành động hoặc quyền truy cập không được phép vào dữ liệu trong tổ chức. Điều này có thể xảy ra trong ứng dụng có lỗ hổng, hoặc trên các hệ thống back-end khác mà ứng dụng có thể giao tiếp. Trong một số tình huống, lỗ hổng SSRF có thể cho phép kẻ tấn công thực hiện thực thi lệnh tùy ý (command execution)
