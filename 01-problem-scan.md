# 🔍 01-problem-scan.md — SCAN & QUICK-ASSESS (Cá nhân)

Tài liệu này ghi lại quá trình tìm kiếm cơ hội (SCAN) và đánh giá nhanh (QUICK-ASSESS) các bài toán thực tế có thể tối ưu bằng AI tại các công ty thành viên Vingroup.

---

## 🔍 Phase 1 — SCAN (Cá nhân)

Sử dụng 4 Lenses (Lặp lại, Tốn thời gian, AI-upgrade, Stakeholder Pain) để quét qua hoạt động của các công ty thành viên Vingroup. Dưới đây là 5 bài toán/bottleneck thực tế tìm được:

| # | Subsidiary | Lens | Mô tả ngắn bài toán |
|---|------------|------|---------------------|
| 1 | **Xanh SM** | Tốn thời gian | Điều phối viên xử lý thủ công các báo cáo khẩn cấp từ tài xế về sự cố sạc pin hoặc hết pin giữa đường (mất 12-15 phút/lượt). |
| 2 | **Vinhomes** | Lặp lại | Phân loại tự động và điều hướng các phản ánh (mất nước, hỏng điện, ồn ào) của cư dân trên App Vinhomes Resident về đúng Ban quản lý tòa nhà. |
| 3 | **Vinmec** | Tốn thời gian | Bác sĩ mất quá nhiều thời gian viết tóm tắt hồ sơ xuất viện (Discharge Summary) từ ghi chú bệnh án lâm sàng phức tạp (20-30 phút/bệnh nhân). |
| 4 | **VinFast** | AI-upgrade | Trợ lý ảo gợi ý trạm sạc trống phù hợp với dòng xe và cổng sạc (CCS2/GBT) tự động thay vì tài xế phải tự tìm kiếm thủ công trên bản đồ. |
| 5 | **Vinpearl** | Pain từ người khác | Quản lý khách sạn mất nhiều giờ tổng hợp review từ Agoda, Booking.com để lọc ra các phàn nàn khẩn cấp (như phòng bẩn, thái độ phục vụ kém). |

---

## 🃏 Phase 2 — QUICK-ASSESS (Cá nhân)

Dưới đây là 3 Quick Problem Cards được hoàn thiện cho 3 bài toán tiềm năng nhất.

### 1️⃣ QUICK PROBLEM CARD #1 — Xanh SM Xử lý sự cố sạc pin thực địa
```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #1                                       │
│                                                             │
│ Bài toán: Tài xế Xanh SM báo sự cố cạn pin giữa đường, cần  │
│ chỉ đường trạm gần nhất hoặc điều phối xe sạc pin cứu hộ.    │
│ Công ty thành viên: [x] Xanh SM (GSM)                       │
│                                                             │
│ Ai đang đau? Tài xế (chờ lâu), Điều phối viên (quá tải)     │
│                                                             │
│ Workflow thủ công hiện tại (5 bước):                        │
│   1. Tài xế gọi báo sự cố ──> 2. Dispatcher check GPS xe    │
│   ──> 3. Check thủ công các trạm sạc VinFast còn trụ trống   │
│   ──> 4. Soạn tin nhắn chỉ đường gửi qua App cho tài xế    │
│   ──> 5. Gọi xe sạc pin lưu động (mobile charger) nếu pin <5%│
│                                                             │
│ Bước nào tốn nhất? Bước 3-4 (⏱ 10-12 phút/lượt)              │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3-4 và 5         │
│ (Trích xuất GPS, tự động đề xuất trạm trống hoặc draft cứu hộ)│
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian xử lý sự cố từ 15 phút ──> dưới 3 phút.      │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

### 2️⃣ QUICK PROBLEM CARD #2 — Vinhomes Tự động phân loại phản ánh cư dân
```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #2                                       │
│                                                             │
│ Bài toán: Tự động phân loại và điều hướng các khiếu nại      │
│ của cư dân gửi qua app Vinhomes Resident tới đúng BQL.      │
│ Công ty thành viên: [x] Vinhomes                            │
│                                                             │
│ Ai đang đau? Nhân viên CSKH (quá tải lọc tin), Cư dân (chờ)  │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Cư dân gửi phản ánh ──> 2. CSKH trung tâm đọc thủ công │
│   ──> 3. Phân loại loại phản ánh (kỹ thuật/vệ sinh/an ninh) │
│   ──> 4. Gửi email/lệnh điều phối tới Ban quản lý tòa nhà đó │
│                                                             │
│ Bước nào tốn nhất? Bước 2 & 3 (⏱ 5-7 phút/tin phản ánh)       │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 2 & 3             │
│ (Tự động đọc, phân tích ngôn ngữ tự nhiên và gắn tag tự động)│
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ 90% phản ánh được phân loại và chuyển đúng BQL dưới 5 phút.  │
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```

### 3️⃣ QUICK PROBLEM CARD #3 — Vinmec Soạn tóm tắt hồ sơ xuất viện
```text
┌─────────────────────────────────────────────────────────────┐
│ QUICK PROBLEM CARD #3                                       │
│                                                             │
│ Bài toán: Tự động tổng hợp dữ liệu lâm sàng để soạn thảo     │
│ báo cáo tóm tắt xuất viện dễ hiểu cho bệnh nhân.            │
│ Công ty thành viên: [x] Vinmec                              │
│                                                             │
│ Ai đang đau? Bác sĩ (quá tải viết hồ sơ xuất viện)          │
│                                                             │
│ Workflow thủ công hiện tại (4 bước):                        │
│   1. Bệnh nhân có lịch xuất viện ──> 2. Bác sĩ đọc lại bệnh│
│   án điện tử, kết quả xét nghiệm ──> 3. Gõ tóm tắt tay các │
│   chẩn đoán và dặn dò ──> 4. In và ký gửi bệnh nhân.        │
│                                                             │
│ Bước nào tốn nhất? Bước 2 & 3 (⏱ 20-30 phút/bệnh nhân)       │
│ AI có thể nhảy vào hỗ trợ ở bước nào? Bước 3 (Draft bản tóm │
│ tắt dễ hiểu từ dữ liệu thô bệnh án, bác sĩ chỉ cần review)  │
│                                                             │
│ Đo thành công bằng gì (Metric có số)?                        │
│ Giảm thời gian viết hồ sơ của bác sĩ từ 25 min ──> dưới 5 min│
│                                                             │
│ Quick Architecture: [ ] No AI  [ ] Rule  [x] LLM  [ ] Agent │
└─────────────────────────────────────────────────────────────┘
```
