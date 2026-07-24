# 🏢 02-deep-dive-report.md — Problem Deep-Dive & Evaluation (Nhóm)

**Tên Nhóm:** Vin Smart Future - GSM Co-pilot  
**Thành viên:**  
- Nguyễn Thiên Lộc  
- Lê Bình Nguyên  
- Huỳnh Thị Hải Châu
- Hoàng Lê Minh
- Hán Vũ Long
- Nguyễn Phúc Huy Hoàng

---

## 🗳️ Quyết định lựa chọn của nhóm
Nhóm quyết định chọn bài toán: **Xanh SM Xử lý sự cố sạc pin thực địa**

### Lý do lựa chọn và loại bỏ các thẻ khác:
- **Lý do chọn bài toán này:** Đây là bài toán có tần suất xảy ra cao ở thực địa (tài xế taxi điện di chuyển liên tục, việc hết pin đột xuất là rủi ro lớn). Giải quyết bài toán này giúp tăng trực tiếp SLA và giảm tỷ lệ hủy chuyến của hành khách thời gian thực (real-time). Hơn nữa, việc tích hợp AI để tự động hóa định vị và soạn tin nhắn hướng dẫn/cứu hộ mang lại hiệu quả tức thì với độ phức tạp kỹ thuật vừa phải.
- **Lý do loại bỏ các thẻ khác:** 
  - *Vinhomes CSKH:* Quy trình phân loại phản ánh tuy lặp lại nhưng các tranh chấp hành chính, phí quản lý đòi hỏi kiến thức pháp lý và chính sách nội bộ thay đổi liên tục, có rủi ro pháp lý cao nếu AI trả lời sai.
  - *Vinmec Discharge Summary:* Mặc dù giúp tiết kiệm nhiều thời gian cho bác sĩ, nhưng việc xử lý dữ liệu y tế cực kỳ nhạy cảm và yêu cầu độ chính xác 100%, cần thời gian chuẩn bị dữ liệu chuẩn (ground truth) rất lớn.

---

## 🏗️ Phase 3 — DEEP-DIVE

### 3.1. Problem Statement (6-field) & Metrics

| Field | Nội dung chi tiết |
|---|---|
| **1. Actor / Operator** | Điều phối viên (Dispatcher) tại Trung tâm Điều vận Xanh SM. |
| **2. Current Workflow** | Tài xế gọi báo sự cố ──> Dispatcher tra cứu GPS thủ công ──> Tra cứu các trạm sạc VinFast còn trụ trống gần đó ──> Soạn tin nhắn chỉ dẫn gửi qua App tài xế ──> Gọi xe cứu hộ lưu động nếu pin xe cạn kiệt. |
| **3. Bottleneck** | Bước tra cứu thủ công các trạm sạc trống và ngồi gõ tay tin nhắn chỉ dẫn/đường đi cho tài xế (mất 10-12 phút mỗi lượt). |
| **4. Business Impact** | Xe nằm chết trên đường lâu (15-20 phút), tài xế bị gián đoạn đón khách, giảm tỷ lệ hoàn thành chuyến (SLA) và làm hành khách hủy chuyến do tài xế trễ hẹn. |
| **5. Success Metric** | Giảm thời gian điều vận và hướng dẫn từ **15 phút ──> dưới 3 phút/lượt**. 95% tin nhắn hướng dẫn được AI soạn thảo tự động chính xác. |
| **6. Operational Boundary** | AI chỉ được phép *soạn thảo tin nhắn nháp (draft)* và đề xuất cứu hộ, luôn bắt đầu bằng tag `[DRAFT_ONLY]`. Tuyệt đối không được tự động gửi tin nhắn đến tài xế hoặc tự động ra lệnh điều xe sạc cứu hộ mà không có sự phê duyệt thủ công của Dispatcher. |

### 3.2. Future-State Flow & AI Fit
- **Mức độ ứng dụng AI (AI Fit Matrix):** [ ] Rule / State-Machine  [x] LLM Feature  [ ] Agentic Loop
- **Mô tả Future-State Flow:**
  - *Bước 1 (AI Step):* Hệ thống tự động lấy tọa độ GPS của xe và dung lượng pin của tài xế khi nhận cuộc gọi. LLM đọc danh sách các trạm sạc trống gần nhất để tự động soạn nháp tin nhắn chỉ đường (nếu pin >= 5%) hoặc xuất cấu trúc JSON điều phối xe sạc pin lưu động (nếu pin < 5% và không có trạm nào dưới 5km).
  - *Bước 2 (Human-in-the-loop Step):* Điều phối viên kiểm tra lại nội dung tin nhắn nháp `[DRAFT_ONLY]` hoặc thông tin xe cứu hộ được đề xuất trên màn hình điều vận, chỉnh sửa nếu cần và bấm nút "Gửi/Phê duyệt".
  - *Bước 3 (Fallback Step):* Nếu gặp lỗi kết nối API hoặc LLM trả về dữ liệu không hợp lệ, hệ thống tự động bỏ qua AI và chuyển thẳng thông tin tọa độ GPS cùng số điện thoại tài xế lên giao diện bản đồ truyền thống để điều phối viên tự xử lý thủ công.

*(Lưu ý: Sơ đồ quy trình hiện tại được cả nhóm phác thảo chi tiết và lưu tại file `04-workflow-diagram.png` ở thư mục gốc).*

---

## 📊 Phase 5 — EVALUATE: Phân tích độ sẵn sàng của AI

Nhóm tự đánh giá dự án bằng cách điền vào bảng Checklist dưới đây:

| Câu hỏi đánh giá độ sẵn sàng (Go/No-Go Checklist) | Yes | No | Giải thích ngắn gọn lý do / Dẫn chứng số liệu |
|---|---|---|---|
| **1. Business Value:** Bài toán này có thực sự quan trọng và giải quyết xong sẽ tiết kiệm > 50% thời gian/chi phí không? | [x] | [ ] | Tiết kiệm từ 15 phút xuống 3 phút (> 80% thời gian xử lý sự cố), giúp xe nhanh chóng quay lại vận hành đón khách. |
| **2. Feasibility:** Công nghệ LLM/AI hiện tại có đủ khả năng xử lý tốt bài toán này mà không bị lỗi nghiêm trọng không? | [x] | [ ] | Rất khả thi. Việc so khớp khoảng cách GPS và soạn thảo văn bản hướng dẫn là thế mạnh của các mô hình LLM nhỏ như Gemini 2.5 Flash. |
| **3. Data Readiness:** Nhóm đã có sẵn dữ liệu mẫu hoặc cách thức thu thập dữ liệu sạch để làm ground truth chưa? | [x] | [ ] | Đã có sẵn dữ liệu vị trí các trạm sạc VinFast trên toàn quốc và log tọa độ GPS thời gian thực của đội xe GSM. |
| **4. Human-in-the-loop:** Đã thiết kế bước phê duyệt/kiểm soát của con người để chặn lỗi đầu ra chưa? | [x] | [ ] | Có. Mọi tin nhắn đều là bản nháp `[DRAFT_ONLY]` và phải qua nút bấm phê duyệt của Điều phối viên mới được gửi đi. |
| **5. Fallback:** Đã có phương án dự phòng thủ công rõ ràng khi hệ thống AI gặp sự cố/sập mạng chưa? | [x] | [ ] | Có. Hệ thống tự động chuyển sang chế độ tra cứu và soạn tin bằng tay trên bản đồ truyền thống nếu API lỗi. |

### 🏁 Quyết định cuối cùng của nhóm:
👉 **GO**

### Luận điểm kỹ thuật & Ước lượng chi phí:
- **Luận điểm kỹ thuật:** Sử dụng mô hình `gemini-2.5-flash` có tốc độ phản hồi cực nhanh (dưới 1.5 giây), chi phí API rất rẻ và hỗ trợ gán chỉ thị hệ thống (`system_instruction`) tốt để đảm bảo giữ vững ranh giới an toàn (`[DRAFT_ONLY]` và JSON cứu hộ).
- **Ước lượng chi phí vận hành:** Trung bình Xanh SM có khoảng 100 sự cố sạc/ngày trên toàn hệ thống.
  - Số lượng request: 100 requests/ngày = 3,000 requests/tháng.
  - Chi phí cho mỗi request trên Gemini 2.5 Flash (khoảng 1,000 tokens input + 200 tokens output) là $0.0001.
  - Tổng chi phí API: **$0.3 / tháng** (chưa tới 10,000 VND), hiệu quả kinh tế cực kỳ cao so với giá trị mang lại.
