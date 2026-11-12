# Báo cáo cá nhân — DoThiHuyen-2A202604048

Dữ liệu: 12 ảnh người COCO val2017 (Lin et al., 2014) và 8 ảnh chân dung Wikimedia Commons, chỉ dùng trong lớp.

## 0. Thông tin học viên

- Họ tên: Đỗ Thị Huyền
- MSSV: 2A202604048
- Email: hdo110194@gmail.com
- Link repository: https://github.com/NhatHaDo/K5-L2-DAY04-DoThiHuyen-2A202604048-KeypointPose
- Phiên bản CVAT (Help → About): 2.74.1
- Máy: hệ điều hành macOS · RAM 16 GB (Docker được cấp 7.7 GB) · chạy CVAT chế độ đầy đủ
- Bộ ảnh (`src/exports/assignment.json` do `lab.py setup` tạo): có
- Giờ bắt đầu: 16:30
- Giờ kết thúc: 18:50

## 1. Kiểm tra label (CP0)

| Label | Kiểu | Số điểm | Tên điểm đúng bảng của lab? | Đúng thứ tự? | Số cạnh |
|---|---|---:|---|---|---:|
| `person` | Skeleton | 17 | Có | Có | 19 |
| `face` | Skeleton | 13 | Có | Có | 13 |

Mẫu `person` dựng theo người quay mặt về camera (các điểm `left_*` nằm bên phải khung mẫu)? Có

Cách đổi cờ `occluded` / `outside` cho **một điểm** trên phiên bản CVAT này (một câu):

Rê chuột vào đúng một điểm của skeleton rồi bấm `Q` để bật/tắt `occluded` (v = 1) hoặc `O` để bật/tắt `outside` (v = 0); không dùng `H` (hidden).

## 2. Số lượng đã gán và thời gian (CP1, CP2)

| Nhóm ảnh | Số ảnh | Số skeleton đã gán | Số người dưới 60 px (không gán) | Số poster / gương / màn hình bỏ qua | Thời gian (phút) |
|---|---:|---:|---:|---:|---:|
| `easy` | 5 | 5 | 0 | 0 | 25 |
| `occluded` | 3 | 5 | 0 | 0 | 20 |
| `back` | 2 | 3 | 0 | 0 | 15 |
| `small` | 1 | 3 | 7 | 0 | 10 |
| `crowd` | 1 | 3 | 0 | 0 | 10 |
| `faces` | 8 | 8 | — | 0 | 15 |

- Số ảnh người đã xong ở mốc phút 105: 10 / 10
- Thời gian thực tế (phút, ước lượng): CP1 ~80 · CP2 ~15 · CP3 ~45
- Ảnh tốn thời gian nhất là ảnh nào, vì sao?

`000000572411.jpg` (nhóm `small`): ba người ngồi trong toa tàu, ảnh nhỏ và mờ, phần thân dưới bị vách toa che, một người khoanh tay nên phải phóng to để phân biệt cổ tay trái/phải và quyết định `v = 1` cho phần chân bị che.

## 3. Bảng ca khó (CP1, CP2)

| Ảnh | Người / mặt | Điểm | Cờ đã chọn | Phân vân giữa | Lý do chọn |
|---|---|---|---:|---|---|
| `000000022857.jpg` | #1 đầu bếp (đứng nghiêng, mặt hướng sang phải ảnh) | `left_ear`, `left_eye` | 1 | `v = 1` / `v = 0` | Bị đầu che nhưng vẫn ước lượng được vị trí đối xứng với bên phải, nằm trong ảnh |
| `000000022857.jpg` | #1 đầu bếp | `left_ankle`, `right_ankle` | 0 | `v = 1` / `v = 0` | Chân bị khung ảnh cắt ở mép dưới, cổ chân nằm ngoài ảnh nên là `outside` |
| `000000022857.jpg` | #1 đầu bếp | `left_shoulder` → `left_knee` | 1 | `v = 1` / `v = 2` | Nửa người bên trái bị thân che khi đứng nghiêng, chỉ thấy được bên phải |
| `000000515309.jpg` | #1 người lái mô tô | `nose`, `left_eye`, `right_eye`, `left_ear`, `right_ear` | 1 | `v = 1` / `v = 2`; hướng người | Người nghiêng theo xe, camera thấy lưng; mặt nằm sau kính mũ bảo hiểm, không nhìn rõ nhưng ước lượng được vị trí nên chọn `occluded` |
| `000000572411.jpg` | người khoanh tay | `left_wrist`, `right_wrist` | 2 | `lr_swap` hay tay bắt chéo thật | Người khoanh tay nên hai cổ tay nằm ngược phía so với hai vai |

## 4. Bảng tự review (CP3)

Đi từ ảnh cuối về ảnh đầu. Mã lỗi: `lr_swap`, `vis_flag`, `position`, `missing`, `extra`, `ok`.

| Ảnh | Người / mặt | Điểm | Mã lỗi | Ghi chú | Đã sửa? |
|---|---|---|---|---|---|
| `000000572411.jpg` | #2 người khoanh tay | `left_wrist`, `right_wrist` | `ok` | Checker cảnh báo cặp wrist ngược phía; tay bắt chéo thật | Không cần |
| `000000515309.jpg` | #1 người lái mô tô | mũi, mắt / toàn người | `lr_swap` | Checker báo thấy rõ mũi + mắt (v = 2) nhưng `left_*` ở bên trái ảnh như người quay lưng | Có |
| `000000375535.jpg` | #2 | `hip`, `knee`, `ankle` | `lr_swap` | Cặp hông–gối–cổ chân ngược phía so với nửa trên | Có |
| `000000326480.jpg` | #3 | `left_ear`, `right_ear` | `lr_swap` | Cặp tai ngược phía | Có |
| `000000326480.jpg` | 3 skeleton | — | `extra` | Có 3 skeleton label `face` vẽ nhầm trong task `person` | Có (xóa) |
| `000000022857.jpg` | #1 đầu bếp | `left_ankle`, `right_ankle` | `vis_flag` | Điểm ngoài ảnh để `v = 2`, phải là `outside` | Có |
| `000000022857.jpg` | #1 đầu bếp | nửa người bên phải (mũi, mắt, vai, tay, hông, gối) | `vis_flag` | Bấm `Q` khi rê vào dây skeleton nên cả skeleton bị `occluded`; bên phải nhìn rõ phải là `v = 2` | Có |
| `000000022857.jpg` | — | — | `extra` | 1 skeleton `face` trong task `person` | Có (xóa) |
| `000000022857.jpg` | #2 người áo trắng, tạp dề sọc bên trái ảnh | cả skeleton | `missing` | Người thật > 60 px bị bỏ sót, chưa gán | Có (gán thêm) |
| `face_219250.jpg` | mặt nghiêng, kính râm tối | 8 điểm mắt | `ok` | Kính râm che hết mắt, cả 8 điểm mắt đã là `occluded` (`v = 1`) đúng quy tắc | Không cần |
| `face_143959.jpg` | mặt thẳng, kính trong | `lip_lower` | `position` | Điểm môi dưới đặt hơi thấp, xuống dưới mép môi dưới; kéo lên đúng mép môi. 8 điểm mắt qua kính trong vẫn `v = 2` (đúng) | Có |
| `face_125051.jpg` | mặt thẳng | cả 13 điểm | `ok` | Đủ 13 điểm, đúng vị trí, tất cả `v = 2` | Không cần |

Tổng hợp số lỗi theo mã (đếm theo dòng ở bảng trên, cập nhật sau khi điền 3 dòng mặt):

| `lr_swap` | `vis_flag` | `position` | `missing` | `extra` | `ok` |
|---:|---:|---:|---:|---:|---:|
| 3 | 2 | 1 | 1 | 2 | 3 |

Ngoài bảng: tổng cộng 13 skeleton `face` bị vẽ nhầm trong task `person` (9 ảnh), đã xóa hết.

Điểm còn phân vân giữa `v = 1` và `v = 0` (ghi cả hai lựa chọn và lý do chọn):

`left_ear` của đầu bếp trong `000000022857.jpg`: nếu chọn `v = 0` thì bỏ thông tin vị trí tai dù vẫn ước lượng được; chọn `v = 1` vì điểm nằm trong ảnh, chỉ bị đầu che, vị trí đoán được nhờ đối xứng với `right_ear`.

## 5. Kiểm file xuất và kết quả tự kiểm (CP4)

Tự kiểm tay trong file JSON:

| File | Người / mặt kiểm (ảnh, thứ tự) | Số phần tử `keypoints` | Điểm `outside` ra `v = 0`, `occluded` ra `v = 1`? | `num_keypoints` khớp? |
|---|---|---:|---|---|
| `DoThiHuyen_persons.zip` | `000000022857.jpg`, #1 | 51 | Có: `left_ankle`, `right_ankle` ra `v = 0`; các điểm bị che ra `v = 1` | Có: `num_keypoints` = 15 = 17 − 2 điểm `v = 0` |
| `DoThiHuyen_persons.zip` | `000000515309.jpg`, #1 | 51 | Có: 7 điểm bị che (mặt, tai, khuỷu/cổ tay trái, hông phải) ra `v = 1`; không có điểm `outside` | Có: `num_keypoints` = 17 = số điểm `v > 0` |
| `DoThiHuyen_faces.zip` | `face_125051.jpg`, #1 | 39 | Mặt thẳng, cả 13 điểm `v = 2`; không có điểm `occluded`/`outside` | Có: `num_keypoints` = 13 |

Sau khi xuất, CVAT ghi `occluded` thành `v = 1` và `outside` thành `v = 0`. Tọa độ của điểm `outside` trong file là `0, 0` hay tọa độ cũ? Tọa độ cũ (CVAT 2.x xuất như vậy; chuẩn COCO ghi `0, 0`).

Hai job đã chuyển sang completed trên CVAT? Có

Dán phần **TÓM TẮT** in ra từ `src/check_submission.py`:

```text
- File person: DoThiHuyen_persons.zip
  Số ảnh: 12 · số skeleton `person`: 19 · mỗi annotation 51 số
  Skeleton theo ảnh: 000000022857=2, 000000043968=1, 000000103970=1, 000000184224=1, 000000193879=1, 000000194027=1, 000000302102=2, 000000326480=3, 000000375535=2, 000000493741=1, 000000515309=1, 000000572411=3
  Cờ v: v=2 245 (76%) · v=1 74 (23%) · v=0 4 (1%)
  num_keypoints trung bình: 16.8
- File face: DoThiHuyen_faces.zip
  Số ảnh: 8 · số skeleton `face`: 8 · mỗi annotation 39 số
  Skeleton theo ảnh: face_125051=1, face_143959=1, face_145621=1, face_147624=1, face_159828=1, face_219250=1, face_223746=1, face_469647=1
  Cờ v: v=2 95 (91%) · v=1 9 (9%) · v=0 0 (0%)
  num_keypoints trung bình: 13.0
- Ghi chú: 4 điểm v = 0 vẫn mang tọa độ cũ (CVAT 2.x xuất như vậy; chuẩn COCO ghi 0, 0). Không phải lỗi: điểm v = 0 bị bỏ qua khi huấn luyện và khi chấm.
- Số [LỖI]: 0 · số [CẢNH BÁO]: 5
Kết quả: ĐẠT (đọc lại từng cảnh báo ở trên)
```

Các `[CẢNH BÁO]` còn giữ nguyên và lý do:

- `000000022857.jpg` #2 — `left_shoulder` sát mép ảnh (1, 126): khớp vai trái của người này nằm ngay mép trái ảnh, vẫn nhìn thấy một phần vai nên giữ `v = 2`.
- `000000022857.jpg` #1 — cặp wrist ngược phía: đầu bếp đứng nghiêng, hai tay cùng vươn về phía bếp; tay phải (gần camera) duỗi xa hơn tay trái nên cổ tay phải nằm xa hơn về bên phải ảnh dù vai phải ở bên trái. Hiệu ứng phối cảnh, không phải `lr_swap`; giữ nguyên.
- `000000043968.jpg` #1 — cặp hip, knee, ankle ngược phía: đã kiểm lại từng điểm, tư thế thật (người vặn mình / chân bắt chéo), nhãn đúng nên giữ nguyên.
- `000000193879.jpg` #1 — cặp knee, ankle ngược phía: đã kiểm lại, chân bắt chéo thật, nhãn đúng nên giữ nguyên.
- `000000572411.jpg` #2 — cặp wrist ngược phía: người khoanh tay, hai cổ tay bắt chéo thật nên giữ nguyên.

## 6. Ảnh chụp màn hình

1. Label `person` trong label constructor (đủ 17 điểm có tên):

   ![Label person](images/1_label_person.png)

2. Một người quay lưng đã gán, cờ đã đặt:

   ![Người quay lưng](images/2_nguoi_quay_lung.png)

3. Một khuôn mặt đã gán 13 điểm, phóng to vùng mắt:

   ![Khuôn mặt phóng to](images/3_mat_phong_to.png)

4. Một đoạn JSON xuất của một người:

   ![JSON export](images/4_json_export.png)

Nguồn ảnh: COCO val2017 / Wikimedia Commons.

## 7. Tự đánh giá

1. Lỗi bạn mắc nhiều nhất hôm nay, và cách bạn sẽ tránh ở dự án thật:

   Lỗi nhiều nhất là `extra`: vẽ 13 skeleton label `face` trong task `person`. Thứ hai là `lr_swap` ở người đứng nghiêng/quay lưng. Ở dự án thật: kiểm label trong popup trước khi bấm Shape, sau mỗi ảnh lọc panel Objects theo label, và rê chuột kiểm tên một điểm `left_*` trước khi kéo cả skeleton.

2. Một quy tắc bạn đề xuất thêm vào guideline (kèm ảnh ví dụ nếu có):

   Với người đứng nghiêng hoặc quay lưng, xác định hướng người trước rồi mới kéo điểm: rê chuột kiểm tên `left_shoulder` và `left_hip`, hai điểm này phải cùng một phía cơ thể. Đường vai–hông chéo nhau chỉ chấp nhận khi người vặn mình thật (ví dụ `000000022857.jpg`).

3. Nhãn của bạn đi vào model ở đâu? Chọn một lỗi bạn đã sửa và nói nó sẽ làm model học sai thế nào (heatmap, trọng số trong loss, hay bước lật ảnh):

   Lỗi `vis_flag` ở hai cổ chân của đầu bếp (`000000022857.jpg`): điểm ngoài ảnh nhưng để `v = 2` nên model sẽ được dạy tạo heatmap đỉnh tại một vị trí không có cổ chân (mép ảnh), và điểm đó vẫn được tính trọng số trong loss. Model học cách "bịa" khớp ở biên ảnh. Sau khi sửa thành `v = 0`, điểm bị bỏ qua khi tính loss. Với lỗi `lr_swap`, bước lật ảnh khi augment sẽ đổi tên trái/phải theo cặp, nên nhãn sai càng nhân đôi: model nhận tín hiệu mâu thuẫn giữa ảnh gốc và ảnh lật.

## 8. Khó khăn và lỗi kỹ thuật

- Máy macOS: hướng dẫn viết cho Windows/PowerShell; dùng `python3`, `ls` và đường dẫn `/` thay cho `python`, `dir`, `\`.
- Lần đầu chạy `setup` nhầm trong thư mục repo Ngày 2 nên báo không có `lab.py`; chạy lại trong repo Ngày 4.
- Quên tài khoản CVAT Ngày 2: liệt kê user bằng `manage.py shell` (CVAT 2.74 dùng `iam.User`, phải gọi `get_user_model()`), đặt lại mật khẩu bằng `manage.py changepassword`.
- Đăng nhập báo "Unverified email": đánh dấu email đã xác minh qua `allauth.account.models.EmailAddress`.
- Mật khẩu chỉ gồm dấu cách bị CVAT coi là rỗng ("This field may not be blank"); đặt lại mật khẩu có chữ và số.
- Lần export đầu CHƯA ĐẠT (15 lỗi: 13 skeleton `face` trong task `person`, 2 cổ chân ngoài ảnh để `v = 2`); sửa trên CVAT, export lại ĐẠT.

## 9. Tài liệu tham khảo và công cụ AI đã dùng

- Tài liệu lab: `docs/GUIDELINE.md`, `docs/CVAT_CHEATSHEET.md`, `CHECKPOINTS.md`, `SUBMISSION.md`.
- Lin et al., 2014. Microsoft COCO: Common Objects in Context.
- Claude (Anthropic): hỏi cách chạy lab trên macOS
