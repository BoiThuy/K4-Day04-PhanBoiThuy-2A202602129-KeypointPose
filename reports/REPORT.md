# Báo cáo Ngày 4 - Keypoint & Pose

Họ tên: **Phan Bội Thúy** Nhóm: **Cá nhân (solo)** Ngày: **16/09/2026**

## 1. Nhãn của bạn

| Chỉ số                       |                                 Giá trị |
| ---------------------------- | --------------------------------------: |
| Số ảnh đã gán                |                                  **20** |
| Số skeleton                  |                                  **29** |
| v=2 / v=1 / v=0              |                       **389 / 26 / 78** |
| Thời gian trung bình mỗi ảnh | **Không ghi nhận trong dữ liệu đã lưu** |

Ba khớp có `%v=1` cao nhất:

1. `left_ear`: **5/29 = 17.24%**
2. `right_wrist`: **4/29 = 13.79%**
3. `right_ear`: **3/29 = 10.34%**  
   (`left_hip` cũng có **3/29 = 10.34%**, đồng hạng.)

Các khớp trên đúng là những vị trí bạn thấy cần chú ý khi gán nhãn, đặc biệt ở các ảnh có che khuất hoặc tư thế khó. Tuy nhiên, `%v=1` cao không có nghĩa là khớp đó luôn khó xác định về mặt giải phẫu; nó chủ yếu cho thấy khớp còn nằm trong ảnh nhưng bị che một phần. Khi kiểm tra lại, bạn cũng nhận thấy lỗi khó hơn không chỉ là visibility mà còn là xác định đúng bên trái/phải, điển hình ở `train_06`.

## 2. Chấm với gold

bạn không còn file kết quả/snapshot trước rework, vì vậy bạn không tự điền số liệu cho lần chạy đó. Với trạng thái nhãn hiện tại, bạn đối chiếu trực tiếp nhãn của bạn với file gold đã được cung cấp.

| Chỉ số                |           Trước rework |                     Sau rework |
| --------------------- | ---------------------: | -----------------------------: |
| OKS trung bình        | **Không còn snapshot** |                     **0.8488** |
| OKS@0.50              | **Không còn snapshot** |                     **0.9655** |
| OKS@0.75              | **Không còn snapshot** |                     **0.8966** |
| Lỗi `dao_trai_phai`   | **Không còn snapshot** |                    **1 ca rõ** |
| Lỗi `nham_nguoi`      | **Không còn snapshot** | **0 ca rõ khi ghép theo bbox** |
| Lỗi `xoa_khop_bi_che` | **Không còn snapshot** |                     **5 khớp** |

**bạn đã sửa gì giữa hai lần chạy:**

Do bạn không còn snapshot trước rework nên bạn không thể khẳng định chính xác toàn bộ lịch sử sửa. Từ kết quả đối chiếu hiện tại, các lỗi bạn xác định cần sửa rõ nhất là:

- `train_06`, người thứ 1, các cặp keypoint trái/phải: bạn kiểm tra lại và hoán đổi đúng bên trái/phải. OKS của skeleton này tăng từ khoảng **0.009** lên khoảng **0.771** sau khi hoán đổi các cặp trái/phải.
- `train_10`, người thứ 1, `left_hip` và `right_hip`: nhãn của bạn đang để `v=0` trong khi gold là `v=1`; bạn cần giữ tọa độ và đánh dấu khớp bị che thay vì xóa.
- `train_15`, người thứ 2, `right_wrist`, `left_knee`, `left_ankle`: nhãn của bạn đang để `v=0` trong khi gold là `v=1`; bạn cần giữ các khớp này và dùng trạng thái bị che.

**Lỗi đảo trái/phải của bạn xảy ra ở ảnh nào?**

Lỗi rõ nhất xảy ra ở **`train_06`, người thứ 1**. Đây là ảnh khó vì sai lệch không nằm ở một keypoint riêng lẻ mà xuất hiện có hệ thống trên nhiều cặp trái/phải. Khi bạn hoán đổi các cặp trái/phải, OKS tăng rất mạnh, vì vậy bạn xác định nguyên nhân chính là gán nhầm bên trái/phải chứ không chỉ là lệch tọa độ nhỏ.

## 3. Kiểm chéo

Bạn cùng nhóm: **Không có - bạn làm bài cá nhân (solo).**

Vì bạn làm bài solo nên bạn không có bảng visibility của bạn cùng nhóm để tính chênh `%v=1`. bạn giữ nguyên mục này để đúng cấu trúc báo cáo, nhưng không tự tạo số liệu kiểm chéo.

| Khớp          | bạn |  Họ | Lệch | Nguyên nhân (guideline hay gán sai?) |
| ------------- | --: | --: | ---: | ------------------------------------ |
| Không áp dụng |   — |   — |    — | bạn làm bài solo                     |
| Không áp dụng |   — |   — |    — | Không có dữ liệu của người thứ hai   |

Luật bạn bổ sung vào `GUIDELINE_MINI.md` sau khi tự kiểm tra lại:

- **Nếu keypoint bị che nhưng từ phần cơ thể liền kề vẫn xác định được rằng vị trí giải phẫu của khớp còn nằm trong khung ảnh, bạn giữ tọa độ và chọn `v=1`. bạn chỉ chọn `v=0` khi khớp thực sự ra ngoài khung hoặc không còn vị trí hợp lệ để đặt điểm.**

## 4. Model

| Chỉ số         | yolo26n-pose gốc | Sau fine-tune |       Chênh |
| -------------- | ---------------: | ------------: | ----------: |
| pose_mAP50     |       **0.8450** |    **0.8450** | **+0.0000** |
| pose_mAP50-95  |       **0.6853** |    **0.6908** | **+0.0055** |
| pose_precision |       **0.9734** |    **0.9792** | **+0.0058** |
| pose_recall    |       **0.8462** |    **0.8462** | **+0.0000** |
| box_mAP50-95   |       **0.8119** |    **0.8041** | **-0.0078** |

### Trả lời năm câu hỏi ở cuối notebook

**1. `pose_mAP50-95` thay đổi bao nhiêu? Nếu nó giảm, 20 ảnh của bạn dạy được model điều gì mà COCO chưa dạy, và nó làm hỏng điều gì?**

`pose_mAP50-95` tăng từ **0.6853** lên **0.6908**, tức tăng **0.0055**. Trong lần chạy này chỉ số không giảm nên trường hợp giả định “nếu nó giảm” không xảy ra. Sau fine-tune, `pose_precision` cũng tăng từ **0.9734** lên **0.9792**, trong khi `box_mAP50-95` giảm từ **0.8119** xuống **0.8041**. bạn kết luận tập 20 ảnh của bạn có tạo ra một thay đổi nhỏ theo hướng cải thiện pose, nhưng chưa đủ lớn và chưa cải thiện đồng đều mọi chỉ số.

**2. `box_mAP` và `pose_mAP` chênh nhau bao nhiêu? Model tìm người dễ hơn hay tìm khớp dễ hơn? Vì sao?**

Sau fine-tune, `box_mAP50-95 = 0.8041` và `pose_mAP50-95 = 0.6908`, chênh nhau **0.1133**. Như vậy model tìm **người** dễ hơn tìm chính xác các **khớp**. Bounding box chỉ cần bao đúng vùng có người, còn pose phải định vị đúng nhiều keypoint riêng lẻ và còn chịu ảnh hưởng của che khuất, tư thế khó và khả năng nhầm trái/phải.

**3. Một ảnh test model đoán sai - gọi tên lỗi theo bốn loại của slide 43.**

bạn chọn **`test_07`**. Ở ảnh này, các keypoint phần chân bị kéo xuống khu vực mặt bàn thay vì bám đúng vào chân của người. bạn xếp lỗi này vào loại **trượt hẳn**, vì vị trí dự đoán không chỉ lệch nhẹ vài pixel mà đã nằm sai vùng cơ thể.

**4. Ảnh nào có OKS thấp nhất giữa nhãn của bạn và model? Ai đúng, và bạn dựa vào đâu?**

Ảnh có OKS thấp nhất giữa model và nhãn của bạn là **`train_06`**, với **OKS = 0.059** trong notebook. Khi bạn đối chiếu nhãn của mình với gold, đây cũng là ca có sai lệch rất lớn và có dấu hiệu đảo trái/phải rõ ràng. Sau khi hoán đổi các cặp trái/phải, OKS nhãn của bạn so với gold tăng mạnh, vì vậy bạn xác định **nhãn của bạn là phía có lỗi rõ ở `train_06`**, đặc biệt ở việc xác định bên trái/phải. bạn không xem model là đáp án tuyệt đối; căn cứ chính của bạn là kết quả gold.

**5. Ảnh bạn gán tệ nhất có cũng là ảnh model đoán tệ nhất không? Nếu có, điều đó nói gì về bức ảnh đó?**

Có. Khi bạn đối chiếu trạng thái nhãn hiện tại với gold, **`train_06`** là skeleton có OKS thấp nhất, khoảng **0.009** trước khi sửa lỗi trái/phải. Trong notebook, `train_06` cũng là ảnh có OKS model-vs-nhãn thấp nhất, chỉ **0.059**. Điều này cho thấy đây là một ca khó và đồng thời nhãn của bạn có lỗi hệ thống; vì vậy khi model và nhãn bất đồng mạnh, bạn phải kiểm tra lại gold và guideline trước khi kết luận model sai.

## 5. Một rule evidence bạn đã dùng

Ở **`train_01`, người thứ 1, `left_wrist`**, bạn đặt keypoint ở khoảng **(327.60, 326.31)** với trạng thái **`v=1`**. Gold cũng giữ keypoint này ở vị trí gần **(324, 321)** với `v=1`. bạn dựa vào hướng của cẳng tay và phần cơ thể liền kề để xác định rằng cổ tay vẫn nằm trong khung ảnh dù bị che một phần. Vì vậy bạn giữ tọa độ và chọn `v=1`, thay vì xóa keypoint thành `v=0`.
