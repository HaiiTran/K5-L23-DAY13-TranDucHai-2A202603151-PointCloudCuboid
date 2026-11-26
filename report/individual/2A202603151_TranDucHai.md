# Báo cáo Day 13 — Trần Đức Hải, 2A202603151

> **Cách dùng mẫu này:** đổi tên file thành `<MSSV>_<HoTen>.md` (họ tên không dấu, viết liền), ví dụ `2A202600123_NguyenVanAn.md`. Đổi tên, đừng chép thành file thứ hai.
>
> Xoá mọi dấu TODO và dòng gợi ý (dòng bắt đầu bằng `>`). Mỗi con số lấy từ **bảng A** hoặc **phiếu tự kiểm** trong `submission/`, kèm đơn vị (m, °). Ảnh để trong `report/individual/images/` (nộp LMS trong file zip, không commit); trong báo cáo chỉ ghi **tên file ảnh**. Báo cáo này nằm trên repo public: không chép biển số, chữ trên biển hiệu hay biển chỉ đường, tên đường, địa danh; nhắc tới bạn khác thì ghi MSSV, không ghi họ tên. Cả báo cáo ~2 trang.

## 1. Kết luận trước
- MSSV `2A202603151`; task `D13-2A202603151`; 10 frame: s01f09, s01f30, s02f21, s02f46, s03f17, s03f45, s04f02, s04f52, s05f00, s05f41.
- Môi trường: Windows, trình duyệt Brave, tài khoản CVAT `haniki`; phiên bản CVAT: **2.81.0**; MSSV bạn ngồi cạnh: **2A202603053**.
- Tôi đã tạo 108 cuboid. `s04f52` có ít nhất, 5 cuboid, vì tôi chưa đánh nhãn đủ vật thể trong frame đó. Sau lần `sync`, selfcheck còn cảnh báo ở 2 cuboid: `s04f02` #14 có đáy cao hơn mặt đường 0,52 m và #20 có đáy thấp hơn mặt đường 0,33 m. 
- Tự đánh giá: tôi tự tin nhất với class và vị trí của các xe gần có cụm điểm rõ; phần còn yếu nhất là căn đáy với các cuboid ở xa bị mờ đáy, vì hai hộp ở `s04f02` vẫn bị selfcheck cảnh báo.

- Cách dùng CVAT: xoay khung 3d bằng chuột trái, di chuyển bằng chuột phải và zoom bằng lăn chuột; nhìn từ trên xuống để xác định vị trí vật quanh xe. Khi vẽ chọn class rồi dùng **Shape**; chỉnh tâm, chiều dài, chiều rộng ở **Top**, rồi chỉnh chiều cao và đáy ở **Side/Front**. Bật **Cuboid orientation** để kiểm tra hướng mũi tên của object, sau đó dùng `python scripts/lab.py preview` để rà lại các hộp trên ảnh.

## 2. Đọc frame (Bước 2)
Hai frame đọc kỹ. Ảnh: `t1-<frame>-3d`, `t1-<frame>-top` (ảnh nhìn từ trên xuống, có đánh dấu phía trước, +x, +y).

| Frame | Phía trước xe ở cạnh nào của ảnh nhìn từ trên xuống; +x, +y chỉ về đâu | Mặt đường (phẳng/dốc; thấp hơn LiDAR ~ m) | Vật xa nhất đọc được (~ m) | Loại vật thấy | Vật phía sau / hai bên | Cụm chưa đọc được |
|---|---|---|---|---|---|---|
| s01f09 | Phía trước ở cạnh trên, +x bên trái, +y xuống dưới | Phẳng; mặt đường thấp hơn LiDAR 1 khoảng nhỏ | Khoảng 100 m | Xe con, xe tải/xe buýt, xe hai bánh và người đi bộ | Có xe ở phía sau và hai bên| Một vài cụm xe ở xa chưa rõ class, khó tách khỏi nền cảnh |
| s01f30 | Phía trước ở cạnh trên, +x bên trái, +y xuống dưới | Phẳng; mặt đường thấp hơn LiDAR 1 khoảng nhỏ | Khoảng 50 m | Xe con, người, xe hai bánh | Có xe ở hai bên và phía sau | Cụm xe dày đặc để trên vỉa hè sát bên đường |

Camera cho biết màu xe và chi tiết nhận dạng giúp phân biệt loại xe. LiDAR cho biết khoảng cách và vị trí 3D, kích thước/hình dạng không gian của vật, mật độ và phân bố điểm để thấy vùng bị che hoặc vật chỉ còn ít điểm.

Tám frame lướt:

| Frame | Phía trước ở cạnh nào của ảnh nhìn từ trên xuống | Điều lạ (nếu có) |
|---|---|---|
| s02f21 | Phía trước ở cạnh trên, +x bên trái, +y xuống dưới | Không thấy điểm lạ rõ khi lướt |
| s02f46 | Phía trước ở cạnh trên, +x bên trái, +y xuống dưới | Không thấy điểm lạ rõ khi lướt |
| s03f17 | Phía trước ở cạnh trên, +x bên trái, +y xuống dưới | Không thấy điểm lạ rõ khi lướt |
| s03f45 | Phía trước ở cạnh trên, +x bên trái, +y xuống dưới | Không thấy điểm lạ rõ khi lướt |
| s04f02 | Phía trước ở cạnh trên, +x bên trái, +y xuống dưới | Không thấy điểm lạ rõ khi lướt |
| s04f52 | Phía trước ở cạnh trên, +x bên trái, +y xuống dưới | Có một điểm đơn lẻ thấp bất thường ở phía sau, lệch phải; cần xem nghiêng trong CVAT |
| s05f00 | Phía trước ở cạnh trên, +x bên trái, +y xuống dưới | Không thấy điểm lạ rõ khi lướt |
| s05f41 | Phía trước ở cạnh trên, +x bên trái, +y xuống dưới | Không thấy điểm lạ rõ khi lướt |

## 3. Ba cuboid có hướng dẫn (Bước 3)
Ảnh: `t2-a-car`, `t2-b-bus-truck`, `t2-c-ped-cyclist`.

| Vật | Frame, idx | Class | Mũi tên yaw đúng hướng? | Đáy chạm đường? | Đã kiểm Top/Side/Front + ảnh? | Kích thước trong bảng 4.3? |
|---|---|---|---|---|---|---|
| (a) xe con gần | s01f09, #11 | Car | Có | Có | Có | dài 4.27 m - cao 1.46 m - rộng 1.59 m |
| (b) xe buýt hoặc xe tải | s01f09, #6 | Truck | Có | Có | Có | dài 5.91 m - rộng 2.16 m - cao 1.79 m |
| (c) người đi bộ hoặc xe hai bánh | s01f09, #10 | Cyclist | Có | Có | Có | dài 1.29 m - rộng 0.61 m - cao 1.89 m |

Vòng hiệu chuẩn: cuboid của tôi ao sát vào xe hơn của bạn.

## 4. Bốn hiện tượng chất lượng (Bước 4)
Nguyên nhân ghi là **giả thuyết**. Frame mượn thì ghi rõ nguồn: task chung, hoặc MSSV của bạn cho mượn.

| Hiện tượng | Frame / tên ảnh / nguồn | Nguyên nhân (giả thuyết) | Thấy gì trên point cloud | Hệ quả khi gán (theo guideline) |
|---|---|---|---|---|
| 1. Bóng che | `s04f02` / `t4-1-bong-che.png` / task | minivan gần che tia LiDAR tới vùng phía sau | Cụm điểm của xe gần rõ, vùng ngay phía sau xe thưa hoặc khuất; camera thấy xe áp sát bên phải | Chỉ gán cuboid cho vật nhận diện được; bật occluded nếu vật bị che một phần và suy luận hình dạng |
| 2. Thưa ở xa | `s02f46` / `t4-2-thua-o-xa.png` / task | khoảng cách xa làm điểm phản hồi thưa hơn | Xe con #6 ở khoảng 38,82 m có 68 điểm trong hộp; đường bao ít điểm | Chỉ gán nếu còn đủ bằng chứng (khoảng 15–20% thể tích); nếu không đủ thì bỏ qua và ghi LOW_VISIBILITY, không đoán kích thước |
| 3. Phản xạ thấp / ít điểm bất thường | `s04f02` / `t4-3-phan-xa-thap.png` / task | bề mặt tối hoặc kính của minivan trả về ít năng lượng hơn bề mặt sáng | Phần thân có điểm yếu hoặc thưa hơn; biển báo sáng trong cùng cảnh làm mốc so sánh | Giữ hộp theo phần vật thể có bằng chứng; không nới hộp theo quầng phản xạ, ghi class/occluded theo vật thật |
| 4. Điểm lạ | `s05f41` / `t4-4-diem-la.png` / task | nhiễu cảm biến hoặc lỗi đo khoảng cách; nguyên nhân chưa xác định | Cụm điểm thấp hơn mặt đường dự kiến | Không gán cuboid cho cụm điểm vì không tạo thành vật thể nhận diện được; chụp ở góc nghiêng để thấy nó nằm dưới mặt đường |

## 5. Số từ cuboid của tôi (Bước 5)
Lấy từ `submission/D13-2A202603151-bangA.csv`; số điểm lấy từ cột `points_in` của `-tukiem.csv`. Mặt đường khoảng −1,7 m: z đáy lệch hơn 0,3 m là lơ lửng hoặc chìm.

| Cuboid (frame, idx, class) | z | h | z đáy = z − h/2 | dist_m | r = √(x²+y²+z²) | yaw_deg | Nhận xét (lơ lửng / chìm / chạm đường) |
|---|---|---|---|---|---|---|---|
| `s03f17`, idx 1, Car | −0.880 m | 1.580 m | −1.670 m | 3,16 m | 3,28 m | 270,6° | Chạm đường; ground gap 0,09 m |
| `s05f41`, idx 10, Car | −0,102 m | 1,595 m | −0,900 m | 65,60 m | 65,60 m | 13,3° | Ground gap −0,04 m theo tự kiểm; ngoài vùng 50 m được chấm |
| `s02f21`, idx 2, Cyclist | −0,780 m | 1,510 m | −1,535 m | 6,07 m | 6,12 m | 90,0° | Chạm đường; ground gap 0,22 m |

Gần nhất là `s03f17` idx 1 (`Car`): `dist_m=3,16 m`, `r=3,28 m`. Xa nhất là `s05f41` idx 10 (`Car`): `dist_m=65,60 m`, `r=65,60 m`; hộp này nằm ngoài vùng 50 m được chấm. Xe hai bánh `s02f21` idx 2 có kích thước `1,79 × 0,70 × 1,51 m`: gần khoảng xe máy có người (dài thấp hơn mốc 1,80 m khoảng 0,01 m; rộng và cao trong khoảng). Xe con `s03f17` idx 1 là `3,86 × 1,76 × 1,58 m`, nằm trong khoảng sedan ở bảng 4.3. Xe cùng chiều phía trước `s01f09` idx 11 có yaw `268,9°`; xe ngược chiều `s01f09` idx 2 có yaw `91,1°`, gần các hướng tham chiếu 270° và 90°.

Mật độ theo khoảng cách (4 cuboid ở các `dist_m` khác nhau):

| Cuboid (frame, idx, class) | dist_m | Số điểm (`points_in`) |
|---|---|---|
| `s03f17`, idx 1, Car | 3,16 m | 4492 |
| `s04f02`, idx 2, Car | 15,10 m | 716 |
| `s04f02`, idx 13, Car | 24,96 m | 449 |
| `s05f41`, idx 11, Car | 35,15 m | 111 |

Trong mẫu này, xe ở 15.10 m xa gấp khoảng 4.8 lần xe ở 3.16 m nhưng số điểm còn khoảng 16% (716 so với 4492); nhìn chung điểm giảm theo khoảng cách. So với slide, mẫu 10–20 m gần 718 điểm; mẫu 20–30 m (449) và 30–40 m (111) cao hơn mức tham chiếu 214 và 72.

Sau khi xem `preview`/`selfcheck`, `s02f46` idx 6 được chỉnh width từ 2,44 m xuống 2,278 m và không còn cờ ngoài khoảng. `s04f02` idx 20 được chỉnh đáy; ground gap đổi từ −0,25 m thành −0,33 m nhưng vẫn bị cờ chìm, nên cần kiểm tra lại ở Side/Front. Hai cảnh báo hiện còn là `s04f02` idx 14 (đáy cao hơn mặt đường 0,52 m) và idx 20 (thấp hơn 0,33 m); giữ tạm vì cờ selfcheck là gợi ý, chưa đủ để tự kết luận hộp sai, và cần đối chiếu lại với point cloud/mặt đường trong CVAT.

Đổi kiểm: MSSV người đã kiểm chéo với bạn, hai con số nào được đối chiếu với bảng A, khớp hay lệch (lệch thì đã sửa gì).

## 6. Ý nghĩa của nhãn, cách dùng AI và điều còn chưa chắc
(a) Với model, nhãn 3D dùng mét để vị trí và kích thước vật giữ ý nghĩa trong không gian, không đổi theo độ phân giải ảnh hay khoảng cách đến camera như pixel. Vật thưa điểm cần quy tắc riêng vì vài điểm không xác định chắc toàn bộ kích thước; chỉ gán khi còn đủ bằng chứng, nếu không thì bỏ qua hoặc ghi LOW_VISIBILITY để tránh dạy model bằng hộp đoán.

(b) Công cụ: ChatGPT/Codex.
- Câu hỏi chính: tìm ví dụ điểm lạ trong point cloud và hỗ trợ báo cáo từ Bảng A/phiếu tự kiểm.
- Cách dùng kết quả: dùng làm gợi ý và bản nháp; cần đối chiếu cuboid, điểm lạ và ảnh minh chứng trực tiếp trong CVAT trước khi chốt, không xem gợi ý AI là nhãn chuẩn.

(c) Điều còn chưa chắc: nguyên nhân của cụm điểm nằm dưới mặt đường ở `s05f41` chưa xác định. Hai cảnh báo đáy ở `s04f02` vẫn cần đối chiếu lại với mặt đường và point cloud. Lỗi phát hiện sau hạn: 