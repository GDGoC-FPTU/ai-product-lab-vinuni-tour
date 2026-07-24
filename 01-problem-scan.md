# 01 - Problem Scan

**Họ và tên:** .............................................

**MSSV:** .............................................

**Nhóm:** .............................................

---

# Phase 1 — SCAN

## Danh sách bài toán

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | Vinpearl | Tốn thời gian | Nhân viên CSKH phải trả lời hàng trăm email và tin nhắn hỏi về đặt phòng, đổi lịch, hoàn tiền mỗi ngày, gây quá tải vào mùa du lịch. |
| 2 | VinFast | AI-upgrade | Khách hàng mô tả lỗi xe bằng ngôn ngữ tự nhiên nhưng nhân viên mất nhiều thời gian xác định đúng nhóm lỗi trước khi chuyển kỹ thuật viên. |
| 3 | Vinhomes | Lặp lại | Ban quản lý phân loại và chuyển các phản ánh của cư dân (mất điện, hỏng thang máy, vệ sinh...) hoàn toàn thủ công. |
| 4 | Vinmec | Tốn thời gian | Bác sĩ phải viết tóm tắt hồ sơ xuất viện cho từng bệnh nhân sau khi điều trị, mất nhiều thời gian mỗi ngày. |
| 5 | Xanh SM | Stakeholder Pain | Điều phối viên xử lý thủ công các sự cố xe điện hết pin hoặc hỏng xe giữa đường, khiến tài xế chờ lâu và khách hàng phải hủy chuyến. |

---

# Phase 2 — QUICK ASSESS

# Quick Problem Card #1

## Bài toán

Nhân viên CSKH Vinpearl phải trả lời thủ công các câu hỏi về đặt phòng, đổi lịch và hoàn tiền.

**Công ty thành viên:** Vinpearl

### Actor

Nhân viên Chăm sóc khách hàng (Customer Support).

### Workflow hiện tại

1. Khách gửi email hoặc chat.
2. Nhân viên đọc nội dung.
3. Tra cứu chính sách đặt phòng.
4. Soạn câu trả lời.
5. Gửi phản hồi cho khách.

### Bottleneck

Bước 3 và bước 4.

Thời gian trung bình: **8 phút/yêu cầu.**

### AI hỗ trợ

LLM đọc yêu cầu, tra cứu chính sách và tạo bản nháp trả lời để nhân viên kiểm duyệt trước khi gửi.

### Success Metric

- Giảm thời gian xử lý từ **8 phút xuống dưới 2 phút**.
- 95% câu trả lời đúng chính sách.

### Quick Architecture

☑ LLM Feature

---

# Quick Problem Card #2

## Bài toán

Phân loại phản ánh của cư dân Vinhomes đến đúng bộ phận xử lý.

**Công ty thành viên:** Vinhomes

### Actor

Nhân viên Ban quản lý tòa nhà.

### Workflow hiện tại

1. Cư dân gửi phản ánh.
2. Nhân viên đọc nội dung.
3. Xác định loại sự cố.
4. Chuyển đúng bộ phận.
5. Theo dõi trạng thái xử lý.

### Bottleneck

Bước 2 và bước 3.

Khoảng **5 phút/yêu cầu.**

### AI hỗ trợ

LLM tự động phân loại nội dung và đề xuất bộ phận xử lý.

### Success Metric

- 90% phản ánh được phân loại đúng.
- Thời gian phân loại dưới **10 giây**.

### Quick Architecture

☑ LLM Feature

---

# Quick Problem Card #3

## Bài toán

Điều phối viên Xanh SM xử lý sự cố xe điện hết pin ngoài đường.

**Công ty thành viên:** Xanh SM

### Actor

Điều phối viên (Dispatcher).

### Workflow hiện tại

1. Tài xế báo sự cố.
2. Điều phối viên xác định vị trí xe.
3. Tra cứu trạm sạc gần nhất.
4. Soạn hướng dẫn.
5. Liên hệ cứu hộ nếu cần.

### Bottleneck

Bước 3 và bước 4.

Khoảng **12 phút/lượt xử lý.**

### AI hỗ trợ

LLM kết hợp dữ liệu GPS và trạng thái trạm sạc để tạo hướng dẫn, sau đó điều phối viên kiểm duyệt trước khi gửi.

### Success Metric

- Giảm thời gian xử lý từ **15 phút xuống dưới 3 phút**.
- 98% hướng dẫn đúng vị trí và đúng loại trạm sạc.

### Quick Architecture

☑ LLM Feature

---

# Quyết định cá nhân

Trong ba bài toán trên, **Xanh SM – Hỗ trợ điều phối sự cố xe điện hết pin** là bài toán phù hợp nhất để thực hiện Deep Dive vì:

- Có quy trình rõ ràng.
- Có dữ liệu đầu vào tương đối chuẩn (GPS, mức pin, trạng thái trạm sạc).
- Có metric định lượng dễ đo.
- AI chỉ đóng vai trò hỗ trợ tạo đề xuất, vẫn có Human-in-the-loop nên rủi ro thấp.
- Phù hợp sử dụng LLM Feature hơn là Agent tự động hoàn toàn.