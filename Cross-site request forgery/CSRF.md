## CSSRF là gì? ##
Cross-site request forgery (CSRF) là một lỗ hổng bảo mật web cho phép kẻ tấn công lợi dụng người dùng thực hiện những hành động không mong muốn. Để tấn công CSRF cần các điều kiện sau:
- Hành động có liên quan: Cần một hành động trong ứng dụng mà kẻ tấn công có lí do lợi dụng người dùng thực thi. Có thể là hành động đặc quyền (như sửa đổi quyền của người dùng khác) hay hành động liên quan đến trực tiếp người dùng(như thay đổi mật khẩu)
- Xử lý phiên dựa trên cookie: Khi người dùng gửi các HTTP request và ứng dụng chỉ dựa vào cookie session để xác định người dùng nào đã gửi request
- Các tham số request dễ dự đoán: Các request thực hiện hành động không chứa bất kỳ tham số nào mà kẻ tấn công không thể xác định hoặc dự đoán
## Cách tấn công ##
Giả sử một ứng dụng web cho phép người dùng thay đổi email cho tài khoản của họ. Khi người dùng thực hiện hành động này, họ sẽ gửi đi một HTTP request như sau:
```
POST /email/change HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 30
Cookie: session=yvthwsztyeQkAPzeQ5gHgTvlyxHfsAfE

email=wiener@normal-user.com
```
Điều này đáp ứng các điều kiện cho tấn công CSRF:
- Hành động thay đổi email sẽ có lợi cho kẻ tấn công. Hacker sẽ thay đổi email của người dùng thành email của họ rồi thực hiện reset password, sau đó chiếm tài khoản của người dùng
- Ứng dụng web này sử dụng cookie để xác định người dùng nào đã gửi request. Không có token hay cơ chế xác thực nào khác
- Kẻ tấn công dễ dàng xác định được giá trị của các tham số trong request để thực thi hành động

Hacker sẽ lừa trình duyệt của người dùng gửi đi các câu lệnh http đến ứng dụng web. Điều đó có thể thực hiện bằng cách chèn mã độc hay link đến trang web mà người dùng đã được chứng thực. Trong trường hợp phiên làm việc của người dùng chưa hết hiệu lực thì các câu lệnh trên sẽ được thực hiện với quyền chứng thực của người sử dụng. Kẻ tấn công có thể tạo một web page chứa đoạn HTML sau:
```
<html>
    <body>
        <form action="https://vulnerable-website.com/email/change" method="POST">
            <input type="hidden" name="email" value="pwned@evil-user.net" />
        </form>
        <script>
            document.forms[0].submit();
        </script>
    </body>
</html>
```
Sau khi người dùng bấm vào trang web giả mạo của hacker, những điều dưới đây sẽ xảy ra:
- Trang web của hacker sẽ gửi một HTTP request đến trang web bị lỗ hổng
- Nếu người dùng đã đăng nhập sẵn vào web dính lỗ hổng, trình duyệt của họ sẽ tự động đưa cookie chứa phiên làm việc của họ vào request
- Trang web có lỗ hổng vẫn xử lý như bình thường, coi request đó là của nạn nhân và thực hiện đổi email của họ
## Một số cách phòng chống ##
- CSRF tokens: CSRF token là giá trị duy nhất, bí mật và không thể dự đoán được tạo bởi phía server và chia sẻ với client. Khi thực hiện một hành động nhạy cảm, client phải đưa CSRF token chính xác trong request. Điều này khiến kẻ tấn công rất khó để tạo một request hợp lệ dựa trên nạn nhân
- SameSite cookie: SameSite là một cơ chế bảo mật trình duyệt xác định khi nào cookie của một trang web được đưa vào trong các request xuất phát từ các trang web khác. Do yêu cầu để thực hiện các hành động nhạy cảm thường đòi hỏi session cookie đã được xác thực, các ràng buộc của SameSite có thể ngăn chặn kẻ tấn công khởi động những hành động này qua các trang web khác. Đến năm 2021, Chrome mặc định thực hiện các ràng buộc SameSite theo chuẩn Lax.
- Kiểm tra Referer: Một số ứng dụng sử dụng HTTP Referer header để phòng chống tấn công CSRF, thường bằng cách kiểm tra các request bắt nguồn từ đâu. Điều này thường ít hiệu quả hơn so với việc kiểm tra CSRF token
