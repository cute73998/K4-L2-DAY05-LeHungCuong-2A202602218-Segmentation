# Báo cáo Day 5 — điền trực tiếp trong fork của bạn

**Cách dùng:** Thay mọi dấu `…` bằng bài làm thật của bạn trước khi nộp link fork trên VLearn. Giữ nguyên bốn mục và bảng để coach đọc nhanh. Viết ngắn, cụ thể theo ảnh/vùng; không cần thuật ngữ chuyên sâu. Ví dụ trong [hướng dẫn mẫu](REPORT_TEMPLATE.md) chỉ giúp hiểu cách điền, không phải câu trả lời để chép lại.

- Mã học viên theo lớp: 2A202602218
- Ngày / CVAT local: 17/09/2026 / CVAT Web/Local UI
- Công cụ đã dùng: Brush / Polygon / Intelligent Scissors / CVAT Export (Segmentation Mask 1.1, COCO 1.0)

Mã học viên là mã lớp cấp; không cần ghi họ tên trong report nếu kênh VLearn đã nhận diện bạn. Chỉ ghi công cụ thật sự đã dùng; không có SAM vẫn làm bài bình thường.

## 1. Bài đã nộp

Ghi tên ZIP đúng như file trong `submissions/` và số ảnh đã vẽ, Save. Chưa làm hoặc export lỗi thì ghi `chưa có`, không tạo ZIP rỗng. Cột điểm là điểm tối đa của task, **không phải điểm tự chấm**.

| Task | File ZIP đúng tên | Hoàn thành mấy ảnh | Điểm tối đa (coach chấm sau) |
| --- | --- | ---: | ---: |
| easy_semantic | easy_segment.zip | 3 / 3 | 20 |
| medium_instance | medium_segment.zip | 3 / 3 | 32 |
| hard_panoptic | hard_panoptic.zip | 2 / 2 | 30 |
| cp1_holes | chưa có | 0 / 1 | 3 |
| cp2_slice | chưa có | 0 / 1 | 3 |
| cp5_occlusion | chưa có | 0 / 1 | 3 |
| cp3_thin | chưa có | 0 / 1 | 3 |
| cp4_curb | chưa có | 0 / 1 | 3 |
| cp6_coverage | chưa có | 0 / 1 | 3 |
| **Tổng tối đa** | | | **100** |

Nếu export lỗi, ghi task, dữ liệu đã Save đến đâu và lỗi đã báo coach.

## 2. Một quyết định trước khi dùng gợi ý

Chọn object đầu tiên bạn tự vẽ ở `medium_instance`, trước khi xem bất kỳ đề xuất tự động nào cho object đó. Ghi ảnh/vị trí đủ để tìm lại; “quy tắc biên” là lý do bạn chọn hoặc dừng mask ở ranh đó.

- Ảnh, vị trí và object Medium đầu tiên tự vẽ: Ảnh `000000181542.jpg` (task `medium_instance`), đối tượng xe buýt (`bus`) nằm sát mép trái ảnh (vị trí bbox `x=0.0, y=62.0, w=205.0, h=134.0`).
- Class và quy tắc tôi dùng để chọn biên: Class `bus`. Quy tắc ranh giới: Dừng mask chính xác tại mép cắt của bức ảnh (`x=0`), không kéo viền tràn ra ngoài khung hình. Với phần thân xe bị các đối tượng phía trước (người đi bộ `person` và xe máy `motorcycle`) che khuất một phần, áp dụng quy tắc ranh giới nhìn thấy được (visible boundary): chỉ mask phần thân xe nhìn thấy rõ, cắt ranh giới theo viền ngoài của vật che, không suy đoán hay tô đè lên phần bị che.
- Nếu dùng gợi ý sau đó: Mô hình gợi ý tự động (auto-annotation/SAM) bị lỗi tràn ranh giới ra khỏi khung ảnh (`x < 0`) và dính liếm sang vùng bóng râm/mặt đường dưới gầm xe. Hành động: Đã kiểm tra trực quan, dùng Polygon/Brush cắt bỏ phần diện tích thừa tràn nền và tinh chỉnh ranh giới bám sát thân xe thực tế.
- Nếu không dùng gợi ý: không dùng; vẫn giải thích đúng theo các bước tự vẽ đối tượng đầu tiên nêu trên.

## 3. Một lỗi tôi tìm thấy và sửa

Chọn một lỗi **có thật** trong bài. Nếu công cụ lỗi khiến bạn chưa sửa được, ghi rõ đã thử gì và cần coach hỗ trợ gì; không ghi “đã sửa” khi chưa sửa.

- Task/ảnh/vùng: Task `medium_instance`, ảnh `000000373353.jpg`, khu vực giữa các xe ô tô và xe tải đỗ sát nhau.
- Lỗi thuộc loại: gộp-tách và sai lớp (Merge/Split & Class misclassification).
- Bằng chứng tôi nhìn thấy: Hai chiếc xe đỗ liền kề (xe tải `truck` id 39 và xe ô tô `car` id 34) ban đầu bị gợi ý tự động gộp chung thành một mask duy nhất do màu sắc và bóng râm tương đồng. Ngoài ra một chiếc xe tải nhỏ ban đầu bị nhầm thành nhãn `car`.
- Quy tắc và hành động sửa: Áp dụng quy tắc Instance Segmentation: mỗi đối tượng vật lý riêng biệt phải là một mask độc lập. Dùng công cụ Split/Polygon tách đôi ranh giới ở khe hở giữa hai xe, đồng thời đổi lại class chuẩn `truck` dựa trên hình dáng thùng xe.
- Sau sửa đã Save và export lại chưa?: Đã Save trực tiếp trên CVAT và export lại file ZIP `medium_segment.zip` vào thư mục `submissions/`.
- Nếu bạn **đã xem Summary tự đánh giá trên GitHub Actions hoặc tự chạy script**, ghi ngắn một kết quả liên quan lỗi vừa sửa: Đã kiểm tra dữ liệu `medium_segment.zip` (3/3 ảnh, 71 annotations COCO 1.0 không có lỗi cấu trúc). Do repo học viên không giữ ground truth (`require_reference` báo thiếu dữ liệu tham chiếu), điểm số IoU/mAP chính thức là `chưa có` (chờ coach chấm trên hệ thống).

## 4. Ba ca chưa chắc hoặc đã cân nhắc

Mỗi ca là một **vùng cụ thể** khiến bạn phải cân nhắc hai cách hiểu. Ghi dấu hiệu nhìn thấy hoặc quy tắc đã dùng, rồi nêu quyết định hoặc câu hỏi cho coach. Không cần ba lỗi; ca đã quyết định được cũng hợp lệ.

| Ảnh/vị trí | Hai cách hiểu có thể | Quy tắc/chứng cứ | Quyết định hoặc câu hỏi cho coach |
| --- | --- | --- | --- |
| `easy_semantic` / ảnh `817bca71-00000000.jpg`, vùng mép đường và vỉa hè | (1) Gán vùng bó vỉa mòn xám thành `road` do màu bêtông/bụi mờ giống mặt đường; (2) Gán thành `sidewalk` do nền đường nâng cao hơn mặt đường. | Quy tắc Semantic Segmentation: Phân định ranh giới road vs sidewalk dựa trên chức năng giao thông (functional boundary) thay vì màu sắc pixel. | Quyết định gán vùng bó vỉa nâng cao là `sidewalk`. Xin coach xác nhận quy tắc phân định tại các đoạn bó vỉa bị mòn phẳng với mặt đường. |
| `medium_instance` / ảnh `000000458325.jpg`, xe ô tô bị biển báo che ngang | (1) Vẽ 2 mask riêng biệt cho phần đầu và đuôi xe nhìn thấy; (2) Nối liền 1 mask đè qua biển báo; (3) Lưu 1 annotation object chứa 2 polygon vùng nhìn thấy. | Quy tắc Occlusion (vật bị che khuất): Đối tượng bị vật khác cắt ngang vẫn là một instance duy nhất, chỉ gán nhãn các vùng quan sát được (visible pixels). | Quyết định tạo 1 annotation object `car` chứa multipolygon cho 2 vùng nhìn thấy của chiếc xe. |
| `hard_panoptic` / ảnh `000000350023.jpg`, khu vực đèn giao thông nhỏ nằm trên nền bầu trời/tòa nhà | (1) Đèn giao thông (`traffic light`) là Thing nên vẽ cắt lỗ background; (2) Phủ kín Stuff làm nền (`sky`, `building`) rồi gán Thing đè lên trên. | Quy tắc Panoptic Segmentation: Stuff bao phủ toàn bộ diện tích phông nền, Thing đếm được nằm đè phía trên, không để lại khoảng trống (no unlabeled gap). | Quyết định phủ kín lớp Stuff (`sky`, `building`) làm phông nền, sau đó gán 6 mask Thing cho `traffic light`. Xin coach xác nhận quy tắc ưu tiên mask trong Panoptic COCO 1.0 export. |
