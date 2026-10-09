# Bài 2: Quản trị Tường lửa UFW cho Cụm Dịch vụ Multi-port

## 1. Yêu cầu
Thiết lập tường lửa UFW để chặn tất cả kết nối đến, chỉ cho phép các cổng 22 (SSH), 80 (HTTP), 8082 (Spring Boot), và chặn cổng 3306 (MySQL).

## 2. Các lệnh thực hiện

1. **Khai báo các chính sách mặc định:**
   ```bash
   sudo ufw default deny incoming
   sudo ufw default allow outgoing
   ```

2. **Cho phép các cổng dịch vụ quy định:**
   ```bash
   sudo ufw allow 22/tcp
   sudo ufw allow 80/tcp
   sudo ufw allow 8082/tcp
   ```

3. **Kích hoạt UFW:**
   ```bash
   sudo ufw enable
   ```

## 3. Kết quả kiểm tra (Đầu ra lệnh ufw status verbose)

Thực hiện lệnh:
```bash
sudo ufw status verbose
```

**Kết quả:**
```
Status: active
Logging: on (low)
Default: deny (incoming), allow (outgoing), disabled (routed)
New profiles: skip

To                         Action      From
--                         ------      ----
22/tcp                     ALLOW IN    Anywhere
80/tcp                     ALLOW IN    Anywhere
8082/tcp                   ALLOW IN    Anywhere
22/tcp (v6)                ALLOW IN    Anywhere (v6)
80/tcp (v6)                ALLOW IN    Anywhere (v6)
8082/tcp (v6)              ALLOW IN    Anywhere (v6)
```
