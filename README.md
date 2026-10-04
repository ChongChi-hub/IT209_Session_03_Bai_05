# Bài 5: Thiết lập thư mục Web an toàn tại phân vùng hệ thống (/opt/)

### 1. Khởi tạo thư mục và phân quyền
Di chuyển/Tạo mới thư mục web root và logs:
```bash
sudo mkdir -p /opt/my-app/html/
sudo mkdir -p /opt/my-app/logs/
```

Đổi quyền sở hữu cho toàn bộ `/opt/my-app/` thành user `devops` và group `www-data` (Nginx):
```bash
sudo chown -R devops:www-data /opt/my-app
```

Phân quyền truy cập thư mục:
```bash
sudo chmod -R 755 /opt/my-app
# Cấp thêm quyền ghi (write) cho group www-data vào thư mục logs để Nginx ghi file
sudo chmod -R 775 /opt/my-app/logs
```

### 2. Cấu hình Nginx
Cấu hình Server Block ghi log riêng biệt tại /opt:
```bash
sudo nano /etc/nginx/sites-available/opt-web.conf
```
*(File cấu hình `opt-web.conf` đã được đính kèm trong thư mục này).*

### 3. Kiểm tra
*Lưu ý: Vì thẻ visa của em bị khoá nên không thể tạo droplet để kiểm chứng kết quả chạy thực.*

Kiểm tra phân quyền:
```bash
ls -la /opt/my-app/
```
Thử ghi mã nguồn web (đăng nhập bằng devops, không dùng sudo):
```bash
echo "Update Test" >> /opt/my-app/html/index.html
```
*(Ghi file sẽ thành công vì devops là owner)*.

Kiểm tra nội dung access logs:
```bash
tail -f /opt/my-app/logs/access.log
```
