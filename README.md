# Session 02 - Bài 2: Khởi tạo User thường và thiết lập đặc quyền quản trị (Sudoers Configuration)

## 1. Mục tiêu
- Tạo tài khoản người dùng thường (`devops`) trên Ubuntu Server tuân thủ nguyên tắc đặc quyền tối thiểu (Least Privilege).
- Cấp đặc quyền quản trị thông qua nhóm `sudo` và sao chép cấu hình SSH Key để đăng nhập trực tiếp bằng tài khoản `devops`.

## 2. Các bước thực hiện

### Bước 1: Tạo user `devops` trên Droplet
Chạy lệnh với quyền `root`:
```bash
sudo adduser devops
```

### Bước 2: Thêm user `devops` vào nhóm `sudo`
```bash
sudo usermod -aG sudo devops
```

### Bước 3: Sao chép SSH Key và phân quyền chính xác
Sử dụng lệnh `rsync` để sao chép thư mục `.ssh` từ `root` sang `devops` và tự động cập nhật owner:
```bash
sudo rsync --archive --chown=devops:devops ~/.ssh /home/devops/
```

Kiểm tra phân quyền an toàn:
```bash
sudo chmod 700 /home/devops/.ssh
sudo chmod 600 /home/devops/.ssh/authorized_keys
```

## 3. Kết quả kiểm tra (Terminal Log)

### Lệnh 1: Kiểm tra kết nối SSH bằng tài khoản `devops`
```bash
ssh -i ~/.ssh/id_ed25519 devops@128.199.200.100
```
**Kết quả:**
```text
devops@ubuntu-devops-server:~$ whoami
devops
```

### Lệnh 2: Kiểm tra quyền quản trị với `sudo whoami`
```bash
devops@ubuntu-devops-server:~$ sudo whoami
[sudo] password for devops:
root
```
Thực thi lệnh với quyền sudo thành công mà không gặp bất kỳ lỗi phân quyền nào.
