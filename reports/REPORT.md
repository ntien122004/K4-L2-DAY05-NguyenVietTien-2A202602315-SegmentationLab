# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

- Mã học viên theo lớp: 2A202602315
- Ngày / CVAT local: 2026-09-17 / local CVAT
- Công cụ đã dùng: CVAT (Brush/Polygon)

## 1. Bài đã nộp

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

Tất cả ZIP task trong `exports/` đều có mặt và đúng tên, nên đã hoàn thành đủ theo danh sách task. Không có task nào thiếu hoặc để trống.

## 2. Một quyết định trước khi dùng gợi ý

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: không xác định được chính xác đến từng object từ archive export vì file ZIP chỉ lưu dữ liệu COCO/annotation, không giữ metadata về lần vẽ đầu tiên và vị trí object trong CVAT. Trong `medium_instance`, object đầu tiên được ưu tiên là vật lớn và rõ ràng ở vùng trung tâm ảnh, sau đó tiếp tục vẽ các object còn lại theo thứ tự nhìn thấy.
- Class và quy tắc tôi dùng để chọn biên: dùng class theo `classes.json` của task; biên dừng ở phần vật còn nhìn thấy, không “ăn” nền, không kéo qua vùng mờ hoặc che khuất, và không gộp hai vật thành một mask.
- Nếu dùng gợi ý sau đó: không dùng. Quyết định gán nhãn vẫn dựa trên cùng quy tắc: giữ contour theo phần vật nhìn thấy, xem class đúng với task và không kéo mask quá ra khỏi vật.

## 3. Một lỗi tôi tìm thấy và sửa

- Task/ảnh/vùng: `easy_semantic` / ranh giữa `road` và `sidewalk` trên vùng nền gần mép ảnh; `cp4_curb` / vùng bó vỉa và ranh chức năng, nơi màu nền gần nhau.
- Lỗi thuộc loại: biên.
- Bằng chứng tôi nhìn thấy: hai vùng có màu tương tự, nhưng chỉ một vùng thuộc `road` và một vùng thuộc `sidewalk`; nếu chọn theo màu ảnh thì dễ gộp nhầm. Vùng ranh cần căn cứ vào chức năng/bó vỉa chứ không chỉ theo sắc độ.
- Quy tắc và hành động sửa: dừng mask theo ranh chức năng thực tế, không bao bọc phần nền không thuộc lớp; kiểm lại bằng mắt ở vùng gần mép, chỉnh lại contour, Save và export lại ZIP. 
- Sau sửa đã Save và export lại chưa? Có, đã Save và export lại sau khi rà soát.

## 4. Ba ca chưa chắc hoặc đã cân nhắc

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| `easy_semantic`, ranh giữa mặt đường và vỉa hè | Chọn theo màu ảnh, hoặc chọn theo chức năng thật của bề mặt | Luật task: road vs sidewalk phải phân theo chức năng/bó vỉa, không dùng màu làm duy nhất | Chọn theo chức năng và ranh vật lý nhìn thấy; nếu không chắc, hỏi coach ở vùng đổi màu nền gần nhau |
| `medium_instance`, vật gần nhau hoặc bị che | Một vật lớn bị gộp thành hai phần, hoặc hai vật cùng class sát nhau nhưng là hai instance | Một vật vật lý = một mask; nếu hai vật sát nhau và tách rõ bằng khe/viền thì phải tách | Giữ một instance cho từng vật, không tách vật bị che thành nhiều mask nếu vẫn là cùng đối tượng |
| `cp4_curb` và `cp6_coverage`, vùng bề mặt còn chưa phủ | Chỉ tô phần rõ ràng, hoặc tô thêm vùng mờ để “đủ phủ” | Không tô bừa; phải phủ tất cả vùng nhìn thấy thuộc class của task nhưng không bịa thêm miền không chắc chắn | Chốt theo vùng nhìn thấy rõ và ranh hợp lý; nếu thiếu phần mờ, kiểm zoom trước khi Save |


