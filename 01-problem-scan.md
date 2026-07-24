# LAB 02 — PROBLEM SCAN & QUICK PROBLEM CARDS

## Thông tin bài làm

- **Tên nhóm:** `VinUni Tour`
- **Người thực hiện:** Huỳnh Thị Hải Châu
- **MSSV:** `[2A202601912]`
- **Ngày thực hiện:** 24/07/2026
- **Đơn vị giả lập:** Vin Smart Future
- **Bài toán được ưu tiên:** Hỗ trợ điều phối sự cố pin thấp/hết pin cho đội xe Xanh SM
-**Thành viên trong nhóm:** Nguyễn Thiên Lộc, Lê Bình Nguyên, Huỳnh Thị Hải Châu, Hoàng Lê Minh, Hán Vũ Long, Nguyễn Phúc Huy Hoàng


> **Lưu ý về dữ liệu:** Các số liệu thời gian, tần suất và tỷ lệ trong bài là **working assumptions phục vụ Lab**, được xây dựng từ tình huống mẫu. Trước khi triển khai thật, nhóm cần xác minh bằng log vận hành và phỏng vấn điều phối viên.

---

# Phase 1 — SCAN: Quét cơ hội AI

Nhóm sử dụng bốn lenses: **Lặp lại**, **Tốn thời gian**, **AI-upgrade** và **Stakeholder Pain** để tìm các điểm nghẽn vận hành.

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---:|---|---|---|
| 1 | **Xanh SM** | Tốn thời gian / Stakeholder Pain | Khi tài xế báo pin thấp hoặc sự cố sạc ngoài đường, điều phối viên phải tra GPS, tìm trạm phù hợp, kiểm tra khoảng cách và soạn hướng dẫn thủ công. |
| 2 | **VinFast** | AI-upgrade | Khách hàng mô tả lỗi xe bằng tiếng Việt tự nhiên; cố vấn dịch vụ phải đọc, hỏi lại và gán nhóm lỗi kỹ thuật ban đầu. |
| 3 | **Vinhomes** | Lặp lại | Phản ánh cư dân về điện, nước, thang máy, tiếng ồn và an ninh phải được nhân viên đọc rồi chuyển đến đúng ban quản lý hoặc nhà thầu. |
| 4 | **Vinpearl** | Stakeholder Pain | Quản lý phải đọc review từ nhiều nền tảng để phát hiện phàn nàn khẩn cấp như vệ sinh phòng, chất lượng dịch vụ hoặc sự cố an toàn. |
| 5 | **Vinmec** | Tốn thời gian | Bác sĩ mất nhiều thời gian tổng hợp ghi chú, xét nghiệm và chỉ định để tạo bản nháp tóm tắt xuất viện. |
| 6 | **VinFast** | Lặp lại | Bộ phận tài chính đối chiếu dữ liệu phiên sạc với hóa đơn của đối tác theo nhiều định dạng khác nhau, dễ sai lệch và mất thời gian. |

---

# Phase 2 — QUICK-ASSESS

## Quick Problem Card #1 — Xanh SM: Hỗ trợ điều phối sự cố pin thực địa

```text
┌────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                              │
│                                                                    │
│ Bài toán: Điều phối viên cần xử lý nhanh yêu cầu hỗ trợ khi xe      │
│ Xanh SM bị pin thấp, lỗi sạc hoặc có nguy cơ hết pin ngoài đường.  │
│ Công ty thành viên: [x] Xanh SM                                    │
│                                                                    │
│ Ai đang đau?                                                       │
│ - Tài xế: phải chờ lâu và có nguy cơ dừng xe giữa đường.           │
│ - Điều phối viên: phải tra cứu nhiều hệ thống trong giờ cao điểm.  │
│                                                                    │
│ Workflow thủ công hiện tại:                                        │
│ 1. Nhận cuộc gọi/App report                                        │
│ → 2. Tra GPS và xác minh pin/biển số                               │
│ → 3. Tra trạm sạc phù hợp, khoảng cách và tình trạng trụ           │
│ → 4. Soạn tin nhắn chỉ dẫn                                         │
│ → 5. Liên hệ xe sạc di động/cứu hộ nếu xe không thể di chuyển      │
│                                                                    │
│ Bước tốn thời gian/lỗi nhất: Bước 3–4, khoảng 10 phút/lượt.         │
│ AI hỗ trợ: Tổng hợp dữ liệu và tạo bản nháp hướng dẫn có cấu trúc. │
│                                                                    │
│ Metric:                                                            │
│ - Giảm thời gian xử lý từ 15 phút xuống dưới 3 phút/lượt.          │
│ - 98% đề xuất đúng loại trạm và tuân thủ ranh giới an toàn.        │
│ - 100% phản hồi vận hành bắt đầu bằng [DRAFT_ONLY].                │
│                                                                    │
│ Quick Architecture: Rule Engine + [x] LLM Feature + HITL           │
└────────────────────────────────────────────────────────────────────┘
```

### Nhận xét nhanh

- **AI phù hợp ở phần ngôn ngữ:** tóm tắt sự cố và soạn tin nhắn rõ ràng.
- **Rule-based bắt buộc ở phần an toàn:** kiểm tra pin, khoảng cách, loại cổng sạc và quyết định điều xe sạc di động.
- **Không chọn Agent tự trị:** AI không được tự gửi lệnh hoặc tự điều xe khi chưa có người duyệt.

---

## Quick Problem Card #2 — Vinhomes: Phân loại và điều hướng phản ánh cư dân

```text
┌────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                              │
│                                                                    │
│ Bài toán: Tự động phân loại phản ánh cư dân và đề xuất bộ phận      │
│ tiếp nhận phù hợp, đồng thời tạo bản nháp phản hồi ban đầu.        │
│ Công ty thành viên: [x] Vinhomes                                   │
│                                                                    │
│ Ai đang đau? Nhân viên CSKH, ban quản lý tòa nhà và cư dân.        │
│                                                                    │
│ Workflow thủ công hiện tại:                                        │
│ 1. Cư dân gửi phản ánh trên App                                    │
│ → 2. CSKH đọc nội dung và ảnh đính kèm                             │
│ → 3. Chọn nhóm vấn đề/mức ưu tiên                                  │
│ → 4. Chuyển ticket đến ban quản lý/nhà thầu                        │
│ → 5. Soạn phản hồi xác nhận                                        │
│                                                                    │
│ Bước tốn thời gian/lỗi nhất: Bước 2–4, khoảng 8 phút/ticket.       │
│ AI hỗ trợ: Trích xuất tòa nhà, loại sự cố, mức khẩn cấp và draft.  │
│                                                                    │
│ Metric:                                                            │
│ - Giảm thời gian phân loại từ 8 phút xuống dưới 30 giây.           │
│ - Routing accuracy tối thiểu 92% trên bộ dữ liệu đã gán nhãn.      │
│ - 100% ticket về an ninh, cháy nổ hoặc y tế được chuyển HITL.      │
│                                                                    │
│ Quick Architecture: Rule ưu tiên + [x] LLM Feature                 │
└────────────────────────────────────────────────────────────────────┘
```

### Nhận xét nhanh

Bài toán có tiềm năng lớn nhưng cần taxonomy rõ ràng cho từng khu đô thị. Các từ khóa khẩn cấp nên được bắt bằng rule trước khi LLM phân loại để giảm rủi ro bỏ sót.

---

## Quick Problem Card #3 — VinFast: Phân loại lỗi xe từ mô tả của khách hàng

```text
┌────────────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                              │
│                                                                    │
│ Bài toán: Từ mô tả tiếng Việt của khách hàng, hệ thống đề xuất     │
│ nhóm triệu chứng và mã lỗi kiểm tra ban đầu cho cố vấn dịch vụ.    │
│ Công ty thành viên: [x] VinFast                                    │
│                                                                    │
│ Ai đang đau? Khách hàng, tổng đài viên và cố vấn dịch vụ.          │
│                                                                    │
│ Workflow thủ công hiện tại:                                        │
│ 1. Khách mô tả tiếng kêu/hiện tượng                                │
│ → 2. Tổng đài hỏi lại nhiều lần                                    │
│ → 3. Tra sổ tay triệu chứng và chọn nhóm lỗi                       │
│ → 4. Chuyển thông tin đến xưởng dịch vụ                            │
│                                                                    │
│ Bước tốn thời gian/lỗi nhất: Bước 2–3, khoảng 12 phút/lượt.        │
│ AI hỗ trợ: Chuẩn hóa mô tả và đưa ra Top-3 nhóm lỗi để tham khảo.  │
│                                                                    │
│ Metric:                                                            │
│ - Giảm thời gian triage từ 12 phút xuống dưới 3 phút.              │
│ - Top-3 recall tối thiểu 90% trên tập case đã xác nhận.            │
│ - 100% cảnh báo an toàn nghiêm trọng phải được kỹ thuật viên duyệt.│
│                                                                    │
│ Quick Architecture: [x] LLM Feature + Retrieval + HITL             │
└────────────────────────────────────────────────────────────────────┘
```

### Nhận xét nhanh

AI chỉ hỗ trợ **triage**, không được kết luận xe an toàn, không tự chẩn đoán cuối cùng và không thay thế kỹ thuật viên.

---

# So sánh và lựa chọn bài toán Deep-Dive

| Tiêu chí | Xanh SM — Sự cố pin | Vinhomes — Routing phản ánh | VinFast — Triage lỗi xe |
|---|---:|---:|---:|
| Tần suất lặp lại | Cao | Rất cao | Trung bình |
| Tác động vận hành tức thời | **Rất cao** | Trung bình | Cao |
| Dữ liệu đầu vào có cấu trúc | Khá rõ | Trung bình | Thấp hơn |
| Rủi ro nếu AI sai | Cao nhưng kiểm soát được bằng rule + HITL | Trung bình–cao | Rất cao |
| Khả năng prototype trong Lab | **Cao** | Cao | Trung bình |
| Quyết định | **Chọn Deep-Dive** | Để backlog | Cần thêm dữ liệu kỹ thuật |

## Kết luận cá nhân

Tôi chọn bài toán **“Xanh SM — Hỗ trợ điều phối sự cố pin thực địa”** vì:

1. Workflow hiện tại có bước lặp lại và bottleneck rõ ràng.
2. Có thể phân tách tốt giữa **rule an toàn** và **LLM tạo ngôn ngữ**.
3. Có metric định lượng trực tiếp về thời gian xử lý và độ tuân thủ.
4. Có thể stress-test ranh giới bằng adversarial prompts trong thời gian Lab.
