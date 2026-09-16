# Triển khai Pi-hole từ GitHub lên máy chủ ngoài PC

Mục tiêu của cấu hình này là giữ source trên GitHub và chạy Pi-hole trên một máy chủ Linux/VPS riêng, không dùng CMD, Docker hay dung lượng ổ C/D của PC Windows.

## Cấu hình đã chuẩn bị

- Image: `pihole/pihole:2026.07.2`
- DNS: TCP/UDP port `53`
- Web quản trị: HTTPS port `8443`
- Múi giờ: `Asia/Ho_Chi_Minh`
- Dữ liệu Pi-hole được lưu bằng Docker volume trên máy chủ ngoài PC.

## Yêu cầu bắt buộc trước khi chạy thật

Máy chủ phải có Docker + Docker Compose và phải có firewall của nhà cung cấp/VPS. Không được mở DNS port 53 cho toàn Internet vì sẽ biến máy chủ thành open resolver.

Hãy chỉ cho phép TCP/UDP 53 từ IP hoặc dải mạng của bạn. Web quản trị 8443 cũng nên chỉ cho phép từ IP của bạn.

## Biến môi trường

Tạo `.env` trên máy chủ từ `.env.example` và đặt mật khẩu quản trị mạnh:

```env
PIHOLE_ADMIN_PASSWORD=mat-khau-rat-manh
```

Không commit `.env` thật lên GitHub.

## Chạy trên máy chủ

Trong thư mục chứa `compose.yaml` và `.env`:

```bash
docker compose pull
docker compose up -d
```

Sau khi chạy, DNS dùng địa chỉ IP của VPS trên port 53. Giao diện quản trị dùng `https://IP_VPS:8443/admin/`.

## Lưu ý cho điện thoại nối Wi-Fi do PC phát

Nếu DNS của điện thoại/hotspot được trỏ tới IP VPS thì truy vấn DNS có thể đi qua Pi-hole. Với máy chủ public, firewall vẫn phải giới hạn nguồn truy cập port 53. Nếu IP Internet của bạn thay đổi thường xuyên, nên dùng VPN/private network thay vì để port 53 public.
