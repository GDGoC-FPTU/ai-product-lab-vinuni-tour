# 02 - Deep Dive Report

# AI Product Scoping – Vin Smart Future

## Thông tin nhóm

**Tên nhóm:** .............................................................

| Họ và tên | MSSV |
|-----------|------|
| | |
| | |
| | |

---

# Bài toán được lựa chọn

**Xanh SM – Hỗ trợ điều phối sự cố xe điện hết pin bằng AI**

---

# 1. Current-State Workflow Mapping

## Quy trình hiện tại

```text
Tài xế gặp sự cố hết pin
            │
            ▼
1. Gọi đến tổng đài Xanh SM
            │
            ▼
2. Điều phối viên xác minh thông tin tài xế
            │
            ▼
3. Tra cứu vị trí GPS của xe
            │
            ▼
4. Tìm thủ công trạm sạc gần nhất
            │
            ▼
5. Kiểm tra loại cổng sạc phù hợp
            │
            ▼
6. Soạn hướng dẫn gửi tài xế
            │
            ▼
7. Nếu không thể tới trạm sạc
   → gọi xe cứu hộ
```

### Bottleneck

| Bước | Thời gian | Vấn đề |
|------|-----------|---------|
| Tra cứu trạm sạc | 5 phút | Nhiều hệ thống cần kiểm tra |
| Kiểm tra tương thích | 2 phút | Dễ nhầm loại cổng sạc |
| Soạn hướng dẫn | 5 phút | Lặp lại, mất thời gian |

**Tổng thời gian xử lý:** khoảng **15 phút/sự cố**

---

# 2. Problem Statement (6 Fields)

## 1. Actor / Operator

Điều phối viên (Dispatcher) tại Trung tâm Điều hành Xanh SM.

---

## 2. Current Workflow

Khi tài xế báo xe sắp hết pin hoặc không thể tiếp tục di chuyển, điều phối viên phải:

- xác minh vị trí,
- kiểm tra mức pin,
- tìm trạm sạc phù hợp,
- xác định khả năng xe có thể tới nơi,
- soạn hướng dẫn gửi tài xế,
- gọi xe cứu hộ nếu cần.

Toàn bộ quá trình thực hiện thủ công.

---

## 3. Bottleneck

Hai bước mất thời gian nhất là:

- Tra cứu trạm sạc còn chỗ.
- Soạn hướng dẫn cho tài xế.

Ngoài ra còn có nguy cơ:

- chọn nhầm trạm sạc,
- chọn trạm quá xa,
- hướng dẫn không đầy đủ.

---

## 4. Business Impact

Ước tính mỗi ngày:

- khoảng 80 sự cố liên quan đến pin tại các thành phố lớn.
- mỗi sự cố mất khoảng 15 phút.

=> khoảng **20 giờ làm việc mỗi ngày** của đội điều phối.

Ngoài ra còn:

- tăng thời gian chờ của tài xế,
- tăng tỷ lệ hủy chuyến,
- giảm mức độ hài lòng của khách hàng.

---

## 5. Success Metric

Mục tiêu sau khi triển khai AI:

- giảm thời gian xử lý

```
15 phút
↓

< 3 phút
```

- 98% đề xuất đúng trạm sạc.
- 95% bản nháp không cần chỉnh sửa nhiều.
- giảm ít nhất 30% số chuyến bị hủy do hết pin.

---

## 6. Operational Boundary

### AI được phép

- đọc vị trí GPS.
- đọc mức pin.
- tra cứu danh sách trạm sạc.
- tạo bản nháp hướng dẫn.
- đề xuất gọi cứu hộ.

### AI KHÔNG được phép

- tự động gửi tin nhắn.
- tự ý điều xe cứu hộ.
- thay đổi dữ liệu hệ thống.
- đưa ra hướng dẫn khi không đủ dữ liệu.
- bỏ qua bước phê duyệt của điều phối viên.

---

# 3. AI Fit Analysis

## So sánh giải pháp

| Tiêu chí | Rule-based | LLM | Agent |
|----------|------------|-----|-------|
| Hiểu ngôn ngữ tự nhiên | Thấp | Cao | Cao |
| Soạn hướng dẫn | Kém | Tốt | Tốt |
| Tự động thực hiện nhiều bước | Không | Không | Có |
| Rủi ro | Thấp | Trung bình | Cao |

### Quyết định

**LLM Feature**

Lý do:

- quy trình tương đối cố định;
- AI chỉ cần hiểu dữ liệu và sinh văn bản;
- chưa cần Agent tự trị;
- vẫn có Human-in-the-loop.

---

# 4. Future-State Workflow

```text
Tài xế báo sự cố
        │
        ▼
AI lấy GPS + mức pin
        │
        ▼
AI tra cứu trạm sạc
        │
        ▼
AI sinh bản nháp hướng dẫn
        │
        ▼
Dispatcher kiểm duyệt
        │
        ├─────────────── Không đạt
        │                     │
        │                     ▼
        │             Điều phối viên xử lý thủ công
        │
        ▼
Gửi hướng dẫn cho tài xế
```

---

# 5. Human-in-the-loop

Điều phối viên phải xác nhận:

- đúng vị trí.
- đúng loại xe.
- đúng loại cổng sạc.
- đúng trạm sạc.
- nội dung hướng dẫn.

Chỉ sau khi xác nhận mới được gửi cho tài xế.

---

# 6. Fallback

Nếu AI:

- không đủ dữ liệu,
- trả lời sai định dạng,
- không tìm được trạm sạc,
- mức độ tự tin thấp,

thì:

1. AI trả về trạng thái **Need Human Review**.
2. Điều phối viên xử lý hoàn toàn thủ công như quy trình hiện tại.

---

# 7. AI Readiness Checklist

| Nội dung | Đánh giá |
|----------|----------|
| Có dữ liệu GPS | ✅ |
| Có dữ liệu trạm sạc | ✅ |
| Có lịch sử sự cố | ✅ |
| Có Human Review | ✅ |
| Có phương án Fallback | ✅ |

---

# 8. Quyết định

## GO

### Lý do

Nhóm quyết định triển khai Prototype vì:

- bài toán có nhu cầu thực tế;
- workflow rõ ràng;
- có dữ liệu đầu vào;
- metric định lượng cụ thể;
- AI chỉ đóng vai trò hỗ trợ nên rủi ro thấp;
- Human-in-the-loop giúp kiểm soát các quyết định quan trọng.

---

# 9. Ước lượng lợi ích

| Tiêu chí | Hiện tại | Sau AI |
|----------|----------|---------|
| Thời gian xử lý | 15 phút | < 3 phút |
| Tỷ lệ chọn đúng trạm | ~90% | 98% |
| Hủy chuyến do xử lý chậm | Cao | Giảm khoảng 30% |
| Khối lượng công việc điều phối | 100% | Giảm khoảng 70% |

---

# 10. Kết luận

Giải pháp sử dụng **LLM Feature** giúp giảm đáng kể thời gian xử lý các sự cố xe điện hết pin mà không làm tăng rủi ro vận hành. AI chỉ hỗ trợ phân tích dữ liệu và tạo bản nháp hướng dẫn, trong khi mọi quyết định cuối cùng vẫn thuộc về điều phối viên. Đây là một bài toán có phạm vi rõ ràng, dữ liệu sẵn có và phù hợp để triển khai Prototype trong giai đoạn đầu của Vin Smart Future.