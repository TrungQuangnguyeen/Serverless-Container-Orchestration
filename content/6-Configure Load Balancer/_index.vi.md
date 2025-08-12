---
title : "Cấu hình Load Balancer"
date : 2025-08-12
weight : 6
chapter : true
pre : " <b> 6. </b> "
---

**Nội dung:**
- [Tổng quan](#tổng-quan)
- [Tạo Application Load Balancer (ALB)](#tạo-application-load-balancer-alb)
- [Tạo Target Groups](#tạo-target-groups)
- [Gán Services vào Target Groups](#gán-services-vào-target-groups)
- [Kiểm tra hoạt động](#kiểm-tra-hoạt-động)

---

#### Tổng quan

Application Load Balancer (ALB) sẽ giúp phân phối lưu lượng truy cập đến nhiều dịch vụ (microservices) chạy trên ECS Fargate.  
ALB hỗ trợ **path-based routing** và **host-based routing**, giúp điều hướng request tới đúng service.  

**Lợi ích:**
- Cân bằng tải giữa nhiều instance của service
- Đảm bảo high availability và fault tolerance
- Dễ dàng mở rộng khi có nhiều traffic

---

#### Tạo Application Load Balancer (ALB)

1. Vào **EC2 Console** → **Load Balancers** → **Create Load Balancer**
2. Chọn **Application Load Balancer**
3. Đặt tên: `fargate-alb`
4. Scheme: **Internet-facing** (nếu public)
5. IP address type: IPv4
6. Chọn VPC và **ít nhất 2 Subnet** ở 2 AZ khác nhau (Multi-AZ)
7. Security Group:
   - Cho phép HTTP (80) và HTTPS (443) nếu dùng SSL
8. Bỏ qua Target Group tại bước này (sẽ tạo sau)
9. Nhấn **Create Load Balancer**

---

#### Tạo Target Groups

1. Vào **EC2 Console** → **Target Groups** → **Create target group**
2. Loại target: **IP addresses**
3. Đặt tên: `api-target-group`
4. Protocol: HTTP, Port: 8080 (hoặc port service API)
5. Chọn VPC đã tạo trước đó
6. Health check path: `/health`
7. Tạo thêm target group cho Frontend:
   - Tên: `frontend-target-group`
   - Port: 80
   - Health check path: `/`

---

#### Gán Services vào Target Groups

1. Vào **ECS Console** → **Cluster** → **Service** → Chọn service cần cấu hình
2. Chỉnh sửa service → Phần **Load balancing**:
   - Chọn **Application Load Balancer**
   - Listener: HTTP 80 (hoặc HTTPS 443 nếu có SSL)
   - Chọn Target Group tương ứng (API → `api-target-group`, Frontend → `frontend-target-group`)
3. Lưu và deploy lại service

---

#### Kiểm tra hoạt động

1. Vào **EC2 Console** → **Load Balancers** → Chọn `fargate-alb`
2. Sao chép DNS name của ALB
3. Truy cập trên trình duyệt:
   - `http://<ALB_DNS>/` → hiển thị frontend
   - `http://<ALB_DNS>/api/health` → trả về thông báo API health check
4. Đảm bảo trạng thái Target Group là **healthy**

---
