# Mini guideline - bài cá nhân Nguyễn Minh Quân

Ngày tổng hợp: 16/09/2026. Đây là bản tổng hợp từ quy tắc lab và rà soát annotation hiện có; không phải nhật ký được xác nhận đã ghi trong lúc gán. Số thứ tự người là số dòng trong file YOLO, bắt đầu từ 1, không phải thứ tự trái sang phải hoặc số người của gold.

## 1. Luật bắt buộc

- Giữ đủ 17 điểm COCO, đúng tên và thứ tự; mỗi điểm có x, y, v.
- Trái/phải theo cơ thể người, không theo phía ảnh.
- Khớp nhìn rõ: v=2. Bị che nhưng còn trong khung: v=1 và giữ tọa độ ước lượng.
- Khớp ra ngoài ảnh: v=0; bản YOLO chuẩn hóa tọa độ thành (0, 0).
- Không xóa điểm và không dùng Hidden (`h`); dùng Occluded (`q`) hoặc Outside (`o`) trong CVAT.
- Gán xong một người rồi chuyển sang người khác; kiểm chuỗi vai–khuỷu–cổ tay và hông–gối–cổ chân.

## 2. Luật áp dụng cho bài cá nhân

| Tình huống | Quy tắc | Căn cứ / ảnh minh họa |
| --- | --- | --- |
| Hông dưới quần áo | Ước lượng tâm khớp nối đùi–chậu từ trục thân và đùi, không đặt tùy tiện lên thắt lưng. Khi tư thế và đường nét đủ rõ có thể giữ v=2 theo cách gán hiện tại; khi bị vật hoặc người khác che, dùng v=1 nếu còn trong ảnh. | train_01 người #1 hiện có hai hông v=2 nhưng gold báo lệch nhẹ; cần thống nhất vị trí giải phẫu, không tự đổi toàn bộ cờ vì có quần áo. [Ảnh](reports/figures/train_01_annotation.jpg). |
| Tai bị tóc hoặc mũ che | Nếu vị trí tai không nhìn trực tiếp được, đặt v=1 và ước lượng theo đầu; v=2 chỉ khi xác định được tai. Không dùng v=0 chỉ vì có mũ. | train_04 cả hai người có hai tai v=1. [Ảnh](reports/figures/train_04_annotation.jpg). |
| Người bị cắt ở mép ảnh | Xét từng khớp: chỉ v=0 khi khớp nằm ngoài mép; phần bị đồ vật che nhưng còn trong ảnh vẫn v=1. Không đổi cờ cả skeleton. | train_01 người #1 có hai cổ chân v=0, người bị cắt ở đáy ảnh. [Ảnh](reports/figures/train_01_annotation.jpg). |
| Cổ tay sau tay lái hoặc thân người | Lần từ vai qua khuỷu của cùng người; nếu cổ tay khuất nhưng còn trong khung, v=1 và đặt chấm ước lượng. | train_06 người #1 right_wrist=1. [Ảnh](reports/figures/train_06_annotation.jpg). |
| Hai người chồng lên nhau | Giữ định danh theo thân và chuỗi chi, không chọn điểm gần nhất chỉ vì khoảng cách. Nếu chưa rõ, rà soát phóng to và ghi bất định. | train_04 người #2 left_wrist đang được công cụ báo nhầm người; đây là ca cần sửa, không phải ví dụ nhãn hoàn hảo. [Ảnh](reports/figures/train_04_annotation.jpg). |
| Người nhỏ | Bộ core yêu cầu gán mọi người, không tự đặt ngưỡng pixel để bỏ qua. Khớp khó thấy cần xét visibility, không xóa cả người. | train_13 bị gold báo thiếu người; phải kiểm người nền trái. [Ảnh](reports/figures/train_13_annotation.jpg). |

Các ảnh dưới đây do công cụ `visualize_pose.py` vẽ từ nhãn của bài. Chúng cung cấp bằng chứng trực quan nhưng **không phải screenshot CVAT**; nếu chấm đúng yêu cầu screenshot của template, cần bổ sung ảnh chụp task CVAT.

## 3. Ba ca mơ hồ được rà soát từ kết quả lab

### Ca 1 - train_04, người #1, left_ear và right_ear

- Mơ hồ: mũ bảo hiểm che tai; dễ nhầm “không thấy” với “ở ngoài ảnh”.
- Trạng thái hiện có: cả hai tai v=1, vẫn có tọa độ.
- Lý do: đầu và vị trí hai tai đều nằm trong khung; mũ là vật che.
- Nếu đổi thành v=0, nhãn mất giám sát vị trí tai bị che. Nếu v=2, cờ không còn mô tả đúng khả năng nhìn thấy.

![Tai bị mũ che trong train_04](reports/figures/train_04_annotation.jpg)

### Ca 2 - train_06, người #1, right_wrist

- Mơ hồ: tay phía xa thân người và xe khó quan sát trực tiếp; dễ gán theo tay phía gần.
- Trạng thái hiện có: right_wrist v=1; lần theo vai phải và khuỷu phải để ước lượng cổ tay.
- Lý do: vùng dự kiến của cổ tay nằm trong ảnh, bị che chứ không bị cắt khỏi khung.
- Nếu chọn v=0 thì mất tọa độ giám sát; nếu nối sang tay bên kia thì sai bên cơ thể.

![Cổ tay phía xa trong train_06](reports/figures/train_06_annotation.jpg)

### Ca 3 - train_01, người #1, left_ankle và right_ankle

- Mơ hồ: phần chân bị cắt ở đáy ảnh; cần phân biệt cổ chân ngoài ảnh với đầu gối vẫn trong ảnh.
- Trạng thái hiện có: cả hai cổ chân v=0, tọa độ YOLO (0, 0); hai đầu gối vẫn v=2.
- Lý do: cổ chân thuộc phần cơ thể vượt mép dưới; không kéo điểm vào mép để đủ điểm nhìn thấy.
- Nếu gán v=1 và đặt chấm bên trong ảnh, model sẽ học vị trí cổ chân giả ở mép ảnh.

![Người bị cắt chân trong train_01](reports/figures/train_01_annotation.jpg)

## 4. Đối chiếu visibility và điều còn cần làm

Bài cá nhân, chưa có bảng của người khác; không tính được khớp lệch %v=1 và không có quy tắc được thống nhất qua kiểm chéo. Bảng riêng có left_ear=57,14%, right_ear=46,43%; left_eye và left_wrist đồng hạng 25%.

Các quy tắc trên hướng dẫn rà soát, chưa có nghĩa toàn bộ 28 skeleton đã tuân thủ hoàn hảo. Chưa sửa nhãn theo model. Nếu rework, sửa trong CVAT dựa trên ảnh và chẩn đoán gold, export lại rồi sinh lại nhãn, visibility và eval; giữ kết quả cũ để so sánh trước/sau.
