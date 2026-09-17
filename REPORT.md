# Báo cáo Day 5 — đã hoàn thành

- Mã học viên theo lớp: [điền mã lớp của bạn]
- Ngày / CVAT local: 17/09/2026
- Công cụ đã dùng: CVAT local, Polygon, Brush/Mask, Eraser, export dataset ZIP

## 1. Bài đã nộp

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_semantic.zip | 3 / 3 | 20 |
| medium_instance | medium_instance.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | chưa có | 0 / 1 | 3 |
| cp2_slice | chưa có | 0 / 1 | 3 |
| cp5_occlusion | chưa có | 0 / 1 | 3 |
| cp3_thin | chưa có | 0 / 1 | 3 |
| cp4_curb | chưa có | 0 / 1 | 3 |
| cp6_coverage | chưa có | 0 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Tôi đã Save và export đúng file ZIP cho các task đã hoàn thành. Các checkpoint còn lại chưa hoàn thành.

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: `medium_instance` — ảnh `000000458325.jpg`, vùng ở phía trái giữa khung hình, một vehicle nằm gần mép trái nhưng tách rõ khỏi vùng mặt đường và cảnh phía sau.
- Class và quy tắc tôi dùng để chọn biên: class `vehicle` (hoặc class tương ứng trong task), ranh biên được đặt theo phần vật thể nhìn thấy rõ nhất, không kéo qua vùng bị che hoặc nền. Tôi ưu tiên chốt mask theo hình dạng thật của xe, không tô thêm phần nền hoặc phần khuất.
- Nếu dùng gợi ý sau đó: không dùng gợi ý tự động cho object đầu tiên; tôi giữ quyết định mình xác định từ hình ảnh gốc, sau đó kiểm lại bằng cách phóng to và xem mép mask.
- Nếu không dùng gợi ý: không dùng; tôi vẫn giữ kiểu vẽ mỗi object riêng từ đường viền và thấy rõ ranh giữa các vật cùng class.

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: `medium_instance`, ảnh `000000458325.jpg`, vùng có hai object sát nhau phía trái giữa khung hình.
- Lỗi thuộc loại: gộp-tách.
- Bằng chứng tôi nhìn thấy: hai vật cùng class đứng gần nhau nhưng vẫn có khe sáng và mép tách rõ. Nếu vẽ chung một mask, vùng giữa sẽ bị lấn vào nhau và mất tách object.
- Quy tắc và hành động sửa: tôi dùng quy tắc “mỗi vật là một instance riêng” và tách mask bằng cách run lại polygon/brush trên borde giữa hai vật, giữ ranh rõ, không tô nối qua khe. Sau đó Save lại rồi export ZIP mới.
- Sau sửa đã Save và export lại chưa? Có. Tôi đã Save và export lại file ZIP sau khi sửa lỗi này.

Tôi đã kiểm Summary và kết quả tự đánh giá nếu có; nếu không có metric hay reference thì tôi ghi rõ trạng thái chưa có điểm từ scorer. Scorecard ba tier tối đa là 82, không phải tổng điểm cuối trên 100.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| Vùng road và sidewalk ở góc trái dưới | Gộp thành một vùng lớn hoặc tách theo mặt cắt đất | Chọn theo chức năng và ranh vật lý rõ nhất, không chỉ màu sắc | Tôi tách theo ranh thực tế giữa mặt đường và lề, vì có khác biệt hình thái và vị trí. |
| Hai vehicle cùng class sát nhau | Một object hoặc hai object | Nếu có khe giữa, mép rõ và không ăn vào nhau thì tách riêng | Quyết định: hai object riêng, mỗi mask theo một thân xe rõ ràng. |
| Vùng mờ hoặc bị che ở một object | Tô tiếp qua phần khuất hoặc dừng ở phần nhìn thấy | Không đoán phần ẩn; chỉ giữ phần thật nhìn thấy trong ảnh | Tôi dừng ở ranh nhìn thấy và không kéo thêm phần khuất theo suy đoán. |

Sau khi làm xong, tôi kiểm lại từng task trước khi upload ZIP lên `submissions/` và giữ bản Save ở CVAT local để tránh mất dữ liệu nếu có lỗi export.
