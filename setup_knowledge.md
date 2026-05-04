Để con Bot "tự học" một cách chuẩn chỉ ngay từ đầu, cần cấu hình cái Knowledge Base (Dataset) này thật khôn ngoan.

1. Chunk Settings (Cắt nhỏ dữ liệu)
Delimiter: Giữ nguyên \n\n là ổn để phân tách các đoạn văn.

Maximum chunk length: Nên để khoảng 800 - 1000 characters. Đừng để quá dài khiến context bị loãng, cũng đừng quá ngắn làm mất ý nghĩa của câu.

Chunk overlap: Tăng lên khoảng 100 characters. Việc này cực kỳ quan trọng để khi thêm kiến thức mới, nó có sự kết nối (gối đầu) với kiến thức cũ, giúp AI hiểu mạch logic tốt hơn.

2. Index Method (Phương thức lập chỉ mục)
Chọn: High Quality (Recommend).

Vì đang làm về Blockchain và RWA — những thứ đòi hỏi độ chính xác cực cao — nên cần dùng Embedding Model để AI hiểu được ý nghĩa sâu xa của thuật ngữ thay vì chỉ tìm kiếm từ khóa bề nổi (Economical).

3. Retrieval Setting (Cấu hình truy vấn) - CỰC KỲ QUAN TRỌNG
Đây là chỗ sửa lỗi Knowledge Retrieval

Chọn: Hybrid Search (Recommend).

Nó kết hợp cả Vector Search (tìm theo ý nghĩa) và Full-text Search (tìm theo từ khóa chính xác). Với các thuật ngữ như "TSS", "RWA", "Ollama", Hybrid Search là vô đối.

Top K: Đẩy lên 5. Cho phép AI lấy ra nhiều đoạn thông tin hơn để tổng hợp.

Score Threshold: hạ xuống 0.5 hoặc 0.6. Để cao hơn dễ dẫn đến việc Bot im lặng vì không tìm thấy đoạn nào khớp hoàn hảo.

4. Nút thắt "Tự học": Chunk using Q&A format

Lời khuyên: Nếu định để Bot tự tạo cặp Hỏi-Đáp rồi lưu lại, thì ban đầu khi tạo Knowledge bằng file có sẵn, KHÔNG NÊN tích vào đây (để nó lưu dạng văn bản thuần cho đầy đủ).

Tuy nhiên, ở các bước sau này khi Bot tự lưu kiến thức mới qua API, chúng ta sẽ ép nó lưu theo dạng QA để tối ưu tốc độ truy vấn.

Nhấn "Save & Process" để Dify bắt đầu "nhai" dữ liệu.


Sau đó sẽ hiện ra Access API và Go to document

"Access the API" màu xám (bên cạnh nút màu xanh) chính là "chìa khóa" để tự động hóa.

Nói một cách thực dụng: Nếu cái Knowledge Base này là cái kho chứa sách, thì cái API này là cái cổng hậu cho phép con Bot Mentor tự lẻn vào để cất thêm sách mới mà không cần phải tự tay upload file thủ công.

Khi bấm vào nút đó, nó sẽ dẫn đến trang tài liệu kỹ thuật, nhưng chỉ cần quan tâm 2 thứ quan trọng nhất để thiết lập nút HTTP Request trong Chatflow:
Cái nút "Access the API" (ở góc trái bên cạnh nút màu xanh trong hình image_c40621.png) chính là "chìa khóa" để thực hiện nước đi tự động hóa.

Nói một cách thực dụng: Nếu cái Knowledge Base này là cái kho chứa sách, thì cái API này là cái cổng hậu cho phép con Bot Mentor tự lẻn vào để cất thêm sách mới mà không cần phải tự tay upload file thủ công.

Khi bấm vào nút đó, nó sẽ dẫn đến trang tài liệu kỹ thuật, nhưng chỉ cần quan tâm 2 thứ quan trọng nhất để thiết lập nút HTTP Request trong Chatflow:

1. API Key (Quyền truy cập kho)
Cần tạo một cái API Key tại đây. Con Bot Mentor sẽ dùng cái Key này để "chứng minh" với hệ thống Dify rằng: "Tôi có quyền ghi dữ liệu mới vào cái kho IBM_Data_Analyst này".

2. Dataset ID (Địa chỉ kho)
Trong trang API đó, sẽ thấy một dãy ký tự (ví dụ: 27a1b2...). Đây là mã định danh duy nhất của cái Knowledge Base vừa tạo. Cần dán cái mã này vào URL của nút HTTP Request để con Bot biết nó phải ném kiến thức mới vào đúng chỗ.

Tại sao nó lại quan trọng ngay lúc này?
Vì Hỏi đáp xong -> Bot tự rút ra kiến thức mới -> Tự lưu lại.
Để làm được việc "Tự lưu lại", con Bot bắt buộc phải gọi qua cái API này.

Action tiếp theo:
Quay vào trang KnowledgeBase

Bấm vào "Service API". (hiện ra nút API Key)

Tạo một cái và copy lại (cất vào chỗ kín).

Quay lại giao diện Chatflow đã tạo ở bước trước.
-------------------------------------------------

2. Cách lấy Dataset ID (Địa chỉ kho tri thức)
Cái này cực kỳ quan trọng để con Bot không ném dữ liệu nhầm chỗ:

Vào mục Knowledge (Kho tri thức) ở trên cùng giao diện Dify.

Chọn cái kho IBM_Data_Analyst vừa tạo.

Nhìn lên thanh địa chỉ (URL) của trình duyệt. Nó sẽ có dạng: .../datasets/{DATASET_ID}/documents.

Cái chuỗi ký tự nằm sau chữ datasets/ chính là Dataset ID .

-------------------------------------------

