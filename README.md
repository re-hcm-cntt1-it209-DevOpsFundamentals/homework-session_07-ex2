# Bài 2: Cấu hình phân quyền Nhóm và sudoers bằng visudo

## Mục tiêu
- Tạo nhóm người dùng `developers` và thêm user `devuser1`.
- Phân quyền cho nhóm `developers` thực thi các lệnh quản trị dịch vụ (`systemctl restart nginx`, `systemctl status nginx`) mà không cần mật khẩu qua `sudoers`.

---

## 1. Khởi tạo Nhóm và Người dùng

```bash
# Tạo nhóm developers
sudo groupadd developers

# Tạo user devuser1 và thêm vào nhóm developers
sudo useradd -m -g developers -s /bin/bash devuser1
sudo passwd devuser1
```

---

## 2. Cấu hình Quy tắc Sudoers an toàn bằng `visudo`

Mở trình chỉnh sửa an toàn visudo:
```bash
sudo visudo -f /etc/sudoers.d/developers
```

Thêm quy tắc phân quyền giới hạn (Principle of Least Privilege):
```text
# Phân quyền cho nhóm developers thực thi lệnh systemctl liên quan tới nginx mà không cần password
%developers ALL=(ALL) NOPASSWD: /usr/bin/systemctl restart nginx, /usr/bin/systemctl status nginx, /usr/bin/systemctl reload nginx
```

---

## 3. Thử nghiệm và Kiểm tra Phân quyền

### Chuyển sang user `devuser1`:
```bash
su - devuser1
```

### Thử nghiệm chạy lệnh được cấp quyền (thành công không hỏi mật khẩu):
```bash
$ sudo systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since Wed 2026-10-07 10:00:00 UTC; 1h ago

$ sudo systemctl restart nginx
# Thực thi thành công mà không yêu cầu nhập password!
```

### Thử nghiệm chạy lệnh không được cấp quyền (bị từ chối):
```bash
$ sudo systemctl stop docker
[sudo] password for devuser1: 
Sorry, user devuser1 is not allowed to execute '/usr/bin/systemctl stop docker' as root on ubuntu-server.
```

---

## 4. Kết luận
- Việc sử dụng `visudo` giúp đảm bảo cú pháp tệp `/etc/sudoers.d/` chính xác, tránh nguy cơ khóa quyền root của hệ thống.
- Cấu hình chỉ định chính xác các lệnh cho phép giúp ngăn ngừa việc lạm dụng quyền hạn administrator.
