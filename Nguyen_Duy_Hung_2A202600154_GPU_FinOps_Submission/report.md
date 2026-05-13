# BÁO CÁO PHÂN TÍCH GPU FINOPS & COST OPTIMIZATION

**Sinh viên:** Nguyen Duy Hung  
**MSSV:** 2A202600154  
**Ngày thực hiện:** 13/05/2026

---

## 1. Giới thiệu
Báo cáo này tóm tắt kết quả thực hiện Lab Day 25 về chủ đề **GPU FinOps**. Mục tiêu chính của bài lab là làm quen với các kỹ thuật quản lý chi phí GPU, giám sát tài nguyên thời gian thực và áp dụng các chiến lược tối ưu hóa (Spot instances, AMP, Autoscaling) để giảm thiểu lãng phí và tối đa hóa hiệu năng trên mỗi đơn vị chi phí.

## 2. Phân tích kết quả thực nghiệm

### 2.1. Giám sát Cluster giả lập (Part 1-7)
Trong môi trường giả lập (Mock Cluster), chúng tôi đã quản lý hệ thống gồm 5 nodes với các loại GPU khác nhau (T4, A100, V100). Các kết quả chính bao gồm:
- **Lãng phí tài nguyên:** Phân tích cho thấy tỷ lệ lãng phí (Waste %) thường cao ở các cấu hình On-demand không có Autoscaling.
- **Tối ưu chi phí:** Việc áp dụng **Spot Instances** giúp giảm chi phí tới **~70%**. Hệ thống Spot Manager đã xử lý tốt các tình huống bị thu hồi (preemption) và tính toán chính xác số tiền tiết kiệm được.
- **Autoscaling:** Chiến lược Autoscaling dựa trên ngưỡng Utilization (70%) đã giúp hệ thống tự động điều chỉnh số lượng GPU, ngăn chặn việc trả tiền cho các GPU nhàn rỗi.

### 2.2. Huấn luyện trên GPU thực tế (Part 8)
Thực hiện huấn luyện mô hình ResNet-18 trên tập dữ liệu CIFAR-10 sử dụng GPU Tesla T4 (Kaggle). Kết quả so sánh giữa hai phương pháp:
- **FP32 (Baseline):** Thời gian huấn luyện ~120s, chi phí ước tính $0.0116.
- **Mixed Precision (AMP):** 
  - Thời gian huấn luyện giảm đáng kể (giảm ~30-40%).
  - Tiết kiệm chi phí năng lượng và chi phí thuê GPU.
  - Hiệu suất (Util) và nhiệt độ GPU được duy trì ổn định, không có lỗi xảy ra trong quá trình train.

### 2.3. Phân tích FinOps nâng cao (Part 8.5)
Áp dụng các kỹ thuật phân tích sâu:
- **Multi-GPU Scaling:** Qua phân tích, cấu hình 4 GPU thường mang lại điểm cân bằng tốt nhất giữa tốc độ và chi phí (Efficiency ~92%).
- **Dự báo (Forecasting):** Thiết lập mô hình dự báo chi phí dự án theo từng giai đoạn (Data Prep, Training, Tuning) với khoảng tin cậy (Confidence Intervals), giúp quản lý ngân sách chủ động hơn.
- **Chiến lược Challenge:** Đối với bài toán Fine-tuning LLM ngân sách $5000, chúng tôi đã thiết kế chiến lược kết hợp **4x A100 + AMP + Spot Instances**, giúp tiết kiệm ~85% chi phí so với phương án gốc, đưa chi phí dự kiến về mức ~$888.

## 3. Kết luận và Học hỏi
Thông qua bài Lab, tôi đã nắm vững các khái niệm cốt lõi của GPU FinOps:
1. **Visibility:** Phải nhìn thấy được chi phí theo thời gian thực (Dashboard).
2. **Optimization:** Luôn ưu tiên AMP cho Deep Learning và sử dụng Spot Instances cho các tác vụ không yêu cầu uptime 100%.
3. **Accountability:** Mỗi GPU được cấp phát đều phải đi kèm với báo cáo hiệu quả sử dụng.

---
*Tài liệu đính kèm: Notebook thực thi và bộ Dashboard visualization.*
