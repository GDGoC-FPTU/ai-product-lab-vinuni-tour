# 03 - AI Log

**Họ và tên:** .............................................

**MSSV:** .............................................

**Nhóm:** .............................................

---

# AI Reflection Log

## 1. Tôi đã sử dụng AI như thế nào?

Trong quá trình thực hiện bài lab, tôi sử dụng ChatGPT để hỗ trợ ở nhiều giai đoạn khác nhau thay vì để AI làm toàn bộ bài.

Đầu tiên, AI giúp tôi brainstorm các bài toán thực tế trong hệ sinh thái Vingroup như VinFast, Xanh SM, Vinmec, Vinhomes và Vinpearl. AI gợi ý nhiều tình huống vận hành có thể ứng dụng trí tuệ nhân tạo, từ đó tôi lựa chọn bài toán "Hỗ trợ điều phối sự cố xe điện hết pin của Xanh SM" vì có quy trình rõ ràng và dễ xác định phạm vi áp dụng AI.

Sau khi chọn bài toán, tôi tiếp tục sử dụng AI để:

- phân tích quy trình nghiệp vụ hiện tại (Current Workflow);
- xác định các điểm nghẽn (Bottleneck);
- xây dựng Future-State Workflow có sự tham gia của AI;
- đề xuất Success Metrics có thể đo lường được;
- xác định Operational Boundary nhằm giới hạn phạm vi hoạt động của AI;
- xây dựng phương án Human-in-the-loop và Fallback khi AI gặp lỗi.

Ngoài phần phân tích sản phẩm, tôi còn sử dụng AI để hỗ trợ cấu hình môi trường Python, cài đặt thư viện, xử lý lỗi khi kết nối Gemini API và giải thích các lỗi phát sinh trong quá trình chạy chương trình.

---

# 2. AI đã trả lời sai hoặc chưa phù hợp ở điểm nào?

Trong quá trình trao đổi, AI có một số câu trả lời chưa chính xác.

Ví dụ, khi đề xuất kiến trúc hệ thống cho bài toán của Xanh SM, AI ban đầu đề xuất sử dụng mô hình Agent có khả năng tự động gửi hướng dẫn cho tài xế và tự động điều xe cứu hộ.

Sau khi xem xét, tôi nhận thấy đây là giải pháp chưa phù hợp vì:

- việc điều xe cứu hộ ảnh hưởng trực tiếp đến hoạt động vận hành;
- nếu AI đưa ra quyết định sai sẽ gây thiệt hại về chi phí và trải nghiệm khách hàng;
- yêu cầu của bài lab nhấn mạnh phải xác định rõ Operational Boundary và Human-in-the-loop.

Ngoài ra, AI cũng từng giả định rằng dự án đã hỗ trợ đọc file `.env` mặc dù mã nguồn thực tế chưa gọi `load_dotenv()`. Điều này khiến chương trình vẫn báo lỗi không tìm thấy `GEMINI_API_KEY`.

---

# 3. Tôi đã điều chỉnh Prompt như thế nào?

Để nhận được kết quả phù hợp hơn, tôi thay đổi cách đặt câu hỏi.

Thay vì hỏi:

> "Hãy thiết kế hệ thống AI."

Tôi chuyển thành:

> "Thiết kế một giải pháp chỉ sử dụng LLM Feature, không sử dụng Agent tự động. Mọi quyết định cuối cùng phải có Human-in-the-loop và phải có phương án Fallback."

Khi AI trả lời quá chung chung, tôi tiếp tục bổ sung các yêu cầu như:

- đưa ra metric có số liệu cụ thể;
- mô tả workflow theo từng bước;
- giải thích lý do lựa chọn kiến trúc;
- xác định rõ AI được phép và không được phép làm gì.

Đối với phần lập trình, khi AI chưa xác định đúng nguyên nhân lỗi, tôi cung cấp thêm thông báo lỗi (Error Message), cấu trúc thư mục dự án và nội dung file để AI có đủ thông tin phân tích.

---

# 4. Bài học rút ra

Qua bài lab này, tôi nhận thấy AI là một công cụ hỗ trợ rất hiệu quả trong việc phân tích ý tưởng, viết tài liệu và giải thích kỹ thuật.

Tuy nhiên, AI không phải lúc nào cũng đưa ra đáp án chính xác. Nếu người dùng đặt câu hỏi quá chung hoặc không cung cấp đủ ngữ cảnh, AI có thể đưa ra giả định không đúng hoặc đề xuất giải pháp vượt quá phạm vi của bài toán.

Do đó, người sử dụng cần:

- hiểu rõ bài toán trước khi hỏi AI;
- kiểm tra lại các thông tin AI cung cấp;
- liên tục tinh chỉnh prompt;
- xác định rõ giới hạn của AI và luôn có cơ chế Human-in-the-loop đối với các quyết định quan trọng.

Qua trải nghiệm này, tôi nhận thấy AI phù hợp nhất với vai trò **thought partner** – hỗ trợ tư duy, gợi ý giải pháp và tăng năng suất làm việc – thay vì thay thế hoàn toàn con người trong quá trình ra quyết định.