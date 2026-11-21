# Phiếu QC 1 — Road v1.0

**Lô được kiểm** Lê Tuấn Anh (`letuananh`)  
**Người kiểm** Lê Trần Phi Hùng (`letranphihung`)  
**Ngày kiểm** 21/11/2026 · **Quyết định** Rework

## Hai file được kiểm

| File | Thời điểm xuất trong XML (giờ Việt Nam) | Revision | SHA-256 |
| --- | --- | --- | --- |
| letuananh-front.zip | 21/11/2026 11:19:11 | Chưa có manifest để xác nhận | `4ed38f4c49585c1ff8e1b822974611e12fc43100362737bd88999fefd48a0978` |
| letuananh-fisheye.zip | 21/11/2026 11:20:06 | Chưa có manifest để xác nhận | `b1eb9f8fadbabb665e14916bf2c325373821e51eb86d1ca11286d77ec8a0d443` |

Cả hai ZIP mở được và mỗi ZIP chỉ chứa `annotations.xml` phiên bản 1.1. Toàn lô có 16 ảnh front và 12 ảnh fisheye. Cả 28 ảnh đủ bốn tag.

## Mẫu kiểm trên ảnh gốc

| Ảnh bắt buộc | Nội dung kiểm trực quan | Shape | Tag | Nhãn đã kiểm |
| --- | --- | ---: | ---: | ---: |
| A01 | Thứ tự làn, màu và kiểu vạch đôi | 9 | 4 | 13 |
| A05 | Đường giữa hai hàng xe đỗ, khả năng nhìn thấy L1/R1, mép vùng lái | 2 | 4 | 6 |
| A08 | Cụm đèn xe, đèn người đi bộ, biển phụ, crosswalk | 21 | 4 | 25 |
| B02 | Đèn vùng biên của camera trước fisheye | 6 | 4 | 10 |
| B03 | Điểm đầu và thuộc tính làn trên camera sau, hình vùng lái và box | 11 | 4 | 15 |
| B08 | Tâm vạch cong và `position=other` ở camera bên phải | 4 | 4 | 8 |
| **Tổng** | **3 front + 3 fisheye, 0 ảnh thêm** | **53** | **24** | **77** |

Đã chồng từng shape XML lên ảnh gốc cùng tên. Những chi tiết nhỏ như tín hiệu đèn, mép biển và nét sơn được xem phóng to. Tọa độ dưới đây tính theo pixel ảnh gốc, từ góc trên trái.

## Tổng hợp theo mã

| Mã | Nội dung | Loại lỗi | Số mục kiểm | Số lỗi | Mức theo guideline |
| --- | --- | --- | ---: | ---: | --- |
| QC1-01 | Tag camera và ba tag khác | Sai thuộc tính / bỏ sót | 24 tag | 0 | nặng / nhẹ |
| QC1-02 | L1/R1 nhìn thấy | Bỏ sót | 10 phía làn¹ | 0 | nặng |
| QC1-03 | Position và làn thừa | Sai thuộc tính / thừa | 12 lane | 0 | nặng |
| QC1-04 | Tâm vạch, chiều, điểm dừng | Sai phạm vi / bỏ sót | 12 lane | 1 | chính |
| QC1-05 | Vạch đứt, đoạn che | Sai phạm vi / sai thuộc tính | 12 lane | 0 | chính |
| QC1-06 | Style, color, kind | Sai thuộc tính | 12 lane | 2 | chính |
| QC1-07 | Vùng lái, crosswalk, road_curb | Sai phạm vi / bỏ sót / thừa | 19 hình | 2 | chính |
| QC1-08 | Đèn, màu và box | Bỏ sót / sai phạm vi / sai thuộc tính | 15 cụm² | 5 | nặng / chính |
| QC1-09 | Facing, type, group, biển phụ | Sai thuộc tính | 22 box | 0 | chính |
| QC1-10 | Biển thật, độ đầy đủ và box | Bỏ sót / sai phạm vi | 12 box biển³ | 1 | chính |
| QC1-11 | Fisheye, cung và edge_zone | Sai phạm vi / sai thuộc tính | 21 hình B | 1 | chính |
| QC1-12 | File, nhãn và thuộc tính | Lỗi file | 79 mục⁴ | 0 | nhẹ / nặng |

¹ Hai phía trên mỗi ảnh A01, A05, A08, B02, B03. Quy tắc làn của camera bên B08 được kiểm ở QC1-03.  
² 10 box đèn hiện có và 5 cụm bị bỏ sót đã xác nhận. Đèn thiếu được tính lỗi chính.  
³ Đã kiểm 12 box biển đã nộp và rà cảnh theo nhóm nhãn yêu cầu của từng ảnh.  
⁴ Hai ZIP và 77 nhãn trong mẫu. Một nhãn có thể xuất hiện ở nhiều hàng kiểm, nhưng chỉ xuất hiện một lần trong mẫu số 77.

## Danh sách lỗi

`#n` là thứ tự shape trong ảnh, tính từ 0 như `road_check`. Đối tượng bị bỏ sót được nhận diện bằng tọa độ tâm.

| # | Mã | Ảnh | Nhãn và vị trí | Mức | Quan sát và cách sửa |
| ---: | --- | --- | --- | --- | --- |
| 1 | QC1-06 | A01 | lane#5 (0,603) | chính | L2 ngoài trái là hai nét sơn trắng song song, thấy rõ ở mép trái ảnh. Đổi `kind=single` thành `double`. |
| 2 | QC1-07 | A08 | crosswalk#2 (1279,465) | chính | Biên dưới gần như nằm ngang trong khi mép capô xiên. Ở nửa trái, polygon ôm cả phần capô tối dưới các vạch trắng. Cắt polygon theo mép phần đường nhìn thấy. |
| 3 | QC1-08 | A08 | traffic_light thiếu, tâm ≈(433,237) | chính | Thiếu đèn người đi bộ đang sáng màu cam ở bên trái giao lộ. Vỏ cụm đèn cao trên 8 px. |
| 4 | QC1-08 | A08 | traffic_light thiếu, tâm ≈(780,236) | chính | Thiếu đèn người đi bộ có biểu tượng sáng trắng ở bên phải giao lộ, phía dưới đèn xe màu xanh. |
| 5 | QC1-08 | A08 | traffic_light thiếu, tâm ≈(611,208) | chính | Thiếu cụm đèn xe đang sáng xanh ở giữa ảnh, phía trên hàng xe xa. Vỏ đèn cao khoảng 12 px. |
| 6 | QC1-08 | A08 | traffic_light thiếu, tâm ≈(652,216) | chính | Thiếu cụm đèn xe có bóng xanh ở phía phải cụm (611,208). Có thể phân biệt cả vỏ trên ảnh gốc. |
| 7 | QC1-08 | B02 | traffic_light thiếu, tâm ≈(54,380) | chính | Thiếu cụm tín hiệu có bóng đỏ ở mép trái, ngang tầm vai người đi bộ. Hai box hiện có chỉ bao hai cụm phía trên. |
| 8 | QC1-07 | B03 | drivable#0 (278,648) | chính | Biên xa của `direct` vòng xuống dưới đuôi chiếc ô tô màu bạc, tạo chỗ lõm theo thân xe. Tô vùng mặt đường theo nguyên tắc “đi được nếu đường trống”. |
| 9 | QC1-04 | B03 | lane#4 (994,514) | chính | Điểm gần nhất của R1 dừng tại y≈514. Phần sơn trắng của vạch cùng làn còn thấy gần xe ở khoảng (925,590). Kéo điểm đầu về phần sơn gần còn thấy rồi lần theo cung. |
| 10 | QC1-06 | B03 | lane#5 (1023,426) | chính | Vạch ngoài bên phải gồm các vệt trắng có khoảng trống rõ ràng. Đổi `style=solid` thành `dashed` và giữ một polyline cho cả vạch. |
| 11 | QC1-10 | B03 | traffic_sign#10 (1122,358) | chính | Box của biển tròn mũi tên xanh trên trụ vàng lấy dư nền hai bên và một phần trụ bên dưới. Thu box sát mép biển bị biến dạng. |
| 12 | QC1-11 | B08 | lane#2 (66,752) | chính | Ở vệt trắng gần (205,625), polyline chạy dọc theo mép dưới thay vì tâm sơn. Sai lệch vượt 5 px tại đoạn rộng của vệt. Giữ `position=other` và đặt lại điểm trên tâm. |

## Ca mơ hồ

| Ảnh | Câu hỏi cho lead | Lời đáp |
| --- | --- | --- |
| A05 | Hai bên đường là hàng xe đỗ, mặt đường không thấy vạch phân làn. Coach có xác nhận miễn cả hai cảnh báo QC1-02 ở ảnh này không? | Chờ xác nhận. Chưa ghi hai cảnh báo thành lỗi. |

**Ảnh để coach kiểm lại** là A05. Trong hai shape và bốn tag của ảnh, chưa phát hiện lỗi xác định. Cần giữ riêng quyết định miễn QC1-02 của coach. Ba lỗi dễ định vị để đối chiếu là vạch trắng đôi A01, đèn đỏ bị bỏ sót tại mép trái B02 và polyline sát mép sơn B08.

## Quyết định

**Rework.** Mẫu có **0 lỗi nặng, 12 lỗi chính / 77 nhãn = 15,58%, 0 lỗi nhẹ** và **1 ca chờ lead**. Tỉ lệ lỗi chính vượt ngưỡng 5% ngay cả khi coach miễn cả hai cảnh báo A05.

Lint toàn lô đưa ra 5 cảnh báo QC1-02 ở A05 (hai phía), A06 (R1), A07 (L1) và A09 (R1). Danh sách lỗi trên chỉ tính các lỗi đã xác nhận trực quan trong sáu ảnh mẫu. [Thang mức lỗi và luật quyết định](../docs/qc_severity_road_v1.0.md)
