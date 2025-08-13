---
title : "Chuẩn bị môi trường"
date : 2025-08-11
weight : 2
chapter : true
pre : " <b> 2. </b> "
---

**Nội dung:**
- [Danh sách yêu cầu](#danh-sách-yêu-cầu)
- [Thiết lập tài khoản AWS](#thiết-lập-tài-khoản-aws)
- [Cài đặt AWS CLI](#cài-đặt-aws-cli)
- [Cài đặt Docker Desktop](#cài-đặt-docker-desktop)
- [Cài đặt Git](#cài-đặt-git)
- [Thiết lập Code Editor](#thiết-lập-code-editor)
- [Kiểm tra cài đặt](#kiểm-tra-cài-đặt)
- [Ứng dụng mẫu](#ứng-dụng-mẫu)

---

#### Danh sách yêu cầu

Trước khi bắt đầu workshop này, hãy đảm bảo bạn có những thứ sau:

- Tài khoản AWS với quyền Administrator
- AWS CLI được cài đặt và cấu hình
- Docker Desktop được cài đặt và chạy
- Git được cài đặt
- Text editor (khuyến nghị VS Code)
- Hiểu biết cơ bản về containers và các dịch vụ AWS

**Quan trọng**: Workshop này sẽ tạo ra các tài nguyên AWS có thể phát sinh chi phí. Chi phí ước tính: $5-10 USD cho toàn bộ workshop. Nhớ dọn dẹp tài nguyên khi hoàn thành!

---

#### Thiết lập tài khoản AWS

### Yêu cầu tài khoản AWS

Bạn cần một tài khoản AWS với những điều sau:
- Thẻ tín dụng hợp lệ được đính kèm để thanh toán
- Số điện thoại đã được xác minh
- Quyền truy cập Administrator hoặc quyền tương đương

### Ước tính chi phí

| Dịch vụ | Chi phí ước tính/Giờ | Ghi chú |
|---------|-------------------|-------|
| **Fargate Tasks** | $0.04 - $0.08 | 3 services, 0.25 vCPU, 0.5GB RAM mỗi cái |
| **Application Load Balancer** | $0.025 | Giá chuẩn ALB |
| **Aurora Serverless v2** | $0.06 - $0.12 | Phụ thuộc vào sử dụng |
| **NAT Gateway** | $0.045 | Phí xử lý dữ liệu |
| **CloudWatch Logs** | $0.50/GB | Logs tối thiểu dự kiến |
| **ECR Storage** | $0.10/GB/tháng | Container images |
| **Tổng ước tính** | **~$0.25/giờ** | **~$1.00 cho workshop 4 giờ** |

### Tạo IAM User 

Thay vì sử dụng tài khoản root, hãy tạo một IAM user chuyên dụng cho workshop này.

- Đăng nhập AWS Console → IAM
![Đăng nhập AWS](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/02/01.png)

- Users → Create user
![Đăng nhập AWS](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/02/01.png)

- Chi tiết user:
   - User name: `fargate-workshop-user`
   - Cung cấp quyền truy cập user vào AWS Management Console

![Chi tiết user](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/02/03.png)

- Thiết lập quyền:
   - Attach policies directly
   - Tìm kiếm và chọn: `AdministratorAccess`
![Thiết lập quyền](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/02/04.png)

- Review và tạo
![Review và tạo](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/02/05.png)

- Download credentials hoặc copy Access Key ID và Secret Access Key
![download](https://trungquangnguyeen.github.io/Serverless-Container-Orchestration/images/02/06.png)

---

#### Cài đặt AWS CLI


- Download AWS CLI MSI installer: https://awscli.amazonaws.com/AWSCLIV2.msi 

- Chạy file MSI và làm theo hướng dẫn cài đặt

- Mở Command Prompt mới và kiểm tra: aws --version
Kết quả mong đợi: aws-cli/2.15.30 Python/3.11.8 Windows/10 exe/AMD64 prompt/off

- Kiểm tra cài đặt
aws --version
 
---

#### Cấu hình AWS CLI

- aws configure

AWS Access Key ID [None]: AKIA...
AWS Secret Access Key [None]: wJalrXUt...
Default region name [None]: us-east-1
Default output format [None]: json


Kiểm tra cấu hình AWS CLI

- Kiểm tra danh tính của bạn
aws sts get-caller-identity

- Kiểm tra quyền
aws s3 ls

- Kiểm tra region đã cấu hình: aws configure get region
Kết quả mong đợi của aws sts get-caller-identity

{
    "UserId": "AIDACKCEVSQ6C2EXAMPLE",
    "Account": "123456789012",
    "Arn": "arn:aws:iam::123456789012:user/fargate-workshop-user"
}

---

#### Cài đặt Docker Desktop
- Download Docker Desktop: https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe

- Chạy installer và làm theo hướng dẫn thiết lập

- Khởi động lại máy tính sau khi cài đặt

- Khởi động Docker Desktop từ Start Menu

- Kiểm tra cài đặt:
docker --version
docker run hello-world

---

#### Kiểm tra Docker hoạt động

- Kiểm tra Docker version: docker --version

- Test Docker với hello-world: docker run hello-world

- Kiểm tra Docker Compose: docker compose version
Kết quả mong đợi của Docker hello-world

Hello from Docker!

---

#### Cài đặt Git
- Download Git: https://git-scm.com/download/win
Chạy installer với các thiết lập mặc định

- Kiểm tra cài đặt: git --version
Cấu hình Git cơ bản

- Thiết lập tên và email
git config --global user.name "Tên của bạn"
git config --global user.email "email@example.com"

- Kiểm tra cấu hình
git config --list

---

##### Thiết lập Code Editor
- VS Code (Khuyến khích) Download VS Code: https://code.visualstudio.com/

- Cài đặt các extensions hữu ích:

AWS Toolkit - Tích hợp dịch vụ AWS
Docker - Quản lý Docker container
YAML - Syntax highlighting cho YAML
JSON - Định dạng JSON
GitLens - Tính năng Git nâng cao
Python - Hỗ trợ Python
Go - Hỗ trợ Go language

---

#### Kiểm tra cài đặt
Chạy các lệnh sau để đảm bảo mọi thứ đã được cài đặt đúng cách:

- AWS CLI
aws --version
aws sts get-caller-identity

- Docker
docker --version
docker run hello-world

- Git
git --version

- Kiểm tra AWS region
aws configure get region

 Nếu tất cả các lệnh trên chạy thành công, bạn đã sẵn sàng cho workshop!
