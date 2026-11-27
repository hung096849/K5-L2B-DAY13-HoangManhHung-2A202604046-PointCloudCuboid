# Báo cáo Day 13 — Hoàng Mạnh Hùng, 2A202604046

## 1. Kết luận trước
Mã số 2A202604046, task D13-2A202604046, 10 frame: s01f05, s01f46, s02f11, s02f36, s03f14, s03f42, s04f02, s04f49, s05f03, s05f28 (từ kết quả dry).
Hệ điều hành: macOS. Trình duyệt, phiên bản CVAT: 2.8
Tổng cuboid: 69. frame ít nhất, số cờ (8/69)

Cách dùng CVAT: Đổi góc nhìn (nhìn từ trên xuống, xoay, dời); vẽ và chỉnh một cuboid (Shape, nháy đúp, Top/Side/Front); bật mũi tên hướng (Cuboid orientation) và xem cuboid trên ảnh camera. Ngoài ra còn dùng các phím như U, I, O, J, K, L để điều chỉnh góc nhìn 

## 2. Đọc frame (Bước 2)

| Frame | Phía trước xe ở cạnh nào của ảnh nhìn từ trên xuống; +x, +y chỉ về đâu | Mặt đường (phẳng/dốc; thấp hơn LiDAR ~ m) | Vật xa nhất đọc được (~ m) | Loại vật thấy | Vật phía sau / hai bên | Cụm chưa đọc được |
|---|---|---|---|---|---|---|
| s01f05 | Phía trước xe ở phía trên, +x, +y chỉ về bên phải | Mặt đường phẳng, thấp hơn LiDAR ~ 0.5m| Khoảng 20m | Xe | Xe con, xe buýt | Chưa đọc được | 
| s01f46 | Phía trước xe ở phía dưới, +x, +y chỉ về bên phải | Mặt đường phẳng, thấp hơn LiDAR ~ 0.5m| Khoảng 20m | Xe | Xe con | Chưa đọc được | 

3 điều **chỉ camera** cho biết và 3 điều **chỉ LiDAR** cho biết, lấy từ chính hai frame trên.
**Camera** 
1. Màu sắc của vật thể: Camera có thể nhận biết màu sắc thực tế của các vật thể, chẳng hạn màu quần áo, màu xe hoặc màu biển báo.
2. Chi tiết hình ảnh và hoa văn: Camera có thể ghi nhận chữ viết, ký hiệu, biển báo và các chi tiết bề mặt mà dữ liệu LiDAR trong hai frame không thể hiện rõ.
3. Đặc điểm nhận dạng trực quan: Camera có thể cung cấp thông tin về khuôn mặt, kiểu dáng, trang phục và các đặc điểm ngoại hình của con người.
**LiDAR**
1. Khoảng cách chính xác đến vật thể: LiDAR đo khoảng cách từ cảm biến đến các điểm trong môi trường, giúp xác định vật thể ở gần hay xa.
2. Cấu trúc không gian 3D: LiDAR tạo ra Point Cloud, thể hiện hình dạng, vị trí và kích thước không gian của các vật thể.
3. Độ sâu và hình dạng địa hình: LiDAR có thể ghi nhận độ cao, độ sâu, bề mặt và sự thay đổi địa hình, kể cả khi không có thông tin màu sắc.

## 3. Ba cuboid có hướng dẫn (Bước 3)
Ảnh: `t2-a-car`, `t2-b-bus-truck`, `t2-c-ped-cyclist`.

| Vật | Frame, idx | Class | Mũi tên yaw đúng hướng? | Đáy chạm đường? | Đã kiểm Top/Side/Front + ảnh? | Kích thước trong bảng 4.3? |
|---|---|---|---|---|---|---|
| (a) xe con gần | (chưa có, không thấy trong ảnh) | Car | Có vẻ đúng: mũi tên đỏ chạy dọc trục dài của hộp, khớp đám điểm. Chưa xác nhận được đầu/đuôi xe | Gần đúng: đáy ngang mức các vòng quét mặt đường. Cần kiểm lại ở Side view | Chưa đủ: mới có 1 góc phối cảnh (`t2-a-car`). Thiếu Top/Side/Front | Chưa kiểm (thiếu bảng 4.3). Hộp khoảng 4–5 m, có vẻ hơi cao so với phần điểm bên trong |
| (b) xe buýt hoặc xe tải | (chưa có, không thấy trong ảnh) | Bus/Truck (hộp rất dài, dài:rộng khoảng 5:1) | Cần chỉnh: mũi tên đỏ nhô ra ở mép dưới nên đầu xe hướng xuống. Hộp hơi nghiêng so với đám điểm, nên chỉnh yaw vài độ | Chưa đánh giá được (Top view không thấy cao độ). Cần Side view | Chưa đủ: mới có Top view (`t2-b-bus-truck`). Thiếu Side/Front | Chưa kiểm (thiếu bảng 4.3). Khoảng 12 × 2,5 m là hợp lý cho xe buýt |
| (c) người đi bộ hoặc xe hai bánh | (chưa có, không thấy trong ảnh) | Pedestrian (đám điểm là cột dọc mảnh, không có dạng bánh/khung xe) | Chưa xác nhận: mũi tên đỏ ngắn, bị rút gọn do phối cảnh Front view | Gần đúng: đáy ngang mức điểm mặt đường nhưng hơi trũng. Cần kiểm lại ở Side view | Chưa đủ: mới có Front view (`t2-c-ped-cyclist`). Thiếu Top/Side | Chưa kiểm (thiếu bảng 4.3). Hộp trông rộng so với một người, cần đối chiếu kích thước người đi bộ |

pointcloud/s03f42.pcd / vòng hiệu chuẩn: "không có".

## 4. Bốn hiện tượng chất lượng (Bước 4)
Nguyên nhân ghi là **giả thuyết**. Frame mượn thì ghi rõ nguồn: task chung, hoặc MSSV của bạn cho mượn.

| Hiện tượng | Frame / tên ảnh / nguồn | Nguyên nhân (giả thuyết) | Thấy gì trên point cloud | Hệ quả khi gán (theo guideline) |
|---|---|---|---|---|
| 1. Bóng che | pointcloud/s03f42.pcd / t4-1-bong-che |Giả thuyết: Có thể bị xe sau che khuất | Thấy mờ |
| 2. Thưa ở xa | t4-2-thua-o-xa | thấy một xe ở khoảng cách khá xa, khoảng cách càng xa, xew càng mờ | không gán |
| 3. Phản xạ thấp / ít điểm bất thường | t4-3-phan-xa-thap  | | |
| 4. Điểm lạ | t4-4-diem-la | | |

## 5. Số từ cuboid của tôi (Bước 5)
Lấy từ `submission/D13-2A202604046-bangA.csv`; số điểm lấy từ cột `points_in` của `-tukiem.csv`. Mặt đường khoảng −1,7 m: z đáy lệch hơn 0,3 m là lơ lửng hoặc chìm.

| Cuboid (frame, idx, class) | z | h | z đáy = z − h/2 | dist_m | r = √(x²+y²+z²) | yaw_deg | Nhận xét (lơ lửng / chìm / chạm đường) |
|---|---|---|---|---|---|---|---|
| pointcloud/s03f42.pcd / 1, Car | −0,888 | 1,672 | −1,724 | 8,07 | 8,12 | 267,9 | Chạm đường (lệch 0,02 m) |
| pointcloud/s03f42.pcd / 3, Car | −0,799 | 1,781 | −1,689 | 24,44 | 24,46 | 274,0 | Chạm đường (lệch 0,01 m) |
| pointcloud/s03f42.pcd / 4, Car (occluded=1) | −0,616 | 1,176 | −1,204 | 30,62 | 30,62 | 95,1 | Lơ lửng (đáy cao hơn mặt đường 0,50 m, vượt ngưỡng 0,3 m) |

pointcloud/s03f42.pcd / (a) cuboid **gần nhất** là idx 1: `dist_m` = 8,07 m, r = 8,12 m. Cuboid **xa nhất** là idx 4: `dist_m` = 30,62 m, r = 30,62 m. Khung này chỉ có 4 Car (idx 1 đến 4), nên idx 1 và idx 4 là hai đầu khoảng cách.
(b) Frame s03f42 không có người đi bộ hay xe hai bánh, nên chưa có số liệu để so. Trong file chỉ có Pedestrian/Cyclist ở các frame khác (ví dụ Cyclist s02f11 idx 8: l 1,256, w 0,796, h 1,828). Xe con trong frame này: l = 4,49 / 4,52 / 4,73 / 3,20 m; w = 2,13 / 1,82 / 1,78 / 1,22 m; h = 1,67 / 1,69 / 1,78 / 1,18 m. Tôi chưa có bảng 4.3 nên chưa kết luận được chiều nào ngoài khoảng. Có hai chiều đáng nghi: idx 4 ngắn và hẹp bất thường (3,20 × 1,22 × 1,18 m), nhiều khả năng do bị che khuất (occluded = 1) nên hộp bị co lại; idx 1 có w = 2,13 m, hơi rộng so với xe con thông thường.
(c) Ba xe có yaw gần 270°: idx 1 = 267,9°, idx 2 = 264,4°, idx 3 = 274,0° (lệch tối đa khoảng 6°). Xe ngược chiều là idx 4: yaw = 95,1°, lệch 5,1° so với 90°. Vậy cả hai nhóm đều gần 270° và 90°, nhưng idx 4 là cuboid bị che khuất và lơ lửng nên yaw của nó kém tin cậy.

Mật độ theo khoảng cách (4 cuboid ở các `dist_m` khác nhau):

| Cuboid (frame, idx, class) | dist_m | Số điểm (`points_in`) |
|---|---|---|
| pointcloud/s03f42.pcd / 1, Car | 8,07 | [điền từ -tukiem.csv] |
| pointcloud/s03f42.pcd / 2, Car | 10,69 | [điền từ -tukiem.csv] |
| pointcloud/s03f42.pcd / 3, Car | 24,44 | [điền từ -tukiem.csv] |
| pointcloud/s03f42.pcd / 4, Car | 30,62 | [điền từ -tukiem.csv] |

pointcloud/s03f42.pcd / Quy luật: "xa gấp … lần thì số điểm còn khoảng …" [điền sau khi có `points_in`]; so với slide (1968 → 718 → 214 → 72 → 19 điểm, tức khoảng 2,5 đến 3 lần mỗi khi xa thêm 10 m ở gần).
Phần tự kiểm: s03f42 idx 4 có z đáy −1,204 (lơ lửng 0,50 m) và kích thước nhỏ -> N Giữ nguyên, lý do có thể là xe bị che khuất (occluded = 1) nên ít điểm và hộp khó khớp.

không có người kiểm chéo
## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc
pointcloud/s03f42.pcd / (a) Model phát hiện vật thể 3D phải dự đoán vị trí, kích thước và hướng trong không gian thật để xe có thể tính khoảng cách và tránh va chạm, nên nhãn phải đo bằng mét, còn nhãn pixel phụ thuộc camera và không cho biết vật cách xe bao xa. Vật ở xa hoặc bị che chỉ có rất ít điểm (ví dụ chiếc xe cách khoảng 30 m chỉ có vài chục điểm), nên hộp dễ bị vẽ theo phần nhìn thấy chứ không phải cả vật; vì vậy cần quy tắc riêng như kiểm kích thước theo bảng 4.3, đánh dấu vật bị che và không để model học từ hộp quá nhỏ hoặc lơ lửng.

(b) Dùng AI:
Dùng Claude để hướng dẫn các quy trình làm bài lab

(c) Điều còn chưa chắc: Tôi chưa chắc hướng đầu/đuôi của một số xe, vì ảnh chỉ cho thấy mũi tên chạy dọc trục dài của hộp chứ không cho biết đầu xe nằm ở phía nào, và mỗi vật mới được xem ở một góc nhìn chứ chưa đủ Top/Side/Front. Với chiếc xe bị che khuất ở xa, tôi không biết kích thước thật của nó lớn hơn hộp hiện tại bao nhiêu, nên giữ cảnh báo thay vì đoán. Tôi cũng chưa có khoảng kích thước của người đi bộ và xe hai bánh nên chưa khẳng định các hộp đó đã đúng.
