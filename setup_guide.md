## Để con Bot có thể tự học và tự cập nhật Knowledge Base thông qua các nút logic (HTTP Request, If/Else) -> chọn Chatflow
1. Lý do
* Chatflow: Là phiên bản nâng cấp của Chatbot, cho phép ông xây dựng các luồng công việc (workflow) phức tạp cho các cuộc hội thoại nhiều lượt. Chỉ có ở đây ông mới có thể "bố trận" các nút HTTP Request để Bot tự động lưu kiến thức mới vào Dataset mỗi khi ông và nó chat với nhau

* Bước 1: Nút "Knowledge Retrieval" (Truy vấn cũ)
Mục tiêu: Lấy kiến thức đã có để trả lời.
Cấu hình: Kết nối với Dataset hiện tại của ông (ví dụ: "Blockchain_Master_Data").
Biến đầu ra: {{context}}.
Bước 2: Nút "LLM Node" (Xử lý & Suy luận)
Dùng System Prompt sau để con Bot biết nhiệm vụ "đa nhân cách" của nó:

System Prompt:

"Mày là một chuyên gia Blockchain Architecture. Hãy trả lời câu hỏi của người dùng dựa trên {{context}}.
Nhiệm vụ quan trọng: > 1. Nếu người dùng cung cấp thông tin mới hoặc câu trả lời có chứa kiến thức chưa có trong {{context}}, hãy trích xuất nó ra.
2. Định dạng lại kiến thức mới đó theo cấu trúc JSON:
{"is_new": true, "content": "Kiến thức đã được biên tập lại theo văn phong công sở", "topic": "Chủ đề"}.
3. Nếu không có gì mới, để is_new: false."

------------------------------------------------------------------------
Dưới đây là cách "bố trận" các nút trong Chatflow để kết nối với cái API này:

### 1. Luồng logic trong Canvas
1.  **Start:** Nhận câu hỏi .
2.  **Knowledge Retrieval:** Lấy kiến thức cũ từ `IBM_Data_Analyst`.
3.  **LLM Node:** Trả lời  và trích xuất kiến thức mới (như cái System Prompt).
4.  **Condition (If/Else):** Kiểm tra xem LLM có tìm thấy kiến thức mới không.
5.  **HTTP Request:** Nếu có kiến thức mới, nút này sẽ gọi đến **Knowledge API** để lưu lại.

### 2. Thông số "áp mã" cho nút HTTP Request
Nhập chính xác các mục này vào nút HTTP Request để nó thông nòng với cái API của Knowledge Base:

*   **URL:** `[https://api.dify.ai/v1/datasets/](https://api.dify.ai/v1/datasets/){dataset_id}/document/`
    *   *(Thay `{dataset_id}` bằng chuỗi ký tự ông lấy trên thanh địa chỉ khi vào kho tri thức).*
*   **Method:** `POST`
*   **Headers:**
    *   `Authorization`: `Bearer {API_KEY_CỦA_ÔNG}`
    *   `Content-Type`: `application/json`
*   **Body (JSON):**
```json
{
    "text": "{{biến_kiến_thức_mới_từ_LLM_Node}}",
    "indexing_technique": "high_quality",
    "process_rule": {
        "mode": "automatic"
    }
}
```

---


Mỗi khi phát hiện ra một logic mới trong mà trong tài liệu cũ chưa có, chỉ cần chat. Con Bot sẽ tự động cập nhật vào kho. Lần sau, khi hỏi lại đúng vấn đề đó, cái `Knowledge Retrieval` sẽ tìm thấy ngay thông tin mới nhất mà ông đã "dạy" nó ở lượt chat trước.
