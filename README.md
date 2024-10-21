# VN-Traffic-Maritime-Law-RAG-QA
RAG-based QA on administrative penalties in Vietnamese maritime, road, rail, and aviation transport regulations.
Hỏi đáp dựa trên RAG về xử phạt hành chính trong các quy định về vận tải hàng hải, đường bộ, đường sắt và hàng không của Việt Nam.

## Introduction

Project này nhầm thử nghiệm 1 số biến thể của RAG nhầm hỏi đáp số tiền vi phạm luật giao thông ở Việt Nam. Để đơn giản hóa, tôi chỉ thử nghiệm với nghị định 100/2019/NĐ-CP. Hiện tại (2024) nghị định 123/2021/NĐ-CP đã bổ sung và chỉnh sửa 1 số điều, nếu bạn muốn sử dụng hệ thống này thì cần cập nhật các thông tin mới hơn.

## Notebook
- Basic rag: [basic_rag.ipynb](basic_RAG/basic_rag.ipynb):
    - Note book này sử dụng rag đơn giản để thử nghiệm. Có thể thấy nếu dùng LLM đủ lớn thì sẽ trả lời tốt các câu hỏi rõ ràng, ngắn gọn và đơn giản.
    - Chunking:
        - Mình sử dụng [dangvantuan/vietnamese-embedding(https://huggingface.co/dangvantuan/vietnamese-embedding) làm model embeddings.
        - Vì model này limit số token khá ngắn nên mình chia chunk có độ dài 256 token và overlap 50 tokens.
        - Mình sẽ lấy top 20 relevant chunks với query làm context.
        - Kết quả cho thấy các chunk ngắn dẫn đến nội dung mỗi chunk không đầy đủ:
            - Có thể không tổng hợp đủ thông tin cho LLM khi câu hỏi đòi hỏi trích dẫn nhiều điều luật.
            - Context merge nhiều đoạn không đầy đủ có thể làm LLM bị rối khi trả lời.
    - Các LLM được thử nghiệm là:
        - Qwen2.5 70B (dùng togetherai vì colab free không chạy được)
        - Llama 3.1 405B (dùng togetherai vì colab free không chạy được)
    - Kết quả cho thấy LLM trả lời không tốt các câu hỏi đòi hỏi trích xuất contexts dài hơn (cần improve phần chunking).
    - Kết quả 1 số câu hỏi được lưu tại: [short_context](basic_RAG/short_context/)
- Basic rag với long context: [basic_rag_longcontext.ipynb](basic_RAG/basic_rag_longcontext.ipynb):
    - Note book này improve notebook trên bằng việc dùng embedding model có context size lớn hơn
    - Chunking:
        - Mình sử dụng [vietnamese-embedding-LongContext](https://huggingface.co/dangvantuan/vietnamese-embedding-LongContext) làm model embeddings.
        - Mình chia chunk có độ dài lớn hơn 1024 tokens và chỉnh overlap thành 100 tokens. (mình sẽ thử nghiệm thêm tham số overlap)
        - Mình sẽ lấy top 20 relevant chunks với query làm context.
        - model dài hơn cho phép mỗi chunk chứa nhiều thông tin hơn. Giúp cung cấp context tốt hơn.
    - Các LLM được sử dụng: Qwen2 70B và Llama 3.1 405B
    - Kết quả đã tốt hơn phương pháp ban đầu và trả lời được 1 số câu hỏi khó hơn.
    - Kết quả 1 số câu hỏi được lưu tại: [long_context](basic_RAG/long_context/)
