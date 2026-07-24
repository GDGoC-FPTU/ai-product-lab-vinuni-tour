# 01 — Problem Scan & Quick Problem Cards

> **Lab 02 — AI Product Scoping (Vin Smart Future)**  
> **Họ và tên:** `[ĐIỀN HỌ TÊN]`  
> **MSSV:** `[ĐIỀN MSSV]`  
> **Nhóm:** `[ĐIỀN TÊN NHÓM]`

## Lưu ý về số liệu

Các con số trong bài là **ước tính phục vụ scoping trong phạm vi lab**, không phải dữ liệu vận hành đã được Vingroup xác nhận. Khi triển khai thực tế, nhóm phải kiểm chứng lại bằng log hệ thống, phỏng vấn stakeholder và dữ liệu nội bộ.

---

# Phase 1 — SCAN: Quét cơ hội

## Danh sách bài toán

| # | Công ty thành viên | Lens | Mô tả ngắn bài toán |
|---:|---|---|---|
| 1 | Xanh SM | Stakeholder Pain | Khi xe điện còn rất ít pin hoặc gặp sự cố sạc giữa đường, tài xế phải gọi điều vận; điều phối viên tra cứu thủ công vị trí, trạm sạc và phương án cứu hộ. |
| 2 | VinFast | Lặp lại | Nhân viên phải đối chiếu dữ liệu phiên sạc với hóa đơn của trạm sạc đối tác và tìm các dòng chênh lệch. |
| 3 | Vinhomes | AI-upgrade | Phản ánh của cư dân phải được CSKH đọc, phân loại và chuyển thủ công đến đúng bộ phận kỹ thuật, an ninh hoặc ban quản lý. |
| 4 | Vinmec | Tốn thời gian | Bác sĩ phải tổng hợp bệnh án, xét nghiệm, thuốc và chỉ định để viết tóm tắt xuất viện cho từng bệnh nhân. |
| 5 | Vinpearl | Tốn thời gian | Nhân viên đặt phòng đoàn phải đọc email dài, trích xuất ngày ở, số phòng, hạng phòng, suất ăn và yêu cầu đặc biệt. |
| 6 | Xanh SM | Lặp lại | Đội vận hành phải đọc ghi chú tài xế và nội dung cuộc gọi để tổng hợp lý do khách hủy chuyến. |

## Đánh giá nhanh

Thang điểm 1–5; điểm cao hơn nghĩa là cơ hội phù hợp hơn để thử nghiệm trong lab.

| Bài toán | Giá trị vận hành | Dữ liệu có thể thu thập | Thử nghiệm scope hẹp | Rủi ro dễ kiểm soát | Tổng |
|---|---:|---:|---:|---:|---:|
| Xanh SM — xử lý xe pin yếu/sự cố sạc | 5 | 4 | 5 | 4 | **18** |
| VinFast — đối chiếu dữ liệu sạc | 4 | 5 | 4 | 5 | **18** |
| Vinhomes — phân loại phản ánh cư dân | 4 | 4 | 5 | 4 | **17** |
| Vinmec — draft tóm tắt xuất viện | 5 | 3 | 3 | 2 | **13** |
| Vinpearl — đọc email đặt phòng đoàn | 4 | 3 | 4 | 3 | **14** |
| Xanh SM — phân tích lý do hủy chuyến | 3 | 4 | 4 | 5 | **16** |

Ba bài toán được chọn làm Quick Problem Cards là:

1. Xanh SM — xử lý xe pin yếu hoặc sự cố sạc.
2. Vinhomes — phân loại và route phản ánh cư dân.
3. Vinmec — tạo bản nháp tóm tắt xuất viện.

> Bài toán đối chiếu dữ liệu sạc có điểm cao nhưng phần lớn có thể giải quyết tốt bằng SQL, rule và data validation. Nhóm ưu tiên các bài toán có thành phần ngôn ngữ tự nhiên rõ ràng hơn để đánh giá AI fit.

---

# Phase 2 — QUICK-ASSESS

## Quick Problem Card #1 — Xanh SM: Xử lý xe pin yếu/sự cố sạc

**Bài toán (1 câu):** Khi tài xế Xanh SM báo xe còn rất ít pin hoặc không thể tiếp tục hành trình, hệ thống cần tổng hợp dữ liệu và tạo phương án xử lý an toàn để điều phối viên phê duyệt.

**Công ty thành viên:** Xanh SM (GSM)

**Ai đang đau (Actor/Operator):**

- Tài xế đang dừng xe hoặc có nguy cơ cạn pin.
- Điều phối viên phải thao tác trên nhiều hệ thống.
- Khách hàng có thể phải chờ lâu hoặc bị hủy chuyến.

**Workflow thủ công hiện tại:**

1. Tài xế gọi hoặc tạo ticket báo mức pin, vị trí và tình trạng xe.
2. Điều phối viên xác minh biển số, dòng xe, mức pin và tọa độ GPS.
3. Điều phối viên mở bản đồ và dashboard trạm sạc để tìm trạm phù hợp.
4. Điều phối viên kiểm tra khoảng cách, loại cổng sạc và khả năng xe tới trạm.
5. Điều phối viên soạn hướng dẫn hoặc gọi đội xe sạc/cứu hộ, sau đó ghi log.

**Bước tốn thời gian/lỗi nhất:** Bước 3–5, khoảng **12 phút/lượt**, do phải chuyển qua nhiều màn hình và tự kiểm tra các ràng buộc an toàn.

**AI có thể hỗ trợ ở đâu:**

- Hiểu và chuẩn hóa nội dung báo sự cố.
- Tóm tắt dữ liệu xe, pin, vị trí và trạm ứng viên.
- Draft thông báo cho tài xế.
- Giải thích lý do đề xuất.
- Không tự gửi và không được vượt rule an toàn.

**Metric có số:**

- Giảm tổng thời gian xử lý từ **15 phút xuống dưới 3 phút**.
- Ít nhất **98%** đề xuất đúng loại cổng sạc và đúng dữ liệu đầu vào.
- **100%** trường hợp pin `< 5%` và trạm xa hơn `5 km` phải chuyển sang `dispatch_mobile_charger`.
- **0%** tin nhắn được gửi tự động khi chưa có điều phối viên duyệt.
- **100%** output hợp lệ phải bắt đầu bằng `[DRAFT_ONLY]`.

**Quick Architecture:** **LLM Feature + deterministic rules + Human-in-the-loop**

**Vì sao không chọn Agent:** Quy trình có cấu trúc cố định và hành động sai có thể khiến xe cạn pin giữa đường. AI chỉ nên tạo bản nháp, còn rule và con người kiểm soát quyết định.

---

## Quick Problem Card #2 — Vinhomes: Phân loại và route phản ánh cư dân

**Bài toán (1 câu):** Hệ thống đọc phản ánh tiếng Việt của cư dân, gợi ý nhóm sự cố, mức ưu tiên và bộ phận xử lý để CSKH duyệt.

**Công ty thành viên:** Vinhomes

**Ai đang đau (Actor/Operator):**

- Nhân viên CSKH phải đọc và phân loại từng ticket.
- Ban quản lý hoặc kỹ thuật nhận ticket sai nhóm.
- Cư dân phải chờ hoặc mô tả lại sự cố.

**Workflow thủ công hiện tại:**

1. Cư dân gửi mô tả và ảnh qua ứng dụng.
2. CSKH đọc nội dung và tra thông tin tòa/căn hộ.
3. CSKH chọn danh mục: điện, nước, thang máy, an ninh, tiếng ồn...
4. CSKH xác định mức khẩn cấp và chuyển bộ phận.
5. Bộ phận nhận xử lý hoặc trả lại nếu route sai.

**Bước tốn thời gian/lỗi nhất:** Bước 2–4, khoảng **8–12 phút/ticket**, đặc biệt khi mô tả ngắn, mơ hồ hoặc chứa nhiều vấn đề.

**AI có thể hỗ trợ ở đâu:**

- Phân loại ý định và trích xuất vị trí/sự cố.
- Gợi ý mức độ ưu tiên.
- Draft câu hỏi bổ sung khi thiếu thông tin.
- Giải thích lý do route để CSKH kiểm tra.

**Metric có số:**

- Ít nhất **90%** ticket được tạo gợi ý trong **dưới 30 giây**.
- Độ chính xác route top-1 đạt **≥ 90%** trên tập test đã gắn nhãn.
- Giảm tỷ lệ ticket bị trả lại do route sai từ **12% xuống dưới 4%**.
- Các từ khóa cháy, khói, kẹt thang máy hoặc đe dọa an ninh phải đạt recall **100%** trên bộ test safety.

**Quick Architecture:** **LLM Feature + rules cho tình huống khẩn cấp + Human-in-the-loop**

---

## Quick Problem Card #3 — Vinmec: Draft tóm tắt hồ sơ xuất viện

**Bài toán (1 câu):** Hệ thống tạo bản nháp tóm tắt xuất viện từ dữ liệu bệnh án và ghi chú lâm sàng để bác sĩ kiểm tra, chỉnh sửa và ký.

**Công ty thành viên:** Vinmec

**Ai đang đau (Actor/Operator):**

- Bác sĩ mất nhiều thời gian tổng hợp nhiều nguồn dữ liệu.
- Điều dưỡng phải kiểm tra tính đầy đủ.
- Bệnh nhân cần nội dung nhất quán, dễ hiểu.

**Workflow thủ công hiện tại:**

1. Bác sĩ mở bệnh án, xét nghiệm, chỉ định và ghi chú.
2. Chọn chẩn đoán, diễn biến điều trị, thủ thuật và thuốc.
3. Viết tóm tắt xuất viện.
4. Kiểm tra lịch tái khám và hướng dẫn theo dõi.
5. Bác sĩ ký; điều dưỡng in hoặc gửi cho bệnh nhân.

**Bước tốn thời gian/lỗi nhất:** Bước 1–4, khoảng **20–30 phút/bệnh nhân**, vì dữ liệu nằm ở nhiều phần và có nội dung tự do.

**AI có thể hỗ trợ ở đâu:**

- Trích xuất và sắp xếp thông tin theo mẫu.
- Tạo bản nháp ngôn ngữ dễ hiểu.
- Đánh dấu trường thiếu hoặc mâu thuẫn.
- Không tự chẩn đoán, kê thuốc hoặc phát hành hồ sơ.

**Metric có số:**

- Tạo draft trong **dưới 3 phút**.
- Giảm thời gian bác sĩ hoàn thiện hồ sơ từ **25 phút xuống dưới 10 phút**.
- Recall các trường bắt buộc đạt **≥ 98%** trên tập test.
- **100%** hồ sơ phải có bác sĩ phê duyệt trước khi phát hành.
- **0** nội dung thuốc hoặc chẩn đoán được tự thêm khi không tồn tại trong nguồn.

**Quick Architecture:** **LLM Feature/RAG + validation + bắt buộc bác sĩ Human-in-the-loop**

---

# Quyết định lựa chọn

Nhóm chọn **Quick Problem Card #1 — Xanh SM: Xử lý xe pin yếu/sự cố sạc thực địa** để thực hiện Deep-Dive.

## Lý do chọn

- Tác động trực tiếp đến thời gian hoạt động của xe và trải nghiệm tài xế/khách hàng.
- Workflow đủ hẹp để làm prototype trong một buổi lab.
- Có thể tách rõ phần rule an toàn và phần LLM xử lý ngôn ngữ.
- Có metric rõ ràng: thời gian, độ đúng, tuân thủ boundary và fallback.
- Có thể pilot mà không cho AI tự gửi hoặc tự điều phối.

## Vì sao chưa chọn hai bài toán còn lại

- **Vinhomes:** cần taxonomy ticket và tập dữ liệu gắn nhãn riêng cho từng dự án.
- **Vinmec:** rủi ro y tế và dữ liệu nhạy cảm cao hơn; cần governance và đánh giá lâm sàng nghiêm ngặt.
