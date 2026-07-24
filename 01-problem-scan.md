# 01 — Problem Scan & Quick Problem Cards

## Phạm vi và giả định

Bài làm sử dụng bối cảnh, 4 lenses và các gợi ý trong worksheet/inspiration kit. Các con số về thời gian, sản lượng và tỷ lệ lỗi dưới đây là **giả định phục vụ bài lab**, cần được xác minh bằng log vận hành và phỏng vấn stakeholder trước khi triển khai thực tế.

---

# Phase 1 — SCAN

## Bảng quét cơ hội

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---:|---|---|---|
| 1 | **Vinhomes** | Lặp lại (Repetitive) | Nhân viên CSKH đọc, phân loại và chuyển thủ công phản ánh cư dân như mất nước, hỏng đèn, tiếng ồn hoặc vệ sinh đến đúng bộ phận phụ trách. |
| 2 | **Vinpearl** | Pain từ người khác (Stakeholder Pain) | Quản lý khách sạn phải đọc nhiều review từ nhiều nền tảng để phát hiện phàn nàn khẩn cấp về vệ sinh, thái độ phục vụ hoặc an toàn. |
| 3 | **VinFast** | AI-upgrade | Cố vấn dịch vụ tiếp nhận mô tả lỗi xe bằng tiếng Việt tự do, sau đó tự diễn giải và chọn nhóm lỗi kỹ thuật ban đầu. |
| 4 | **Xanh SM** | Tốn thời gian (Time-consuming) | Nhân viên phân tích phải đọc ghi chú tài xế và nội dung cuộc gọi để tổng hợp lý do khách hủy chuyến. |
| 5 | **Vinmec** | Tốn thời gian (Time-consuming) | Bác sĩ mất nhiều thời gian tổng hợp ghi chú, xét nghiệm và chỉ định để soạn bản nháp tóm tắt xuất viện. |
| 6 | **VinUni** | Lặp lại (Repetitive) | Trợ giảng đọc lỗi autograder và viết phản hồi tương tự cho nhiều bài lab có cùng kiểu lỗi cú pháp hoặc logic. |

## Sàng lọc ban đầu

Ba bài toán được chọn để QUICK-ASSESS:

1. Vinhomes — phân loại và điều hướng phản ánh cư dân.
2. Vinpearl — phát hiện và tóm tắt review khách sạn khẩn cấp.
3. VinFast — phân loại sơ bộ mô tả lỗi xe bằng tiếng Việt.

Lý do: cả ba đều có đầu vào ngôn ngữ tự nhiên, quy trình thủ công rõ ràng, metric có thể đo, và có thể giới hạn AI ở vai trò **phân loại/soạn nháp** thay vì tự quyết định nghiệp vụ.

---

# Phase 2 — QUICK-ASSESS

## QUICK PROBLEM CARD #1 — Vinhomes

**Bài toán (1 câu):** Tự động hỗ trợ phân loại mức độ khẩn cấp và điều hướng phản ánh cư dân đến đúng ban quản lý/bộ phận xử lý.

**Công ty thành viên:** Vinhomes

**Ai đang đau (Actor)?** Nhân viên CSKH/Ban quản lý tòa nhà; cư dân phải chờ lâu nếu phản ánh bị chuyển sai nơi.

**Workflow thủ công hiện tại:**

1. Cư dân gửi phản ánh qua app, email hoặc tổng đài.  
2. Nhân viên CSKH đọc và chuẩn hóa nội dung.  
3. Nhân viên tự chọn loại sự cố và mức độ ưu tiên.  
4. Tra cứu tòa nhà/bộ phận phụ trách rồi chuyển ticket.  
5. Ban quản lý nhận ticket và tạo lệnh xử lý.

**Bước tốn thời gian/lỗi nhất:** Bước 3–4, khoảng **9 phút/ticket**; dễ nhầm giữa kỹ thuật, an ninh, vệ sinh và dịch vụ cư dân.

**AI có thể hỗ trợ ở đâu?** Đề xuất `category`, `urgency`, `responsible_team` và tóm tắt một câu; nhân viên duyệt trước khi chuyển.

**Metric có số:**

- Giảm thời gian phân loại và route từ **9 phút xuống dưới 2 phút/ticket**.
- Ít nhất **90%** ticket được đề xuất đúng nhóm xử lý ở top-1.
- **100%** ticket khẩn cấp vẫn phải được con người duyệt.

**Quick Architecture:** **LLM Feature** kết hợp rule bắt buộc cho từ khóa an toàn và bảng ánh xạ bộ phận.

---

## QUICK PROBLEM CARD #2 — Vinpearl

**Bài toán (1 câu):** Hỗ trợ phát hiện review tiêu cực/khẩn cấp và tạo bản tóm tắt hành động cho quản lý khách sạn.

**Công ty thành viên:** Vinpearl

**Ai đang đau (Actor)?** Quản lý trải nghiệm khách hàng, quản lý khách sạn và nhân viên vận hành.

**Workflow thủ công hiện tại:**

1. Nhân viên mở từng nền tảng review.  
2. Đọc nội dung và xác định cảm xúc.  
3. Gắn nhóm vấn đề như phòng, vệ sinh, F&B hoặc thái độ phục vụ.  
4. Chụp/chép review vào báo cáo.  
5. Gửi quản lý và theo dõi phản hồi.

**Bước tốn thời gian/lỗi nhất:** Bước 2–4, khoảng **8 phút/review**; review dài hoặc đa ngôn ngữ dễ bị bỏ sót.

**AI có thể hỗ trợ ở đâu?** Tóm tắt, phân loại chủ đề, chấm mức độ khẩn cấp và dịch nội dung về tiếng Việt.

**Metric có số:**

- Giảm thời gian xử lý từ **8 phút xuống dưới 90 giây/review**.
- Recall ít nhất **95%** với review có từ khóa an toàn, vệ sinh nghiêm trọng hoặc khiếu nại dịch vụ cấp cao.
- Không tự động đăng phản hồi công khai cho khách.

**Quick Architecture:** **LLM Feature** + rule từ khóa khẩn cấp.

---

## QUICK PROBLEM CARD #3 — VinFast

**Bài toán (1 câu):** Hỗ trợ cố vấn dịch vụ chuyển mô tả lỗi xe tiếng Việt tự do thành nhóm lỗi kỹ thuật sơ bộ.

**Công ty thành viên:** VinFast

**Ai đang đau (Actor)?** Cố vấn dịch vụ, kỹ thuật viên và khách hàng chờ tiếp nhận.

**Workflow thủ công hiện tại:**

1. Khách hàng mô tả hiện tượng bằng lời nói hoặc tin nhắn.  
2. Cố vấn hỏi lại để làm rõ.  
3. Cố vấn tự chọn nhóm lỗi ban đầu.  
4. Tạo phiếu tiếp nhận.  
5. Kỹ thuật viên kiểm tra thực tế và xác nhận.

**Bước tốn thời gian/lỗi nhất:** Bước 2–3, khoảng **7 phút/lượt**; ngôn ngữ đời thường khó ánh xạ sang thuật ngữ kỹ thuật.

**AI có thể hỗ trợ ở đâu?** Tóm tắt triệu chứng, đề xuất nhóm hệ thống liên quan và danh sách câu hỏi làm rõ.

**Metric có số:**

- Giảm thời gian tạo phiếu tiếp nhận từ **10 phút xuống dưới 4 phút**.
- Top-3 nhóm hệ thống đề xuất chứa nhóm lỗi được kỹ thuật viên xác nhận trong ít nhất **90%** trường hợp thử nghiệm.
- AI không được kết luận nguyên nhân cuối cùng hoặc tuyên bố xe an toàn để tiếp tục chạy.

**Quick Architecture:** **LLM Feature** với Human-in-the-loop bắt buộc.

---

# Kết luận cá nhân

Bài toán Vinhomes được ưu tiên cho Deep-Dive vì:

- Quy trình có đầu vào văn bản và taxonomy tương đối rõ.
- Sai sót có thể kiểm soát bằng bước duyệt của nhân viên.
- Giá trị vận hành đo được qua thời gian route, tỷ lệ chuyển đúng và SLA.
- Có thể bắt đầu bằng prototype nhỏ, không cần Agent tự trị.
