# Lab 02 – Deep Dive Report

## Nhóm

**Tên nhóm:** VinUni Tour

**MSSV:** 

| Họ và tên | MSSV |
| --------- | ---- |
| Lê Bình Nguyên |  2A202601659   |
| Nguyễn Thiên Lộc |      |
| Huỳnh Thị Hải Châu  |      |
| Hoàng Lê Minh |      |
| Hán Vũ Long |      |
| Nguyễn Phúc Huy Hoàng |      |

---

# Đề tài được lựa chọn

## Xanh SM – AI hỗ trợ phân loại và định tuyến phản hồi khách hàng

### Lý do lựa chọn

Xanh SM tiếp nhận lượng lớn phản hồi từ nhiều kênh như ứng dụng, hotline và email. Hiện nay nhân viên CSKH phải đọc và phân loại từng phản hồi trước khi chuyển đến bộ phận phù hợp. Quy trình này mất nhiều thời gian, dễ nhầm lẫn và làm tăng thời gian xử lý khiếu nại.

LLM có khả năng hiểu ngôn ngữ tự nhiên, vì vậy rất phù hợp để đọc, tóm tắt và phân loại phản hồi trước khi nhân viên xác nhận.

---

# 3.1 Current-State Workflow

```text
Khách hàng gửi phản hồi
(App / Hotline / Email)
            │
            ▼
Hệ thống tạo Ticket
            │
            ▼
CSKH đọc toàn bộ nội dung phản hồi 
            │
            ▼
Xác định loại vấn đề 

• Thanh toán
• Tài xế
• Ứng dụng
• Khuyến mãi
• An toàn
            │
            ▼
Chuyển ticket đến bộ phận liên quan
            │
            ▼
Bộ phận xử lý
```

### Actor

Nhân viên Chăm sóc khách hàng (Customer Support).

### Handoff

* Hệ thống → CSKH
* CSKH → Bộ phận chuyên môn

### Bottleneck

* Đọc phản hồi dài.
* Phân loại thủ công.
* Đánh giá mức độ ưu tiên.

### Thời gian

* Đọc phản hồi: ~4 phút
* Phân loại: ~2 phút
* Chuyển ticket: ~1 phút

**Tổng:** khoảng **7 phút/ticket**

---

# 3.2 Problem Statement

| Field                    | Nội dung                                                                                                                                                                                                            |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Actor / Operator**     | Nhân viên Customer Support của Xanh SM                                                                                                                                                                              |
| **Current Workflow**     | Tiếp nhận phản hồi từ nhiều kênh, đọc toàn bộ nội dung, xác định loại vấn đề, chuyển ticket đến bộ phận phù hợp và theo dõi xử lý.                                                                                  |
| **Bottleneck**           | Việc đọc và phân loại phản hồi bằng tay chiếm nhiều thời gian, đặc biệt với các phản hồi dài hoặc có nhiều vấn đề cùng lúc.                                                                                         |
| **Business Impact**      | Trung bình mỗi ticket mất khoảng 7 phút để xử lý bước đầu. Khi số lượng phản hồi tăng vào giờ cao điểm sẽ làm tăng thời gian chờ, ảnh hưởng SLA và trải nghiệm khách hàng.                                          |
| **Success Metric**       | Giảm thời gian phân loại từ 7 phút xuống dưới 1 phút; độ chính xác phân loại đạt trên 95%; giảm tối thiểu 50% số ticket bị chuyển sai bộ phận.                                                                      |
| **Operational Boundary** | AI chỉ được phép đọc, tóm tắt và đề xuất phân loại. AI **không được tự động trả lời khách hàng hoặc đóng ticket**. Nhân viên CSKH phải xác nhận trước khi ticket được chuyển sang bộ phận khác (Human-in-the-loop). |

---

# 3.3 Future-State Flow & AI Fit

## AI Fit

**LLM Feature**

### Vì sao không dùng Rule?

Khách hàng diễn đạt cùng một vấn đề theo rất nhiều cách khác nhau, ví dụ:

* "Tài xế chạy vòng."
* "Đi đường xa bất thường."
* "Ứng dụng tính sai tiền."

Rule-based khó bao phủ hết các cách diễn đạt.

### Vì sao chưa cần Agent?

Quy trình chỉ cần phân tích văn bản và đưa ra đề xuất phân loại, không cần lập kế hoạch nhiều bước hay tự động thực hiện hành động.

---

## Future Workflow

```text
Khách hàng gửi phản hồi
            │
            ▼
AI đọc phản hồi
            │
            ▼
AI tạo tóm tắt
            │
            ▼
AI phân loại vấn đề
            │
            ▼
AI đề xuất bộ phận xử lý
            │
            ▼
CSKH kiểm tra (HITL)
            │
            ▼
Ticket được chuyển đi
```

---

## Human-in-the-loop

Nhân viên chỉ cần:

* Kiểm tra tóm tắt.
* Xác nhận loại vấn đề.
* Nhấn "Approve".

---

## Fallback

Nếu AI:

* không tự tin,
* phân loại nhiều nhãn,
* hoặc không hiểu nội dung,

thì ticket sẽ được chuyển về quy trình xử lý thủ công như hiện tại.

---

# Phase 5 – Evaluate

## AI Readiness Checklist

✅ Có dữ liệu phản hồi từ khách hàng.

✅ Quy trình hiện tại đã chuẩn hóa.

✅ Có Human-in-the-loop.

✅ Rủi ro thấp vì AI không tự đưa ra quyết định cuối cùng.

---

# Decision

## GO

### Justification

Đây là bài toán có dữ liệu đầu vào rõ ràng, khối lượng xử lý lớn và tốn nhiều công sức của nhân viên CSKH. AI chỉ đóng vai trò hỗ trợ đọc, tóm tắt và phân loại nên rủi ro thấp, dễ triển khai theo từng giai đoạn.

Dự án có khả năng giảm đáng kể thời gian xử lý ticket, cải thiện SLA và nâng cao trải nghiệm khách hàng mà vẫn đảm bảo con người là người ra quyết định cuối cùng.
