# Lab 02 – Problem Scan
**Tên nhóm:** VinUni Tour

**Họ và tên:** Lê Bình Nguyên

**MSSV:** 2A202601659

---

# Phase 1 – SCAN

| # | Subsidiary | Lens             | Mô tả bài toán thực tế                                                                                                                            |
| - | ---------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1 | Xanh SM    | Time-consuming   | Nhân viên CSKH phải đọc hàng trăm phản hồi của khách mỗi ngày để xác định mức độ nghiêm trọng và chuyển đúng bộ phận xử lý.                       |
| 2 | VinFast    | AI-upgrade       | Trung tâm dịch vụ tiếp nhận mô tả lỗi xe bằng tiếng Việt từ khách hàng, kỹ thuật viên phải đọc và phân loại thủ công trước khi đặt lịch sửa chữa. |
| 3 | Vinhomes   | Repetitive       | Ban quản lý nhận rất nhiều phản ánh của cư dân (thang máy, điện, nước, vệ sinh...) và phải tự phân loại trước khi giao việc.                      |
| 4 | Vinmec     | Time-consuming   | Bác sĩ mất nhiều thời gian viết tóm tắt hồ sơ xuất viện từ bệnh án điện tử.                                                                       |
| 5 | Vinpearl   | Stakeholder Pain | Khách sạn nhận nhiều đánh giá trên Google, Booking và Agoda nhưng việc tổng hợp ý kiến còn thực hiện thủ công nên phản hồi chậm.                  |

---

# Phase 2 – QUICK ASSESS

## QUICK PROBLEM CARD #1

### Bài toán

Tự động phân loại phản hồi của khách hàng Xanh SM.

**Công ty:** Xanh SM

### Ai đang gặp khó khăn?

Nhân viên Chăm sóc khách hàng (Customer Support).

### Workflow hiện tại

1. Nhận phản hồi từ ứng dụng hoặc hotline.
2. Đọc toàn bộ nội dung khách hàng gửi.
3. Xác định loại vấn đề.
4. Chuyển ticket đến bộ phận phụ trách.
5. Theo dõi kết quả xử lý.

### Bottleneck

Bước 2 và 3.

Mỗi phản hồi mất khoảng **5–8 phút** để đọc và phân loại.

### AI có thể hỗ trợ

LLM đọc nội dung phản hồi, xác định chủ đề (tài xế, thanh toán, ứng dụng, thái độ phục vụ...), đánh giá mức độ ưu tiên và đề xuất bộ phận xử lý.

### Success Metric

* Giảm thời gian xử lý từ **6 phút xuống dưới 1 phút**.
* Độ chính xác phân loại đạt **95%**.

### Quick Architecture

☑ LLM Feature

---

## QUICK PROBLEM CARD #2

### Bài toán

Hỗ trợ phân loại mô tả lỗi xe VinFast.

**Công ty:** VinFast

### Ai đang gặp khó khăn?

Nhân viên cố vấn dịch vụ.

### Workflow hiện tại

1. Khách hàng mô tả lỗi.
2. Nhân viên đọc mô tả.
3. Xác định nhóm lỗi.
4. Đặt lịch sửa chữa.

### Bottleneck

Phân tích mô tả lỗi bằng ngôn ngữ tự nhiên.

Khoảng **10 phút/khách**.

### AI có thể hỗ trợ

LLM chuẩn hóa mô tả, dự đoán nhóm lỗi và gợi ý hạng mục kiểm tra.

### Success Metric

* Giảm thời gian xuống **2 phút**.
* Độ chính xác dự đoán đạt **90%**.

### Quick Architecture

☑ LLM Feature

---

## QUICK PROBLEM CARD #3

### Bài toán

Tự động phân loại phản ánh cư dân.

**Công ty:** Vinhomes

### Ai đang gặp khó khăn?

Nhân viên Ban quản lý tòa nhà.

### Workflow hiện tại

1. Nhận phản ánh.
2. Đọc nội dung.
3. Xác định loại sự cố.
4. Chuyển cho đội kỹ thuật hoặc CSKH.

### Bottleneck

Đọc và phân loại hàng trăm phản ánh mỗi ngày.

Khoảng **5 phút/ticket**.

### AI có thể hỗ trợ

LLM tự động phân loại, đánh dấu mức độ khẩn cấp và đề xuất bộ phận xử lý.

### Success Metric

* Giảm thời gian xuống **dưới 1 phút**.
* Độ chính xác phân loại đạt **97%**.

### Quick Architecture

☑ LLM Feature
