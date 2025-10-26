# Bài 2-Web
2. NỘI DUNG BÀI TẬP:
   
2.1. Cài đặt Apache web server:
- Vô hiệu hoá IIS: nếu iis đang chạy thì mở cmd quyền admin để chạy lệnh: iisreset /stop
- Download apache server, giải nén ra ổ D, cấu hình các file:
  + D:\Apache24\conf\httpd.conf
    
    <img width="1421" height="573" alt="image" src="https://github.com/user-attachments/assets/e7b9e779-e837-440f-a5d9-d2b69598a1c0" />

  + D:Apache24\conf\extra\httpd-vhosts.conf

    <img width="1445" height="835" alt="image" src="https://github.com/user-attachments/assets/03af95c9-5633-4208-9278-acb1cd1b97ea" />

  để tạo website với domain: fullname.com
  code web sẽ đặt tại thư mục: `D:\Apache24\fullname` (fullname ko dấu, liền nhau)

  <img width="1078" height="67" alt="image" src="https://github.com/user-attachments/assets/3e12ad36-043d-44f0-b8c0-c77b812e6d18" />

- sử dụng file `c:\WINDOWS\SYSTEM32\Drivers\etc\hosts` để fake ip 127.0.0.1 cho domain này
  
- thao tác dòng lệnh trên file `D:\Apache24\bin\httpd.exe` với các tham số `-k install` và `-k start` để cài đặt và khởi động web server apache.
  
  <img width="1334" height="815" alt="image" src="https://github.com/user-attachments/assets/b9feb02c-3fb1-4c07-9677-416399364b5d" />

2.2. Cài đặt nodejs và nodered => Dùng làm backend:
- Cài đặt nodejs:
  + download file `https://nodejs.org/dist/v20.19.5/node-v20.19.5-x64.msi`  (đây ko phải bản mới nhất, nhưng ổn định)

  + cài đặt vào thư mục `D:\nodejs`
 
    <img width="1488" height="287" alt="image" src="https://github.com/user-attachments/assets/50e88fae-53e3-446c-a4dc-c7810e8a76c3" />

- Cài đặt nodered:
  + chạy cmd, vào thư mục `D:\nodejs`, chạy lệnh `npm install -g --unsafe-perm node-red --prefix "D:\nodejs\nodered"`

<img width="951" height="629" alt="image" src="https://github.com/user-attachments/assets/e11ccadd-880b-436d-ac5c-bcb869c0ac8f" />

  + download file: https://nssm.cc/release/nssm-2.24.zip
    giải nén được file nssm.exe
    copy nssm.exe vào thư mục `D:\nodejs\nodered\`

    <img width="1414" height="797" alt="image" src="https://github.com/user-attachments/assets/7291abd0-c54a-4c9b-8da7-84f11697685c" />

  + tạo file "D:\nodejs\nodered\run-nodered.cmd" với nội dung (5 dòng sau):
@echo off
REM fix path
set PATH=D:\nodejs;%PATH%
REM Run Node-RED
node "D:\nodejs\nodered\node_modules\node-red\red.js" -u "D:\nodejs\nodered\work" %*

<img width="1104" height="405" alt="image" src="https://github.com/user-attachments/assets/a2bbd355-87ee-4955-a8e4-88fa551ff3ea" />

  + mở cmd, chuyển đến thư mục: `D:\nodejs\nodered`
  + cài đặt service `a1-nodered` bằng lệnh: nssm.exe install a1-nodered "D:\nodejs\nodered\run-nodered.cmd"
 + chạy service `a1-nodered` bằng lệnh: `nssm start a1-nodered`

<img width="903" height="228" alt="image" src="https://github.com/user-attachments/assets/fcff9812-23ae-4eec-9e5c-36c9ff888d90" />

2.3. Tạo csdl tuỳ ý trên mssql (sql server 2022), nhớ các thông số kết nối: ip, port, username, password, db_name, table_name

<img width="1483" height="442" alt="image" src="https://github.com/user-attachments/assets/6e1305dd-94c1-46a0-8397-f8f23c21db8b" />

2.4. Cài đặt thư viện trên nodered:
- truy cập giao diện nodered bằng url: http://localhost:1880

  <img width="1864" height="869" alt="image" src="https://github.com/user-attachments/assets/59731a70-4f60-4212-8598-637669365826" />

- cài đặt các thư viện: node-red-contrib-mssql-plus, node-red-node-mysql, node-red-contrib-telegrambot, node-red-contrib-moment, node-red-contrib-influxdb, node-red-contrib-duckdns, node-red-contrib-cron-plus

<img width="936" height="695" alt="image" src="https://github.com/user-attachments/assets/ea1bd2ce-1547-462b-bfa4-8b0b5c82e15d" />

- Sửa file `D:\nodejs\nodered\work\settings.js` : 
  tìm đến chỗ adminAuth, bỏ comment # ở đầu dòng (8 dòng), thay chuỗi mã hoá mật khẩu bằng chuỗi mới
    adminAuth: {
        type: "credentials",
        users: [{
            username: "admin",
            password: "chuỗi_mã_hoá_mật_khẩu",
            permissions: "*"
        }]
    },   
   với mã hoá mật khẩu có thể thiết lập bằng tool: https://tms.tnut.edu.vn/pw.php

<img width="1266" height="895" alt="image" src="https://github.com/user-attachments/assets/21b57bbb-33d7-4c24-bc95-b428e809e8ec" />

- chạy lại nodered bằng cách: mở cmd, vào thư mục `D:\nodejs\nodered` và chạy lệnh `nssm restart a1-nodered`

<img width="728" height="139" alt="image" src="https://github.com/user-attachments/assets/356f8180-791d-47ad-8829-b1e50b8a001e" />

  khi đó nodered sẽ yêu cầu nhập mật khẩu mới vào được giao diện cho admin tại: http://localhost:1880
  <img width="1135" height="362" alt="image" src="https://github.com/user-attachments/assets/2513d17e-cc6e-45f3-b8e8-5303ec1a11a1" />

2.5. tạo api back-end bằng nodered:
- tại flow1 trên nodered, sử dụng node `http in` và `http response` để tạo api
- thêm node `MSSQL` để truy vấn tới cơ sở dữ liệu
- logic flow sẽ gồm 4 node theo thứ tự sau (thứ tự nối dây):

  <img width="1502" height="335" alt="image" src="https://github.com/user-attachments/assets/730a9d95-6ca1-444d-8dcc-d7605459de23" />

  1. http in  : dùng GET cho đơn giản, URL đặt tuỳ ý, ví dụ: /timkiem

     <img width="1907" height="847" alt="image" src="https://github.com/user-attachments/assets/798167c7-5e88-4c87-a125-a964d91ee3f3" />

  3. function : để tiền xử lý dữ liệu gửi đến

     <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/48bbecbc-a513-4a7b-a47d-c334375d6e9b" />

  5. MSSQL: để truy vấn dữ liệu tới CSDL, nhận tham số từ node tiền xử lý

     <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/9fbdf55f-a99b-421c-b811-ae1e2226a35f" />
     <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/15d94260-b2ec-4375-b474-a7318b258034" />


  7. http response: để phản hồi dữ liệu về client: Status Code=200, Header add : Content-Type = application/json
  có thể thêm node `debug` để quan sát giá trị trung gian.

    <img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/74930cea-a64c-4047-9ebe-1ef7871cd06c" />

- test api thông qua trình duyệt, ví dụ: http://localhost:1880/timkiem?q=thị

  <img width="1725" height="347" alt="image" src="https://github.com/user-attachments/assets/3306e5ff-9185-4794-87f2-8209ce8e5a91" />

2.6. Tạo giao diện front-end:
- html form gồm các file : index.html, fullname.js, fullname.css
  cả 3 file này đặt trong thư mục: `D:\Apache24\fullname`
  nhớ thay fullname là tên của bạn, viết liền, ko dấu, chữ thường, vd tên là Đỗ Duy Cốp thì fullname là `doduycop`
  khi đó 3 file sẽ là: index.html, doduycop.js và doduycop.css

  <img width="1443" height="445" alt="image" src="https://github.com/user-attachments/assets/476ed5fc-0ec1-4f83-a844-b81554be2a87" />

- index.html và fullname.css: trang trí tuỳ ý, có dấu ấn cá nhân, có form nhập được thông tin.

  <img width="1378" height="908" alt="image" src="https://github.com/user-attachments/assets/8710b1e7-edaf-4b51-86c0-3da58baa4caf" />

  <img width="1093" height="886" alt="image" src="https://github.com/user-attachments/assets/ddd66404-76ef-40a2-a5af-6e6eb5785a9e" />


- fullname.js: lấy dữ liệu trên form, gửi đến api nodered đã làm ở bước 2.5, nhận về json, dùng json trả về để tạo giao diện phù hợp với kết quả truy vấn của bạn.

<img width="1389" height="900" alt="image" src="https://github.com/user-attachments/assets/ded42596-184e-4031-bde4-8ff9454ce99f" />

- Kết quả :

<img width="1816" height="967" alt="image" src="https://github.com/user-attachments/assets/b3fd2486-3f53-4067-987e-f8a09d955ec1" />

2.7. Nhận xét bài làm của mình:
- đã hiểu quá trình cài đặt các phần mềm và các thư viện như nào?
+ Em đã nắm rõ các bước cài đặt và cấu hình các phần mềm cần thiết cho hệ thống.
+ Biết cách vô hiệu hóa IIS, cài đặt và cấu hình Apache Web Server để chạy website thông qua tên miền giả lập (ngothithuylinh.com) bằng file hosts.
+ Cài đặt thành công Node.js và Node-RED, biết cách cài đặt các thư viện mở rộng như: node-red-contrib-mssql-plus, node-red-node-mysql, node-red-contrib-moment, node-red-contrib-telegrambot,...
+ Hiểu cách cấu hình file settings.js để thiết lập bảo mật (adminAuth) và biết chạy Node-RED dưới dạng service bằng NSSM, cũng như khởi động lại Node-RED bằng lệnh nssm restart a1-nodered.
  
- đã hiểu cách sử dụng nodered để tạo api back-end như nào?
- đã hiểu cách frond-end tương tác với back-end ra sao?
