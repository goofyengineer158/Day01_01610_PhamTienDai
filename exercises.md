# K4 — Ngày 1: Bài Tập & Phản Ánh

## Khám Phá LLM API | Phiếu Thực Hành

**Thời lượng:** 14h00–18h00
**Cách làm:** Trả lời từng câu ngay sau khi hoàn thành block tương ứng —
đừng để dồn hết về cuối buổi. Thay dòng `*Câu trả lời của bạn*` bằng câu
trả lời thật (chấm tự động sẽ đếm số câu đã trả lời).

---

## Block 1 — API Cơ Bản (trả lời sau Checkpoint 1)

### Câu 1.1 — Độ nhạy của temperature

Gọi `call_openai` với temperature 0.0, 0.7, 1.2 và 1.8 dùng prompt
**"Hãy kể cho tôi một sự thật thú vị về Hà Nội."**

**Bạn nhận thấy quy luật gì qua bốn phản hồi? Ở mức nào phản hồi bắt đầu
kém mạch lạc?** (2–3 câu)

> _Tại các lần call với temperature thấp, câu trả lời mang tính ổn định cao, lặp lại gần như tương tự với các lần gen liên tiếp. Trong khi đó, khi tiến tới các temperature cao hơn, model có xu hướng tăng độ đa dạng trong câu chữ sinh ra, các từ ngữ bắt đầu văn chương hơn, nhưng cũng vì thế model bắt đầu sinh ra hiện tượng sai thông tin, các từ ngữ quá cao siêu và không phù hợp với câu trả lời mong đợi. Độ tự nhiên và hợp lý của câu trả lời tốt nhất khi sử dụng temperature trong trường hợp này_

### Câu 1.2 — Chọn temperature cho sản phẩm

**Bạn sẽ đặt temperature bao nhiêu cho trợ lý soạn thảo hợp đồng pháp lý,
và bao nhiêu cho trợ lý viết slogan quảng cáo? Giải thích khác biệt.**

> _Dựa trên những gì đã làm ở câu 1.1. Trợ lý soạn thảo hợp đồng pháp lý cần sự chính xác tương đối đặc biệt, ta sẽ để temperature ở mức 0 - 0.2 để ưu tiên các câu trả lời nhất quán, các từ ngữ chuẩn chỉ trong tài liệu đã được mô hình tiếp xúc. Với trợ lý viết slogan quảng cáo ta cần ưu tiên mức sáng tạo cao để có những câu từ bay bổng, độc nhất để không trùng với những thứ nó đã được huấn luyện kích thích vào cảm xúc con người, phù hợp với trường hợp ta đang sử dụng _

### Câu 1.3 — Đánh đổi chi phí

Kịch bản: 20.000 người dùng hoạt động mỗi ngày, mỗi người gọi API 2 lần,
mỗi lần trung bình ~500 token đầu ra.

**Ước tính chi phí mỗi ngày của model lớn so với model nhỏ cho workload này
(dựa trên bảng giá trong template). Nêu một trường hợp model lớn xứng đáng
với chi phí và một trường hợp model nhỏ là lựa chọn đúng:**

> _Mỗi ngày hệ thống xử lý:20.000 người dùng Mỗi người gọi API 2 lầnMỗi lần sinh trung bình 500 output token. Tổng số output token mỗi ngày là: 20.000 × 2 × 500 = 20.000.000 token. Theo bảng giá trong đề, model nhỏ tiêu tốn khoảng 2 USD/ngày, trong khi model lớn (GPT-4o) tiêu tốn khoảng 50 USD/ngày, tức chi phí cao gấp khoảng 25 lần.Tuy nhiên, lựa chọn model không nên chỉ dựa trên chi phí mà còn cần cân nhắc giữa chất lượng đầu ra và giá trị mang lại. Model lớn phù hợp với các tác vụ đòi hỏi độ chính xác và khả năng suy luận cao, chẳng hạn như trợ lý soạn thảo hợp đồng pháp lý hoặc hỗ trợ lập trình, nơi một câu trả lời sai có thể gây thiệt hại lớn hơn nhiều so với phần chi phí tăng thêm. Ngược lại, model nhỏ là lựa chọn hợp lý cho các tác vụ có khối lượng truy cập lớn nhưng không yêu cầu suy luận phức tạp, như chatbot FAQ, trả lời câu hỏi thường gặp, tóm tắt văn bản ngắn hoặc phân loại dữ liệu. Trong những trường hợp này, chi phí thấp giúp hệ thống dễ mở rộng mà vẫn đáp ứng được yêu cầu của người dùng. _

---

## Block 2 — System Prompt & Token (trả lời sau Checkpoint 2)

### Câu 2.1 — Sức mạnh của persona

Gọi `chat_with_system_prompt` hai lần với cùng câu hỏi
**"Giải thích máy học (machine learning) là gì?"** nhưng hai system prompt
khác nhau:

- "Bạn là một nhà thơ, trả lời mọi thứ bằng hình ảnh ví von, tránh thuật ngữ."
- "Bạn là kỹ sư phần mềm senior, trả lời chính xác, có ví dụ code khi phù hợp."

**Hai phản hồi khác nhau như thế nào (giọng văn, độ dài, mức kỹ thuật)?
Từ đó rút ra system prompt điều khiển được những khía cạnh nào của phản hồi?**
(3–4 câu)

> _Response nhận được có đặc điểm giống như system_prompt đã mô tả, với giải thích máy học với giọng điệu nhà thơ, mô hình sử dụng các từ ngữ bay bổng, dễ hiểu và trực quan, phù hợp với những người không có một chút base nào và có một trí tưởng tượng tốt. Trong khi giải thích với tư cách kỹ sư phần mềm, model sử dụng các từ ngữ học thuật , những khái niệm mang tính trừu tượng, nhưng tường minh với một người có nền tảng công nghệ thông tin _

### Câu 2.2 — tiktoken vs đếm từ

Chọn một đoạn văn tiếng Việt ~150 từ. So sánh số token theo `count_tokens`
(tiktoken) với ước lượng `số từ / 0.75` mà Part 1 đã dùng.

**Hai con số chênh nhau bao nhiêu phần trăm? Nếu dùng ước lượng thô để dự
toán ngân sách API cho ứng dụng tiếng Việt, bạn sẽ dự toán thiếu hay thừa —
và vì sao?**

> _kết quả trả ra 215, với tiktoken ước lượng, số lượng trả ra lớn hơn 15 token. 215 lớn hơn 200 7.5%. Nếu ước lượng thô cho dự đoán ngân sách API,chắc chắn ta cần dự đoán thiếu vì một từ tiếng việt có chứa dấu nên sẽ mang nhiều token hơn so với từ tiếng anh được các thư viện ước lượng _

---

## Block 3 — Streaming & Độ Bền (trả lời sau Checkpoint 3)

### Câu 3.1 — Trải nghiệm người dùng với streaming

**Xét ba ứng dụng: (a) chatbot văn bản, (b) trợ lý giọng nói đọc to phản hồi,
(c) pipeline dịch tài liệu chạy ngầm ban đêm. Ứng dụng nào hưởng lợi nhiều
nhất từ streaming, ứng dụng nào không cần — và tại sao?** (1 đoạn văn)

> _Trong ba ứng dụng, trợ lý giọng nói đọc to phản hồi hưởng lợi nhiều nhất từ streaming vì hệ thống có thể bắt đầu phát giọng nói ngay khi mô hình sinh ra những từ đầu tiên, giúp giảm độ trễ và tạo cảm giác hội thoại tự nhiên. Chatbot văn bản cũng được hưởng lợi vì người dùng có thể đọc câu trả lời ngay khi nó đang được tạo, nhưng mức độ quan trọng không bằng trợ lý giọng nói. Ngược lại, pipeline dịch tài liệu chạy ngầm ban đêm hầu như không cần streaming, vì đây là tác vụ xử lý hàng loạt, không yêu cầu phản hồi theo thời gian thực; người dùng chỉ cần nhận được bản dịch hoàn chỉnh sau khi quá trình xử lý kết thúc. _

### Câu 3.2 — Vì sao backoff theo cấp số nhân?

**Khi API quá tải và hàng nghìn client cùng retry, exponential backoff giúp
gì so với delay cố định? Tra cứu thêm: kỹ thuật "jitter" (thêm độ trễ ngẫu
nhiên) giải quyết vấn đề gì còn sót lại?**

> _Khi API bị quá tải và hàng nghìn client cùng retry, exponential backoff giúp giảm áp lực lên máy chủ bằng cách tăng dần thời gian chờ giữa các lần thử lại (ví dụ: 1 giây, 2 giây, 4 giây, 8 giây...). Thay vì liên tục gửi yêu cầu với khoảng thời gian cố định, các client sẽ giãn dần tần suất retry, tạo điều kiện để máy chủ có thời gian phục hồi và giảm nguy cơ quá tải kéo dài. Tuy nhiên, exponential backoff vẫn còn một vấn đề: nếu tất cả client đều bắt đầu cùng lúc, chúng cũng sẽ retry lại cùng một thời điểm (ví dụ tất cả cùng chờ 2 giây rồi gửi lại). Hiện tượng này được gọi là thundering herd (đàn gia súc cùng lao tới), khiến máy chủ lại bị quá tải theo từng đợt. Để khắc phục, người ta sử dụng jitter, tức là thêm một khoảng thời gian ngẫu nhiên vào mỗi lần retry. Ví dụ, thay vì tất cả đều retry sau đúng 4 giây, mỗi client sẽ retry trong khoảng 3,5–4,5 giây hoặc một khoảng ngẫu nhiên khác tùy thuật toán. Nhờ vậy, các yêu cầu được phân tán theo thời gian, giảm xung đột và giúp hệ thống ổn định hơn. _

---

## Block 4 — Mini-Project (trả lời sau Checkpoint 4)

### Câu 4.1 — Thiết kế persona

**Viết lại system prompt bạn dùng cho trợ lý của mình. Chỉ ra 2 chỗ trong
prompt mà nếu xóa đi, hành vi trợ lý sẽ thay đổi rõ rệt — và mô tả thay đổi
đó:**

> _System prompt: "Bạn là một trợ lý AI hữu ích, chính xác và lịch sự. Hãy trả lời bằng tiếng Việt, giải thích ngắn gọn nhưng đầy đủ, chỉ sử dụng thông tin có căn cứ và nếu không chắc chắn thì phải nói rõ thay vì suy đoán. Khi người dùng yêu cầu lập trình, hãy cung cấp mã nguồn kèm giải thích. Luôn giữ giọng điệu chuyên nghiệp và thân thiện." Hai thành phần quan trọng trong prompt là: "Chỉ sử dụng thông tin có căn cứ và nếu không chắc chắn thì phải nói rõ thay vì suy đoán." Nếu bỏ phần này, trợ lý có thể tự tin đưa ra các thông tin chưa được kiểm chứng hoặc suy diễn nhiều hơn, làm giảm độ tin cậy của câu trả lời. "Hãy trả lời bằng tiếng Việt." Nếu bỏ yêu cầu này, trợ lý có thể trả lời bằng tiếng Anh hoặc trộn nhiều ngôn ngữ tùy theo ngữ cảnh hoặc dữ liệu đầu vào, khiến trải nghiệm của người dùng không còn nhất quán. _

### Câu 4.2 — Hạn chế & cải thiện

**Trợ lý của bạn giữ history 4 lượt cuối. Hãy mô tả một tình huống hội thoại
cụ thể mà giới hạn này khiến trợ lý trả lời sai/mất ngữ cảnh, và đề xuất một
cách khắc phục (ví dụ: tóm tắt các lượt cũ, tăng giới hạn có chọn lọc...):**

> _ Nếu trợ lý chỉ lưu 4 lượt hội thoại cuối, nó có thể mất những thông tin quan trọng đã được nhắc đến từ trước. Ví dụ, ở đầu cuộc trò chuyện người dùng nói rằng đang phát triển một chatbot sử dụng GPT-4o và FastAPI, sau đó trao đổi nhiều vấn đề khác. Đến hơn 4 lượt sau, người dùng hỏi: "Hãy tối ưu đoạn code cho dự án của tôi." Vì thông tin về dự án đã không còn trong history, trợ lý có thể không biết người dùng đang nói đến dự án nào và đưa ra câu trả lời không phù hợp hoặc phải hỏi lại. Một cách khắc phục là tóm tắt các lượt hội thoại cũ và lưu những thông tin quan trọng (mục tiêu, dự án, yêu cầu, ràng buộc) dưới dạng ngắn gọn để đưa vào context ở các lượt sau. Ngoài ra, có thể tăng giới hạn history một cách có chọn lọc, ưu tiên giữ lại các tin nhắn chứa thông tin dài hạn thay vì chỉ lưu cố định 4 lượt gần nhất._

---

## Danh Sách Kiểm Tra Nộp Bài

- [ ] `python grade.py` — xem điểm tự động, mục tiêu ≥ 75/100
- [ ] Cả 4 checkpoint pytest đều pass
- [ ] Tất cả 9 câu trong file này đã được trả lời
- [ ] Đã copy bài làm vào folder `solution/`, push lên GitHub cá nhân và nộp link repo vào vlearn (theo hướng dẫn README)
