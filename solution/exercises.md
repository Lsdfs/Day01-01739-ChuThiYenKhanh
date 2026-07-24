# K3 — Ngày 1: Bài Tập & Phản Ánh
## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 9h00–13h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature
Gọi `call_openai` với temperature 0.0, 0.5, 1.0 và 1.5 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Việt Nam."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi?** (2–3 câu)
> *Khi temperature bằng 0.0, các phản hồi thường rất ổn định và gần giống nhau vì model ưu tiên những token có xác suất cao nhất. Khi tăng lên 0.5 và 1.0, nội dung bắt đầu đa dạng hơn về cách diễn đạt và chi tiết được lựa chọn. Ở mức 1.5, câu trả lời khó dự đoán hơn, đôi lúc dài dòng, kém mạch lạc hoặc xuất hiện thông tin ít liên quan do mức ngẫu nhiên quá cao.*

### Câu 1.2 — Chọn temperature cho sản phẩm
**Bạn sẽ đặt temperature bao nhiêu cho chatbot hỗ trợ khách hàng, và tại sao?**
> *Tôi sẽ đặt temperature trong khoảng 0.2–0.3. Chatbot hỗ trợ khách hàng cần đưa ra thông tin chính xác, đồng nhất và đáng tin cậy, đặc biệt khi trả lời về sản phẩm, giá cả hoặc chính sách bảo hành. Temperature thấp giúp giảm sự thay đổi không cần thiết giữa các lần trả lời và hạn chế nguy cơ model tự tạo ra thông tin không đúng.*

### Câu 1.3 — Đánh đổi chi phí
Kịch bản: 10.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 3 lần,
mỗi lần trung bình ~350 token đầu ra.

**Ước tính GPT-4o đắt hơn GPT-4o-mini bao nhiêu lần cho workload này? Nêu một
trường hợp GPT-4o xứng đáng với chi phí và một trường hợp nên dùng mini:**
> *Tổng số token đầu ra mỗi ngày là 10.000 × 3 × 350 = 10.500.000 token. Với bảng giá trong bài, chi phí GPT-4o là 10.500 × 0,010 USD = 105 USD mỗi ngày, còn GPT-4o-mini là 10.500 × 0,0006 USD = 6,30 USD mỗi ngày. Như vậy, GPT-4o đắt hơn khoảng 16,7 lần. GPT-4o phù hợp với những tác vụ phức tạp và có mức độ rủi ro cao như phân tích tài liệu pháp lý hoặc hỗ trợ đánh giá thông tin y khoa, trong khi GPT-4o-mini phù hợp hơn với chatbot FAQ, phân loại tin nhắn hoặc gợi ý sản phẩm ở quy mô lớn.*

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona
Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích blockchain là gì?"** nhưng hai system prompt khác nhau:
- "Bạn là giáo viên tiểu học, giải thích thật đơn giản cho trẻ 8 tuổi."
- "Bạn là chuyên gia tài chính, trả lời chuyên sâu bằng thuật ngữ kỹ thuật."

**Hai phản hồi khác nhau như thế nào (độ dài, từ vựng, ví dụ)? System prompt
ảnh hưởng đến hành vi model ra sao?** (3–4 câu)
> *Với persona giáo viên tiểu học, model trả lời ngắn, sử dụng từ ngữ đơn giản và thường đưa ra ví dụ quen thuộc, chẳng hạn ví blockchain như một cuốn sổ chung mà nhiều người cùng kiểm tra. Với persona chuyên gia tài chính, phản hồi dài và chuyên sâu hơn, có các thuật ngữ như hash function, consensus mechanism và immutable ledger. Câu trả lời dành cho chuyên gia cũng tập trung nhiều hơn vào cơ chế hoạt động thay vì ví dụ đời thường. Điều này cho thấy system prompt có thể định hướng giọng văn, độ khó, cách giải thích và mức độ chi tiết của phản hồi.*

### Câu 2.2 — tiktoken vs đếm từ
Chọn một đoạn văn tiếng Việt ~100 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Vì sao tiếng Việt thường tốn
nhiều token hơn tiếng Anh cùng độ dài?**
> *Với một đoạn gồm khoảng 100 từ tiếng Việt, cách ước lượng số từ / 0.75 cho kết quả khoảng 133 token, trong khi tiktoken thường đếm được khoảng 160–200 token. Mức chênh lệch có thể nằm trong khoảng 20–50%, tùy nội dung cụ thể. Một nguyên nhân là tokenizer được tối ưu nhiều cho tiếng Anh, còn các từ tiếng Việt chứa dấu và có thể bị chia thành nhiều phần nhỏ hơn. Vì vậy, một từ tiếng Việt đôi khi chiếm từ hai token trở lên thay vì chỉ một token.*

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming
**Streaming quan trọng nhất trong trường hợp nào, và khi nào thì
non-streaming lại phù hợp hơn?** (1 đoạn văn)
> *Streaming đặc biệt hữu ích khi model cần tạo phản hồi dài hoặc khi ứng dụng muốn người dùng nhận được nội dung ngay lập tức, chẳng hạn chatbot, trợ lý viết bài hoặc hệ thống giải thích một chủ đề phức tạp. Việc hiển thị từng phần của câu trả lời giúp giảm cảm giác phải chờ đợi, dù tổng thời gian xử lý có thể không thay đổi nhiều. Ngược lại, non-streaming phù hợp với các tác vụ backend như phân loại văn bản, trích xuất dữ liệu, xử lý hàng loạt hoặc trả về JSON, vì chương trình thường cần nhận đầy đủ kết quả trước khi tiếp tục xử lý.*

### Câu 3.2 — Vì sao backoff theo cấp số nhân?
**So với delay cố định (ví dụ luôn chờ 1 giây), exponential backoff có lợi
thế gì khi API bị quá tải? Điều gì xảy ra nếu hàng nghìn client cùng retry
với delay cố định giống nhau?**
> *Exponential backoff làm thời gian chờ tăng dần sau mỗi lần thất bại, ví dụ 0,1 giây, 0,2 giây, 0,4 giây và tiếp tục tăng. Cách này giúp giảm số request được gửi liên tục và tạo thêm thời gian để server phục hồi khi đang quá tải. Nếu hàng nghìn client đều dùng cùng một delay cố định, chúng có thể gửi lại request gần như đồng thời sau mỗi khoảng chờ. Điều đó tạo ra hiệu ứng thundering herd, khiến server liên tục bị một lượng lớn request dồn vào cùng lúc và khó phục hồi.*

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona
**Bạn chọn persona gì cho trợ lý của mình? Viết lại system prompt đó và giải
thích 1–2 lựa chọn từ ngữ quan trọng trong prompt (ví dụ: vì sao yêu cầu
"trả lời ngắn gọn", vì sao chỉ định ngôn ngữ...):**
> *System prompt của tôi là: “Bạn là trợ giảng thân thiện của khóa AI, trả lời ngắn gọn bằng tiếng Việt. Nếu không biết câu trả lời, hãy nói rõ thay vì tự tạo thông tin.” Yêu cầu “trả lời ngắn gọn” giúp nội dung phù hợp với môi trường terminal, đồng thời giảm số lượng output token và chi phí gọi API. Việc yêu cầu model nói rõ khi không biết nhằm hạn chế hallucination, vì một câu trả lời không chắc chắn nhưng được trình bày quá tự tin có thể gây hiểu nhầm cho người học.*

### Câu 4.2 — Hạn chế & cải thiện
**Trợ lý của bạn hiện có hạn chế lớn nhất là gì (ví dụ: history chỉ 3 lượt,
không có bộ nhớ dài hạn, không kiểm duyệt nội dung...)? Đề xuất một cải
thiện cụ thể và mô tả ngắn cách triển khai:**
> *Hạn chế lớn nhất của trợ lý là chỉ lưu ba lượt hội thoại gần nhất. Khi cuộc trò chuyện kéo dài, các thông tin quan trọng ở những lượt đầu sẽ bị loại khỏi history và model không còn sử dụng được ngữ cảnh đó. Một cách cải thiện là bổ sung cơ chế tóm tắt định kỳ: sau một số lượt nhất định, chương trình dùng một model nhỏ để tóm tắt phần history cũ thành một đoạn ngắn và lưu vào biến history_summary. Ở những lần gọi tiếp theo, bản tóm tắt này được thêm vào messages dưới dạng system message, nhờ đó trợ lý vẫn giữ được ngữ cảnh chính mà không làm số token tăng quá nhiều.*

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/` và zip theo hướng dẫn README
