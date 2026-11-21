# Báo cáo cá nhân — Ngày 11 · Road Elements & SVM

| Thông tin | Giá trị |
| --- | --- |
| Họ tên · mã học viên | Lê Trần Phi Hùng · 2A202603353 |
| Tên bàn giao | `letranphihung` |
| Công cụ · project | CVAT 2.81.0 trên máy làm bài · Day11 (#36) |
| Guideline · lần bàn giao | Road v1.0 · đã chuẩn bị revision 2 sau một vòng Rework |
| Thời điểm xuất CVAT của revision 1 · front và fisheye | 21/11/2026 lúc 11:04:20 và 11:05:34 (giờ Việt Nam, theo `meta/dumped`) |
| Job đã kiểm tra trong CVAT | Job 20 (front) và job 21 (fisheye) đều ở trạng thái `completed` |

## 1. Kiểm project

| Hạng mục | Kết quả đối chiếu |
| --- | --- |
| Sáu label | `lane`, `road_curb` dùng polyline. `crosswalk`, `drivable` dùng polygon. `traffic_light`, `traffic_sign` dùng rectangle. |
| Thuộc tính | Các tên thuộc tính, kiểu và tập giá trị trong XML khớp taxonomy Road v1.0. Các thay đổi thuộc tính trong revision 2 được ghi ở mục 4 và 5. |
| Bốn tag | 28/28 ảnh có đủ `camera`, `timeofday`, `weather`, `source`. Đã sửa bốn giá trị camera sai. Revision 2 có 28/28 tag camera đúng. |

Để kết thúc polyline, chọn **Polyline → Shape**, đặt điểm từ gần xe ra xa rồi nhấn **N**. Để gắn tag, chọn công cụ **Tag**, đặt `camera` trước, chọn thuộc tính `value` trong **Details**, sau đó lặp lại với ba tag còn lại và lưu bằng **Ctrl+S**. [Hướng dẫn thao tác của lab](docs/huong_dan_cvat_bang_hinh.md)

## 2. Số lượng đã gán

| Nhóm ảnh | Ảnh | lane | road_curb | crosswalk | drivable | traffic_light | traffic_sign |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| Front bắt buộc A01–A12 | 12 | 34 | 23 | 7 | 26 | 45 | 48 |
| Front thêm A13–A16 | 4 | 12 | 9 | 3 | 9 | 12 | 16 |
| Fisheye bắt buộc B01–B08 | 8 | 14 | 29 | 15 | 17 | 7 | 10 |
| Fisheye thêm B09–B12 | 4 | 2 | 12 | 8 | 9 | 0 | 0 |
| **Tổng** | **28** | **62** | **73** | **33** | **61** | **64** | **74** |

Hai ZIP có **367 shape** và **112 tag**. Tám ảnh thêm được đếm riêng với hai mươi ảnh bắt buộc.

## 3. Bảng ca khó

| Ảnh | Nhãn | Hai khả năng | Lựa chọn trong bản export | Căn cứ khi xem ảnh |
| --- | --- | --- | --- | --- |
| A05 | lane | Vẽ L1/R1 theo hai hàng xe đỗ hoặc để trống | Để trống cả hai phía | Không thấy sơn phân làn. Hàng xe đỗ giúp xác định phần đường đi được, nhưng không cho biết tâm vạch sơn. Cần coach xác nhận miễn QC1-02. |
| B08 | lane#7 | Dùng L1/R1 hoặc `other` | `other` | Hậu tố MVR là camera bên phải. Ở góc nhìn bên, guideline yêu cầu dùng `other`. |
| B11 | lane trong khu vực ô đỗ | Gán biên ô đỗ thành lane hoặc bỏ qua | Không tạo lane | Các nét bao ô đỗ dành cho người khuyết tật. Đường bao chỗ đỗ không phải biên làn xe chạy. Tag camera đã được sửa thành `fisheye_rear`. |

## 4. Phiếu QC 1 nhận được và kết quả sửa

**Lê Tuấn Anh quyết định Rework ngày 21/11/2026.** Phiếu revision 1 kiểm A01, A04, A08, A11, B02 và B06. Kết quả là **4 lỗi nặng, 18 lỗi chính trên 122 nhãn (14,8%) và 0 lỗi nhẹ**. [Phiếu Tuấn Anh lập cho lô của Hùng](letuananh-qc1-for-letranphihung.md)

| Mã | Số lỗi trên phiếu | Phản hồi và thay đổi trong revision 2 |
| --- | ---: | --- |
| QC1-01 | 2 nặng | Đồng ý. Sửa B02 thành `fisheye_front`, B06 thành `fisheye_left`. Kiểm cả lô và sửa thêm B10 thành `fisheye_left`, B11 thành `fisheye_rear`. |
| QC1-03 | 1 nặng | Đồng ý. Đổi vạch đứt sát bên phải ở A01 từ L2 thành R1. |
| QC1-04 | 2 chính | Chỉnh điểm đầu và đường đi của R1 ở A08 theo vệt sơn trước vùng qua đường. Vạch vàng đôi gộp ở A11 được vẽ từ gần ra xa. |
| QC1-05 | 3 chính | Nối hai đoạn L2 ở A08. Gộp mỗi cặp vạch vàng đôi ở A04 và A11 thành một polyline giữa hai nét, giữ `kind=double`. |
| QC1-06 | 1 chính | Đề nghị coach xem lại lỗi #10 của phiếu. Ảnh phóng to ở mép trái A01 có hai nét trắng song song. Đặt polyline vào giữa hai nét và giữ `kind=double`. |
| QC1-07 | 1 chính | Cắt bớt polygon phía phải của A11 khỏi dải xe đỗ và đổi loại vùng còn lại thành `alternative`. |
| QC1-08 | 1 nặng + 1 chính | Đổi đèn #16 ở A08 thành `green`. Đèn #7 nhìn nghiêng, đổi `red` thành `unknown` do ảnh không cho thấy rõ bóng đang sáng. |
| QC1-09 | 5 chính | Sửa hướng đèn A08 #7 và #8 thành `side`. Hai box #18 và #28 phủ hai phần của cùng một cụm, đã gộp thành một box `front/green`. Sửa nhóm biển A08 #30 và A04 #7 thành `information`. |
| QC1-10 | 1 chính | Thêm box riêng `supplementary` cho biển “PASSENGER CARS ONLY” phía trên biển chỉ hướng ở A08. |
| QC1-11 | 4 chính | Bật `edge_zone` cho ba đèn ở B02 và cắt phần nhọn của polygon vùng lái ở B06 theo mép thân xe. |

Các số shape trong bảng tham chiếu revision 1. Bộ revision 2 đã sửa nhãn trong XML của hai ZIP, giữ nguyên định dạng CVAT for images 1.1. Tuấn Anh chưa kiểm lại revision 2. Lô của Tuấn Anh có phiếu riêng. [Phiếu Hùng lập cho Tuấn Anh](letranphihung-qc1-for-letuananh.md)

## 5. Tự kiểm hai ZIP revision 2

| File | Đọc lại và định dạng | Ảnh 1, số shape job cũ / XML revision 2 | Ảnh 2, số shape job cũ / XML revision 2 | Camera trong XML revision 2 |
| --- | --- | --- | --- | --- |
| letranphihung-front.zip | CVAT for images 1.1, chỉ có annotations.xml | A02 · 8 / 8 | A12 · 17 / 17 | 16/16 đúng |
| letranphihung-fisheye.zip | CVAT for images 1.1, chỉ có annotations.xml | B01 · 8 / 8 | B06 · 13 / 13 | 12/12 đúng |

Ngoài các lỗi trên phiếu, đã bổ sung L1 ở A03 và A13, sửa màu hai đèn xanh ở A03, chỉnh vùng lái sát capô ở A13, sửa `weather=rain` ở A07 và A10, gộp vạch đôi ở A14, cập nhật thứ tự làn ở A15, đảo chiều hai polyline ở B03 và xóa điểm trùng ở B01 và B07.

**Lint revision 2 còn 7 cảnh báo nặng, 7 chính và 9 nhẹ**. Revision 1 có 14 nặng, 18 chính và 11 nhẹ. Cả hai ZIP mở được, giữ đúng 28 ảnh và đủ 112 tag.

- Bảy cảnh báo nặng đều là QC1-02 gồm A05 (L1 và R1), A06 (R1), A07 (L1), A09 (R1), A10 (R1) và A14 (R1). A05 không thấy vạch sơn ở cả hai bên. Các ảnh còn lại có lề, dải chắn, xe che hoặc ảnh tối. Cần coach xem từng ảnh trước khi xác nhận miễn hoặc yêu cầu bổ sung.
- Bảy cảnh báo chính gồm trùng L2 ở A09 và sáu polyline ngắn trên B05, B07, B09, B11, B12. Cần đối chiếu tâm vạch hoặc mép lề trên ảnh ở độ phân giải gốc. Số điểm ít trên một đoạn ngắn chưa đủ để kết luận đường vẽ lệch.
- Chín cảnh báo nhẹ là các box thấp hơn 8 px, dùng để rà soát theo guideline.

### Manifest bàn giao

Manifest revision 2 lấy số byte và SHA-256 từ hai ZIP đã sửa. Hai ZIP và manifest revision 1 được lưu trong `r1/`. Bản sửa thực hiện trực tiếp trên XML export. Số shape trong cột job và ảnh chụp ở mục 6 lấy từ CVAT trước khi sửa. Chưa kiểm lại bằng cách nhập revision 2 vào CVAT. [Manifest revision 2](letranphihung-manifest.json)

## 6. Ảnh chụp màn hình

**1. Project Day11, bảng Labels.** Sáu label hình và bốn tag hiển thị trong Constructor.

Bản chụp lưu trên máy làm bài tại `docs/images/letranphihung/01-project-labels.png`.

**2. Front A12, job 20.** Trên ảnh có làn, vùng lái, vạch qua đường và các box đèn, biển. Bảng Objects hiển thị 21 mục gồm 17 shape và 4 tag.

Bản chụp lưu trên máy làm bài tại `docs/images/letranphihung/02-front-a12.jpg`.

**3. Fisheye B06, job 21.** Bên phải là phần phóng to từ cùng ảnh chụp màn hình để nhìn các polyline theo cung. Ảnh chụp từ job trước khi cập nhật tag camera trong revision 2.

Bản chụp lưu trên máy làm bài tại `docs/images/letranphihung/03-fisheye-b06.jpg`.

**4. XML trong bản export front.** Đoạn trích của `lane#9` ở A11 cho thấy dãy điểm và bốn thuộc tính của vạch R1 dùng ở mục 8.

Bản chụp lưu trên máy làm bài tại `docs/images/letranphihung/04-xml-a11.png`.

## 7. Tự đánh giá

Trong sáu ảnh Tuấn Anh kiểm, **QC1-09 xuất hiện nhiều nhất với 5 lỗi**, gồm ba lỗi về hướng đèn ở A08 và hai lỗi về nhóm biển ở A04, A08. Ngày 20, em sẽ phóng to từng cụm đèn, xác định hướng của cả vỏ trước khi chọn màu, rồi đọc chức năng của biển trước khi chọn `group`. Với ảnh fisheye, em sẽ đối chiếu hậu tố FV, RV, MVL hoặc MVR với `camera.value` ngay khi mở frame. Cách kiểm này nhắm trực tiếp vào hai lỗi nặng ở B02 và B06.

Một chỗ em đã bổ sung trong quá trình gán là vùng lái ở **B06**. Trong bản COCO của job 21 ngày 20/11, B06 có hai polygon `crosswalk` và chưa có polygon `drivable`. Bản CVAT XML ngày 21/11 đã có hai polygon `drivable` (#1 và #5). Nếu mặt đường đi được bị bỏ trống trong dữ liệu huấn luyện phân vùng, pixel ở đó có thể bị dùng làm nền và mô hình có thể dự đoán thiếu vùng lái. Việc bổ sung polygon xử lý phần bỏ sót trước khi đổi lô. Trong revision 2, em đã cắt polygon #1 theo mép thân xe và sửa tag thành `fisheye_left`. Nhờ vậy, pixel thân xe ít bị gán nhầm thành vùng lái và ảnh được đưa vào đúng nhóm camera.

## 8. Từ polyline đến đích row-anchor

Chọn **R1 trên A11** vì cả Hùng và Tuấn Anh đều có đúng một polyline mang position này. Bảng dùng phép nội suy của `road_lab.rowanchor`, đơn vị pixel, làm tròn một chữ số.

| Tọa độ x tại hàng y | 720 | 680 | 640 | 600 | 560 |
| --- | ---: | ---: | ---: | ---: | ---: |
| Hùng · R1 | 908,6 | 866,2 | 823,7 | 781,3 | 740,2 |
| Tuấn Anh · lane#2 | Không có làn | 865,6 | 823,2 | 780,9 | 738,6 |

1. Nếu Hùng dừng sớm 40 px **ở đầu xa** theo trục y, đầu xa đổi từ y=434,72 thành y=474,72. Cả năm hàng trong bảng vẫn cắt polyline. Nếu rút 40 px **ở đầu gần**, từ y=720 xuống y=680, riêng hàng 720 đổi thành “không có làn”.
2. Ở y=560, hai bản lệch **1,6 px**. Ở y=720, chỉ Hùng có giao điểm vì điểm đầu của Tuấn Anh ở y=719. Hàng gần nhất có thể so trực tiếp là y=719, lệch **0,7 px**. Với hỗ trợ giữ làn (LKA), hàng gần xe đặc biệt quan trọng vì nó mô tả vị trí ngang của xe so với biên làn ngay trước đầu xe.
3. Đảo thứ tự các điểm trên R1 này vẫn cho cùng tọa độ x ở từng hàng. Tuy vậy, điểm đầu và điểm cuối bị đổi vai trò, nên `road_check lint` và phép kiểm chiều gần–xa của `road_check check` sẽ phát hiện sai chiều.
