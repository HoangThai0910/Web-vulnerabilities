## SSRF là gì? ##
Server-side request forgery(SSRF), hay còn gọi là Tấn công giả mạo yêu cầu từ phía máy chủ, là một lỗ hổng an ninh trên web cho phép kẻ tấn công khiến ứng dụng phía máy chủ thực hiện các request đến một domain tùy ý của kẻ tấn công.
Trong SSRF, kẻ tấn công có thể khiến máy chủ kết nối đến các dịch vụ chỉ có thể truy cập từ bên trong hạ tầng tổ chức. Ngoài ra, họ có thể buộc máy chủ kết nối đến các hệ thống bên ngoài tùy ý. Điều này có thể dẫn đến rò rỉ thông tin nhạy cảm, như thông tin xác thực.
## Hậu quả ##
Một cuộc tấn công SSRF thành công thường dẫn đến các hành động hoặc quyền truy cập không được phép vào dữ liệu trong tổ chức. Điều này có thể xảy ra trong ứng dụng có lỗ hổng, hoặc trên các hệ thống back-end khác mà ứng dụng có thể giao tiếp. Trong một số tình huống, lỗ hổng SSRF có thể cho phép kẻ tấn công thực hiện thực thi lệnh tùy ý (command execution)
## Một số cách tấn công SSRF phổ biến ##
### 1.Tấn công máy chủ cục bộ ###
Trong một cuộc tấn công SSRF vào máy chủ, kẻ tấn công khiến ứng dụng thực hiện một yêu cầu HTTP quay lại máy chủ đang lưu trữ ứng dụng, qua giao diện mạng loopback của nó. Thông thường, điều này liên quan đến việc cung cấp một URL với một tên máy chủ như 127.0.0.1 (địa chỉ IP trỏ đến loopback adapter) hoặc localhost

Ví dụ, ta có một ứng dụng mua sắm cho phép người dùng xem một sản phẩm có sẵn trong cửa hàng nào đó. Để cung cấp thông tin về hàng tồn kho, ứng dụng phải truy vấn các REST API back-end khác nhau. Điều này được thực hiện bằng cách truyền URL đến endpoint API back-end tương ứng thông qua một yêu cầu HTTP front-end. Khi người dùng xem trạng thái tồn kho cho một sản phẩm, trình duyệt của họ thực hiện yêu cầu sau:
```
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://stock.weliketoshop.net:8080/product/stock/check%3FproductId%3D6%26storeId%3D1
```
Máy chủ sẽ thực hiện một yêu cầu đến URL được chỉ định, lấy trạng thái tồn kho và trả về cho người dùng. Trong ví dụ này, kẻ tấn công có thể sửa đổi request để truy cập đến trang quản trị:
```
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://localhost/admin
```
Máy chủ lấy nội dung của URL /admin và trả về cho người dùng.

Thông thường người dùng sẽ không thể truy cập trang admin. Tuy nhiên, nếu yêu cầu đến URL /admin đến từ máy tính cục bộ, các kiểm soát truy cập bình thường sẽ được bỏ qua. Ứng dụng cấp quyền truy cập đầy đủ vào chức năng quản trị, vì request xuất phát từ một vị trí đáng tin cậy.

- Lab: [https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-localhost](https://portswigger.net/web-security/ssrf/lab-basic-ssrf-against-localhost)

Nguyên nhân tại sao ứng dụng hoạt động như vậy:
- Việc kiểm tra truy cập được thực hiện ở phía front-end thay vì phía server. Khi có kết nốt quay trở lại server, việc kiểm tra này bị bypass
- Vì mục đích nào đó, ứng dụng cho phép truy cập với quyền quản trị mà không cần đăng nhập, cho phép mọi người dùng từ máy local
- Giao diện quản trị có thể lắng nghe trên một cổng số khác với ứng dụng chính và có thể không thể tiếp cận trực tiếp bởi người dùng.
### 2.Tấn công SSRF các hệ thống back-end khác ###
Trong một số trường hợp, máy chủ ứng dụng có thể tương tác với các hệ thống back-end mà người dùng không thể tiếp cận trực tiếp. Những hệ thống này thường có địa chỉ IP riêng tư không thể định tuyến được. Các hệ thống back-end thường được bảo vệ bởi cấu trúc mạng, nên chúng thường có bảo mật yếu hơn. Trong nhiều trường hợp, các hệ thống back-end nội bộ chứa các chức năng nhạy cảm có thể được truy cập mà không cần xác thực từ bất kỳ ai có thể tương tác với hệ thống.

Như ví dụ trước, giả sử trang admin nằm ở `https://192.168.0.68/admin`. Kẻ tấn công có thể sửa request như sau để khai thác lỗ hổng SSRF, từ đó truy cập giao diện admin:
```
POST /product/stock HTTP/1.0
Content-Type: application/x-www-form-urlencoded
Content-Length: 118

stockApi=http://192.168.0.68/admin
```
## Vượt qua một số cách phòng thủ SSRF ##
### 1. SSRF với filters dựa trên blacklist ###
Một số ứng dụng chặn các input chứa hostname như `127.0.0.1` và `localhost` hoặc những URL nhạy cảm như `/admin`. Trong trường hợp này ta vẫn có thể bypass filter bằng cách sau:
- Sử dụng cách biểu diễn thay thế cho `127.0.0.1` như `2130706433`(giá trị biểu diễn của 127.0.0.1 trong hệ số nguyên 32bit), `017700000001`(dạng bát phân), `127.1`
- Đăng ký một tên miền mà phân giải thành địa chỉ `127.0.0.1`
- Obfuscate chuỗi bị chặn bằng cách URL encoding hoặc thay đổi kiểu chữ
- Cung cấp một URL mà bạn kiểm soát, có chức năng chuyển hướng đến URL mục tiêu. Hãy thử sử dụng các mã chuyển hướng khác nhau cũng như các giao thức khác nhau cho URL mục tiêu. Ví dụ, việc chuyển đổi từ một URL http: sang https: trong quá trình chuyển hướng đã được chứng minh là có thể phá vỡ một số bộ lọc chống SSRF
### 2. SSRF với filter sử dụng whitelist ###
Một số ứng dụng chỉ cho phép nhập các giá trị phù hợp với một danh sách các giá trị được phép (whitelist). Ta có thể bypass filter bằng cách khai thác sự không nhất quán trong quá trình phân tích cú pháp URL
- Thêm thông tin đăng nhập vào URL trước hostname bằng kí tự `@`. Ví dụ: `https://expected-host:fakepassword@evil-host`
- Sử dụng ký tự `#` để chỉ ra một đoạn URL: `https://evil-host#expected-host`
- Tận dụng cấu trúc tên miền DNS để đặt đầu vào cần thiết vào một tên miền DNS đầy đủ mà bạn kiểm soát. Ví dụ: `https://expected-host.evil-host`
- Sử dụng URL encode để gây nhầm lẫn cho code phân tích URL. Ngoài ra ta có thể double-encoding các ký tự
- Kết hợp tất cả các cách trên
