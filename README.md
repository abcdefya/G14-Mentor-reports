
# **BÁO CÁO TIẾN ĐỘ DỰ ÁN AI AGENT LUYỆN NÓI TIẾNG ANH**
Nhóm 14 - VinAI
## **1. Tổng quan dự án**

Dự án xây dựng một hệ thống AI Agent hỗ trợ người dùng luyện nói tiếng Anh thông qua hội thoại tương tác, tích hợp công nghệ xử lý ngôn ngữ tự nhiên (LLM), tổng hợp giọng nói (TTS), và đánh giá phát âm. Hệ thống được thiết kế theo kiến trúc microservices, triển khai trên nền tảng cloud với khả năng mở rộng và tự động hóa cao.
## Link sản phẩm: https://vinai-speaking-agent.duckdns.org/

Tài khoản test:
- username
```
vinai@vin.com
```
- password:
```
Vinai2A2026+
```
## **2. Các hạng mục đã hoàn thành**

### **2.1. Frontend – Backend (Core Features)**

Trong giai đoạn hiện tại, hệ thống đã hoàn thiện các chức năng cốt lõi phục vụ trải nghiệm người dùng:
* [x] Thêm tính năng học bằng thẻ flashcard **(NEW UPDATE - Chưa Deploy)**
![alt text](image-11.png)
![alt text](image-13.png)
* [x] Triển khai stack VEK (vector-elasticsearch-kibana) cho trace logs **(NEW UPDATE - Chưa Deploy)**
![alt text](image-14.png)
* [x] Triển khai Grafana + Prometheous cho theo dõi metrics (P50, P95, P99, TTFT, Latency, cost,...) **(NEW UPDATE - Chưa Deploy)**
![alt text](image-15.png)
* [x] Cá nhân hóa phản hồi theo user 
* [x] Kiểm soát nội dung người dùng 
![alt text](image-10.png)

* [x] Hạn chế hallucination và phản hồi không phù hợp từ LLM 
* [x] Lấy lịch sử chat 
* [x] Xóa lịch sử chat *
![alt text](image-9.png)

* [x] Chấm điểm ngữ pháp 
![alt text](image-8.png)

* [x] Xây dựng hệ thống **xác thực người dùng** bao gồm đăng ký và đăng nhập.
* [x] Phát triển tính năng **lựa chọn chủ đề (topics)** để cá nhân hóa nội dung luyện tập.
* [x] Triển khai chức năng **giao tiếp trực tiếp với AI Agent**, hỗ trợ hội thoại theo thời gian thực.
* [x] Tích hợp dịch vụ **ElevenLabs** để xử lý chuyển đổi văn bản thành giọng nói (Text-to-Speech), trả về voice tự nhiên cho Agent.
* [x] Hiển thị **kết quả chấm điểm phát âm** của người dùng sau mỗi lượt nói.
* [x] Xây dựng cơ chế **lưu trữ lịch sử hội thoại** vào hệ thống database.
* [x] Lưu trữ file voice của người dùng vào hệ thống object storage **MinIO**.
* [x] Cho phép người dùng lựa chọn **giọng nói (nam/nữ)** cho Agent.
![alt text](image.png)
![alt text](image-1.png)
![alt text](image-2.png)


---

### **2.2. Hạ tầng (Infrastructure & DevOps)**


#### **Triển khai bằng Terraform trên Google Cloud Platform**

* Khởi tạo **01 cụm Kubernetes (K8s)**:
![alt text](image-3.png)
  * Quy mô: 2 nodes
  * Instance type: e2-standard-n2
  * Đảm bảo khả năng scale và orchestration container

#### **Hệ thống server hỗ trợ (Self-hosted services)**

Triển khai 4 server phục vụ DevOps pipeline: 2Gi Ram - 20Gi disk

* **Rancher Server**: Quản lý cluster Kubernetes, deployment, monitoring - https://vinai-rancher.duckdns.org/dashboard/
![alt text](image-5.png)
* **Harbor Server**: Private Docker Registry phục vụ lưu trữ images - https://vinai-registry.duckdns.org/
![alt text](image-4.png)
* **GitLab Server**: Quản lý source code và pipeline CI/CD - https://gitlab-vinai.duckdns.org/
* **GitLab Runner**: Thực thi pipeline tự động
    ![alt text](image-6.png)
#### **Database**

* Sử dụng **Cloud SQL** (GCP)
![alt text](image-7.png)
---

### **2.3. Quy trình CI/CD hiện tại**

Pipeline CI/CD đã được thiết lập với luồng tự động hóa như sau:

```
Push code lên GitHub
        ↓
Sync sang GitLab (chỉ nhánh main)
        ↓
GitLab Runner thực thi pipeline:
    - Chạy pytest
    - Nếu fail → dừng pipeline
    - Nếu pass → tiếp tục
        ↓
Phát hiện thay đổi (frontend / backend / deployment)
        ↓
Build Docker image (Kaniko)
        ↓
Push image lên Harbor
        ↓
Kubernetes pull image và deploy
```

Luồng này đảm bảo:

* Kiểm soát chất lượng code (test gating)
* Tự động hóa build & deploy
---

## **3. Kế hoạch giai đoạn tiếp theo**

### **3.1. Frontend**

* Thiết kế lại **UI/UX** nhằm cải thiện trải nghiệm người dùng:

  * Tối ưu giao diện hội thoại
  * Cải thiện feedback khi luyện nói
* Triển khai cơ chế **cache dữ liệu tin nhắn**:

  * Giảm latency
  * Tăng tốc độ hiển thị lịch sử chat

---

### **3.2. Backend**

* Phát triển thêm các API:
  * Phân quyền gói dùng (cơ bản, premium)


---

### **3.3. Hạ tầng & Giám sát hệ thống**

* Tối ưu lại pipeline CI/CD:

  * Giảm thời gian build
  * Tăng hiệu quả detect thay đổi
