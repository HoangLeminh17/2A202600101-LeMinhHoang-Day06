# Individual reflection — Lê Minh Hoàng (2A202600101)

## 1. Role
FE Designer & Tester. Phụ trách thiết kế giao diện (UI/UX) trên Streamlit và xây dựng bộ công cụ đánh giá (Evaluation Framework) cho hệ thống Trợ lý Dược sĩ, test các module của hệ thống

## 2. Đóng góp cụ thể
- **Thiết kế Front-end (FE):** 
    - Xây dựng giao diện Streamlit chuyên nghiệp với hệ thống màu sắc phân cấp thông tin (Color-coding): Xanh dương cho tư vấn, Cam cho cảnh báo FDA, và Xanh lá cho thuốc thay thế.
    - Sử dụng Custom CSS để tạo các "Clinical Boxes" giúp dược sĩ dễ dàng nhận diện thông tin quan trọng trong 3-5 giây.
    - Triển khai bộ đôi nút "Duyệt Bán/Từ Chối" để thu thập phản hồi của người dùng (Learning Signals), giúp cải thiện Prompt RAG theo thời gian.
- **Hệ thống Kiểm thử (Tester):**
    - Viết script `test.py` tự động hóa việc đánh giá trên 5 kịch bản hết hàng phổ biến nhất.
    - Đo lường và tối ưu các chỉ số metric:
        - **Accuracy (Top-3):** Đảm bảo thuốc thay thế gợi ý có độ tương đồng hoạt chất cao.
        - **Warning Coverage:** Kiểm tra sự hiện diện của từ khóa an toàn ("canh bao", "tac dung phu") đạt mục tiêu >95%.
        - **Latency:** Đo lường thời gian phản hồi thực tế để giữ mức P95 < 5 giây.

## 3. SPEC mạnh/yếu
- **Mạnh nhất:** Cơ chế "Warning Reflection" — Hệ thống không chỉ đưa ra thuốc thay thế mà còn bắt buộc hiển thị dữ liệu gốc từ FDA bên cạnh tóm tắt của AI, giúp tăng tính minh bạch và an toàn (Trust & Safety).
- **Yếu nhất:** Logic so khớp hoạt chất (Matching logic) vẫn dựa trên string-based. Với các thuốc đa thành phần từ FDA, việc map vào `inventory.csv` còn gặp sai lệch nhỏ, cần dùng thêm kĩ thuật Fuzzy Matching hoặc Vector Search.

## 4. Đóng góp khác
- Hỗ trợ team chuẩn hóa dữ liệu `inventory.csv` để đảm bảo field `Hoạt Chất` khớp với API response.
- Debug luồng gọi Gemini API trong `rag_engine.py` để xử lý fallback khi dữ liệu FDA bị thiếu.

## 5. Điều học được
Qua dự án "Trợ lý Dược sĩ", tôi nhận ra metrics cho AI không chỉ là con số kỹ thuật (0/1). Trong y tế, **Recall** của cảnh báo quan trọng hơn **Accuracy** tổng thể. Một hệ thống AI tốt không thay thế con người mà phải "Augment" người dùng (Dược sĩ) bằng cách giảm tải việc tra cứu nhưng vẫn giữ họ ở vị trí ra quyết định cuối cùng (Human-in-the-loop).

## 6. Nếu làm lại
Tôi sẽ triển khai cơ chế Caching cho FDA API kết quả để giảm latency xuống dưới 3 giây và đầu tư thêm vào Mobile UI vì dược sĩ quầy thường dùng máy tính bảng hoặc điện thoại để tra cứu nhanh.

## 7. AI giúp gì / AI sai gì
- **Giúp:** Gemini 2.5 Flash hỗ trợ cực tốt trong việc viết CSS layout tinh tế và brainstorm các kịch bản test case hiểm (Edge cases).
- **Sai/mislead:** AI đôi khi gợi ý thuốc thay thế rất logic về mặt hóa học nhưng thực tế trong kho (`inventory.csv`) không có, dẫn đến việc phải thêm step lọc tồn kho nghiêm ngặt trước khi đưa vào Prompt.