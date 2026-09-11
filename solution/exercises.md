# K4 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 4 tiếng
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> Khi temperature tăng, phản hồi trở nên đa dạng và sáng tạo hơn, nhưng cũng kém nhất quán hơn. Ở mức thấp, câu trả lời ổn định và sát với yêu cầu; ở mức cao, câu trả lời có thể thay đổi từ ngữ, cấu trúc và phong cách nhiều hơn. Nói ngắn gọn: temperature cao = sáng tạo cao, nhưng độ tin cậy giảm.

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> Tôi chọn khoảng 0.2–0.6. Mức này giữ câu trả lời rõ ràng, nhất quán và an toàn, nhưng vẫn tự nhiên hơn so với giá trị gần 0. Với chatbot hỗ trợ khách hàng, độ tin cậy và tính chuẩn xác quan trọng hơn sự sáng tạo ngẫu nhiên.

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> Với workload này, output khoảng 10.000 × 3 × 350 = 10.5 triệu token/ngày. GPT-4o ước tính khoảng 10.5M / 1000 × 0.010 = $105/ngày; GPT-4o-mini khoảng 10.5M / 1000 × 0.0006 = $6.3/ngày. Vậy GPT-4o đắt hơn khoảng 16.7 lần. GPT-4o nên dùng cho phân tích chuyên sâu, viết nội dung chất lượng cao hoặc hỗ trợ nghiệp vụ phức tạp; mini nên dùng cho FAQ, bot hỗ trợ nhanh, tìm kiếm thông tin đơn giản và các case cần tiết kiệm chi phí.

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> Hai phản hồi sẽ khác rất rõ: phiên bản giáo viên ngắn, dễ hiểu, dùng ví dụ gần gũi với trẻ em; phiên bản chuyên gia dài hơn, dùng thuật ngữ kỹ thuật như consensus, transaction, ledger, smart contract. System prompt không chỉ đổi sắc thái lời nói mà còn đổi mục tiêu, mức độ chi tiết và góc nhìn chuyên môn. Nói cách khác, persona ảnh hưởng trực tiếp đến cách model “đặt vai” và cách giải thích nội dung.

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> Trong tiếng Việt, `count_tokens` thường cao hơn ước lượng `số từ / 0.75` khoảng 20–40% tùy đoạn. Vì tokenizer không tách theo "từ" như tiếng Anh một cách đơn giản, mà tách theo đơn vị ngữ nghĩa và ký tự/chuỗi có ý nghĩa hơn. Ngoài ra, tiếng Việt có nhiều từ ghép, dấu câu và cấu trúc rõ ràng hơn, nên cùng một độ dài văn bản lại tốn nhiều token hơn.

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> Streaming quan trọng nhất khi người dùng đang tương tác trực tiếp với chatbot và cần phản hồi ngay như hỗ trợ khách hàng, trợ lý cá nhân, hoặc hỏi đáp thời gian thực. Nó giúp cảm giác chờ ngắn hơn và tạo trải nghiệm mượt mà. Non-streaming phù hợp hơn khi output dài, cần chờ xử lý xong trước khi hiển thị, như tóm tắt tài liệu, sinh báo cáo, hoặc các tác vụ batch mà không cần phản hồi tức thì.

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> Exponential backoff giúp phân tán lượt retry và giảm tải đồng thời trên server, vì không phải tất cả client cùng gọi lại vào thời điểm như nhau. Nếu hàng nghìn client cùng retry với delay cố định, họ sẽ “vỡ nhau” cùng lúc và tạo thành bão request, khiến hệ thống quá tải hơn. Backoff theo cấp số nhân làm nhịp retry san đều hơn và tăng khả năng phục hồi thành công.

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> Tôi chọn persona: "Bạn là trợ lý AI thân thiện, trả lời ngắn gọn bằng tiếng Việt, tập trung vào sự rõ ràng và tính thực dụng." Tôi yêu cầu "trả lời ngắn gọn" để giữ nhịp hội thoại nhanh và tránh dài dòng; "bằng tiếng Việt" đảm bảo phản hồi tự nhiên, dễ hiểu và phù hợp với người dùng. Đây là một persona nhẹ, dễ dùng cho học tập và hỗ trợ công việc.

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> Hạn chế lớn nhất là history chỉ 3 lượt, nên bot dễ quên ngữ cảnh cũ và không có bộ nhớ dài hạn. Một cải thiện cụ thể là lưu context lịch sử dài hơn vào database hoặc session store, ví dụ giữ 20 lượt gần nhất hoặc lưu các thông tin quan trọng từ hội thoại trước. Khi user tiếp tục hỏi, hệ thống sẽ nạp lại context ấy vào prompt để duy trì sự liên tục và mạch lạc.

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên fork và dán link trên trang bài Lab ở VLearn trước 23:59 ngày 11/09/2026
