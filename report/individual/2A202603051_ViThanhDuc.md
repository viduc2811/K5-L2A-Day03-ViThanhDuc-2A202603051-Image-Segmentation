# Báo cáo thực hành Day 3 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên [LMS](https://aithucchien.vinuni.edu.vn). Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](../../reports/REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202603051
- Ngày / CVAT local: 11–12/11/2026 / CVAT local (Docker)
- Công cụ đã dùng: CVAT local (Brush/Polygon để xem và sửa mask); SAM3 (`facebook/sam3`, Hugging Face transformers, chạy trên Google Colab) để tạo mask ban đầu cho các task; YOLO26 (`yolo26x.pt`) kết hợp SAM3 theo box, chỉ thử cho `hard_panoptic`. Mask sinh tự động được upload vào CVAT rồi kiểm tra và sửa.

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh [LMS](https://aithucchien.vinuni.edu.vn) đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | cp1_holes.zip | 1 / 1 | 3 |
| cp2_slice | cp2_slice.zip | 1 / 1 | 3 |
| cp5_occlusion | cp5_occlusion.zip | 1 / 1 | 3 |
| cp3_thin | cp3_thin.zip | 1 / 1 | 3 |
| cp4_curb | cp4_curb.zip | 1 / 1 | 3 |
| cp6_coverage | cp6_coverage.zip | 1 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: ảnh 000000181542.jpg, người phụ nữ mặc áo dài hồng đi qua vạch sang đường ở giữa ảnh
- Class và quy tắc tôi dùng để chọn biên: class là person. Mask chỉ bám theo người, không lấy xe máy bên cạnh, vì xe máy đã là instance riêng. Người lái xe được tách thành person riêng so với xe của họ. Quy tắc chọn vùng biên là bao cả tóc, quần áo, tay chân thành một mask liền.
- Nếu dùng gợi ý sau đó: vùng gợi ý sai/đúng, hành động sửa/giữ và lý do: Đúng: mask người phụ nữ bám sát tà áo và đôi chân(SAM3 cũng tạo mask cho người phụ nữ này), tôi check thấy đúng object và mask nên không sửa
- Nếu không dùng gợi ý: ghi “không dùng”; vẫn giải thích một quyết định gán nhãn của mình

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: medium_instance, ảnh 000000181542.jpg, poster thời trang trong cửa kính Chanel ở phía trên giữa (object #4).
- Lỗi thuộc loại: sai lớp / thiếu-thừa vật / gộp-tách / biên / phủ vùng / khác: sai lớp hoặc thiếu-thừa vật.
- Bằng chứng tôi nhìn thấy: SAM3 gán nhãn person cho hình người in trên poster. Nó nằm sau kính, ngay cạnh cửa hàng, không đứng trên mặt đường như các person thật.
- Quy tắc và hành động sửa: chỉ gán nhãn vật thật, không gán nhãn hình in nên em đã xoá
- Sau sửa đã Save và export lại chưa? đã sửa lại và export

Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa (ví dụ task, metric trước/sau nếu có): … / chưa có điểm. Scorecard ba tier tối đa **82**, không phải điểm cuối trên 100. Không tự ghi PASS/top 3/bonus; người phụ trách xác nhận theo tiêu chí lớp. Không đưa file ground truth vào fork.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| 000000181542, xe bán tải màu cam giữa ảnh | truck hoặc car | Xe bán tải có thùng hàng phía sau, và lớp truck có trong danh sách class |Giữ truck. Hỏi coach xem bán tải có nên tính là car không. |
| 000000373353, vật màu xanh ngọc cắt ở mép trái dưới | motorcycle hoặc phần đầu của một xe khác bị cắt | Chỉ thấy một mảng nhỏ ở mép ảnh, không rõ bánh xe hay tay lái |vật bị cắt gần hết có nên gán nhãn không? Hiện tạm giữ theo SAM3. |
| 000000458325, người trượt ván giữa đường |Mask person có gồm ván trượt, xe nôi hay không, và nhóm người đi sát nhau tách thế nào |Ván trượt và xe nôi không thuộc class nào nên không gán. Mỗi người là một instance riêng. |Giữ mask chỉ gồm người. Nhóm xa nhỏ giữ nếu còn nhận ra hình người. |
