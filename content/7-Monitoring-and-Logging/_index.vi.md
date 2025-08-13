---
title : "Monitoring và Logging"
date : 2025-08-12
weight : 7
chapter : true
pre : " <b> 7. </b> "
---

**Nội dung:**
- [Tổng quan](#tổng-quan)
- [Kích hoạt Amazon CloudWatch Logs](#kích-hoạt-amazon-cloudwatch-logs)
- [Kích hoạt Container Insights](#kích-hoạt-container-insights)
- [Kiểm tra Log và Metrics](#kiểm-tra-log-và-metrics)
- [Tích hợp Cảnh báo (Alarms)](#tích-hợp-cảnh-báo-alarms)

---

#### Tổng quan

Monitoring và Logging giúp bạn theo dõi tình trạng hoạt động của ứng dụng và hạ tầng.  
Trong workshop này, chúng ta sẽ sử dụng:
- **Amazon CloudWatch Logs** để lưu trữ và phân tích log từ container.
- **CloudWatch Container Insights** để theo dõi CPU, Memory, Network và Storage usage.
- **CloudWatch Alarms** để nhận thông báo khi có vấn đề.

---

#### Kích hoạt Amazon CloudWatch Logs

1. Truy cập **ECS Console** → **Cluster** → Chọn service của bạn.
2. Trong **Task Definition**, kiểm tra cấu hình log driver:
   - Chọn `awslogs` làm log driver.
   - Đặt tên Log Group: `/ecs/fargate-workshop`
   - Region: `us-east-1` (hoặc region của bạn).
3. Lưu và redeploy service.
![Amazon CloudWatch Logs](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/07/01.png)
---

#### Kích hoạt Container Insights

1. Truy cập **CloudWatch Console** → **Container Insights**.
2. Chọn **Enable** cho ECS Cluster của bạn.
3. Container Insights sẽ tự động thu thập:
   - CPUUtilization
   - MemoryUtilization
   - NetworkRxBytes / NetworkTxBytes
   - StorageReadBytes / StorageWriteBytes

---

#### Kiểm tra Log và Metrics

1. Vào **CloudWatch Console** → **Logs** → Chọn Log Group `/ecs/fargate-workshop`.
2. Xem log output từ các container (stdout, stderr).
3. Vào **CloudWatch Metrics** → ECS → Cluster Metrics để xem thống kê CPU, RAM, Network.
4. Lọc theo từng service hoặc từng task để phân tích chi tiết.

---

#### Tích hợp Cảnh báo (Alarms)

1. Vào **CloudWatch Console** → **Alarms** → **Create alarm**.
2. Chọn metric, ví dụ: `ECS/ContainerInsights – CPUUtilization`.
3. Đặt ngưỡng cảnh báo, ví dụ: CPU > 80% trong 5 phút.
4. Chọn SNS Topic để nhận email khi cảnh báo kích hoạt.
5. Lưu và kích hoạt alarm.

---

**Kết quả mong đợi:**
- Bạn có thể xem log của từng container để debug.
- Theo dõi được hiệu suất ứng dụng theo thời gian thực.
- Nhận thông báo khi hệ thống gặp sự cố hoặc vượt ngưỡng tài nguyên.

---
