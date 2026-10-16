https://github.com/homework-IT/devops-hackathon-de002-truongcongquy.git1. Tạo file cấu hình Nginx cho cổng 8083

sudo nano /etc/nginx/sites-available/mywebsite

server {
    listen 8083;
    listen [::]:8083;

    server_name _;

    root /var/www/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}

2. Kích hoạt cấu hình và tải lại Nginx

sudo ln -s /etc/nginx/sites-available/mywebsite /etc/nginx/sites-enabled/

sudo nginx -t

sudo systemctl reload nginx

3. Mở cổng 8083 trên tường lửa

sudo ufw allow 8083/tcp
sudo ufw reload

http://20.18.24.239:8083

sudo ss -tulpn | grep 8083 lệnh kiểm tra

1. Kiểm tra nhanh nội dung trang web (Local)
curl http://localhost:8083

2. Chỉ kiểm tra mã trạng thái HTTP (Status Code & Header)
curl -I http://localhost:8083

1. Đẩy file từ máy cá nhân lên server (Upload)
scp index.html username@103.xxx.xxx.xxx:/var/www/html/

Cách 1: Đưa vào thư mục con (chung port 8083)
sudo mkdir /var/www/html/duan2
sudo scp index.html user@IP:/var/www/html/duan2/

nếu đè lại index

scp index.html username@<IP_MÁY_CHỦ>:/var/www/html/index.html
scp index.html username@<IP_MÁY_CHỦ>:~/index.html
sudo cp -f ~/index.html /var/www/html/index.html
(Tùy chọn) Sao lưu file cũ trước khi đè phòng khi cần khôi phục:
sudo cp /var/www/html/index.html /var/www/html/index.html.bak
Mở file bằng trình soạn thảo:Bashsudo nano /var/www/html/index.html
Xóa nội dung cũ, dán mã HTML mới vào.Nhấn Ctrl + O Enter để lưu, rồi Ctrl + X để thoát.

của lê vũ
# 1. Tạo thư mục web
sudo mkdir -p /var/www/

# 2. Trao quyền sở hữu cho tài khoản của bạn
sudo chown -R USER:USER /var/www/project-vu

# 3. Phân quyền đọc cho Nginx
sudo chmod -R 755 /var/www/project-vu

# 4. Tạo file index.html (không cần dùng sudo nữa vì bạn đã là chủ thư mục)
cat << 'EOF' > /var/www/project-vu/index.html
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Project Vu</title>
</head>
<body style="text-align: center; margin-top: 50px; font-family: sans-serif;">
    <h1 style="color: #2563eb;">Dự Án: Project Vu</h1>
    <p>Chạy trực tiếp trên cổng <strong>1825</strong></p>
</body>
</html>
EOF

# 5. Cấu hình Nginx
sudo bash -c 'cat << "EOF" > /etc/nginx/sites-available/project_vu.conf
server {
    listen 1825;
    server_name 103.77.208.221;

    root /var/www/project-vu;
    index index.html;

    location / {
        try_files /index.html =404;
    }
}
EOF'

# 6. Kích hoạt và khởi động lại Nginx
sudo ln -sf /etc/nginx/sites-available/project_vu.conf /etc/nginx/sites-enabled/
sudo ufw allow 1825/tcp
sudo ufw reload
sudo nginx -t
sudo systemctl restart nginx


