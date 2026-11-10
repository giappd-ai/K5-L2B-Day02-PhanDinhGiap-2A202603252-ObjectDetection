# Báo cáo cá nhân — Ngày 2 Object Detection

Đổi tên file theo `MSSV_HoTen.md`, ví dụ `20241234_NguyenVanAn.md`. Điền kết quả thật, xóa dòng hướng dẫn khi nộp. Xem [quy định nộp](../../SUBMISSION.md).

## 1. Thông tin học viên

- Họ tên:Phan Đình Giáp
- MSSV:2A202603252
- Email:giappdhe140952@gmail.com
- Lớp:L2B
- Link repo bài làm:https://github.com/giappd-ai/K5-L2B-Day02-PhanDinhGiap-2A202603252-ObjectDetection
- Link Colab đã chạy:https://colab.research.google.com/drive/1wW45vGJ7JF_4TfUKINLAvrTb0IuiQI-9?usp=sharing

## 2. Schema và mini guideline

Dẫn tới [GUIDELINE_MINI.md](https://github.com/giappd-ai/K5-L2B-Day02-PhanDinhGiap-2A202603252-ObjectDetection/blob/main/GUIDELINE_MINI.md). Tóm tắt ba ca mơ hồ đã quyết định, cách áp dụng schema và phần cần giảng viên làm rõ.

## 3. Annotation và self-QC

- Số file nhãn train:50 file nhãn 
- Kết quả checker và lỗi đã xử lý:50
- Danh sách ít nhất 10 ảnh đã rà bằng mắt:drive_001.jpg, drive_005.jpg, drive_010.jpg, drive_015.jpg, drive_020.jpg, drive_025.jpg, drive_030.jpg, drive_035.jpg, drive_040.jpg, drive_045.jpg.
- Lỗi bỏ sót/box/class phát hiện khi rà và cách sửa:Nhầm lẫn giữa SUV kích thước lớn (car) và xe gia đình nhỏ (van).
Bỏ sót các xe ở rìa ảnh khuất bóng hoặc bị che khuất một phần. Đã bổ sung box và hiệu chỉnh khớp ranh giới vật thể theo đúng guideline.
- Commit khóa nhãn ban đầu:

## 4. Gold QC và rework

| Chỉ số | Trước sửa | Sau sửa nếu có |
|---|---:|---:|
| TP |75| 88
| FP |14|8|
| FN |25| 12
| Precision |0.842| 0.916
| Recall |0.750 | 0.880|
| F1 |0.0.793 | 0.897
| mean_iou_of_matches |0.812 | 0.854
| passed |True |True |

Dẫn tới `outputs/annotation_evaluation.json` trong repo. Nếu rework, ghi ảnh, lỗi đã kiểm, quyết định sửa/giữ và lý do. Không suy nguyên nhân chỉ từ FP/FN. Không có rework thì ghi rõ.

## 5. Kết quả YOLO

- Runtime, GPU/CPU, phiên bản Ultralytics và Torch:GPU: Tesla T4 (15GB VRAM)
Ultralytics: v8.4.208
PyTorch: v2.14.1+cu130
- Các tham số thay đổi so với notebook nếu có và lý do:Giữ nguyên các tham số chuẩn (epochs=50, imgsz=640, batch=-1 tự động tối ưu hóa, seed=20260909).
- Đường dẫn best checkpoint trong phiên chạy:/content/Day2-Lab/outputs/yolo26_runs/vehicle_annotation/weights/best.pt

| Chỉ số trên 10 ảnh eval | Giá trị |
|---|---:|
| Precision |0.4105453698519763 |
| Recall | 0.23026854853628678|
| mAP50 | 0.27225886498171903|
| mAP50–95 |0.20168300309748077 |

Đặt cả 2 folder `assets/eval_predictions/` và `assets/yolo26_runs/` (giải nén từ `day2_colab_outputs.zip`) cạnh báo cáo và dẫn link tương đối. Không chỉ ghi số; chỉ ra ít nhất một ảnh và vật thể cụ thể.

Trả lời bốn câu hỏi cuối notebook:

1. Lớp nào recall thấp nhất? Ảnh nào minh họa?
Lớp có recall thấp nhất: Lớp bus (Recall bằng 0.0), tiếp theo là van (Recall chỉ đạt 0.0833).
Ảnh minh họa:
Trong ảnh drive_053.jpg, mô hình dự đoán ra tận 3 buses và 1 van nhưng thực tế Recall của lớp bus vẫn bằng 0. Điều này cho thấy mô hình đang bị sai lệch nghiêm trọng (dự đoán sai vị trí hoặc gán nhãn nhầm từ car/truck sang bus), khiến cho không có đối tượng bus thực tế nào được khớp đúng (True Positive = 0).
Hoặc ở ảnh drive_052.jpg và drive_058.jpg, mô hình cũng đưa ra dự đoán bus nhưng đều bị trượt hoặc sai nhãn so với nhãn chuẩn (Ground Truth).
2. Nếu mAP50 cao nhưng mAP50–95 thấp, cần kiểm điều gì trên box?
Vấn đề của bounding box: Khi mAP50 cao nhưng mAP50-95 thấp, điều này nghĩa là mô hình tìm và nhận diện đúng lớp đối tượng rất tốt ở mức bao phủ đại khái, tuy nhiên độ chính xác về tọa độ của bounding box rất kém.
Các bounding box dự đoán có thể đang bị lệch viền, quá rộng so với vật thể thực tế, hoặc cắt bớt một phần của xe (không ôm khít lấy toàn bộ thân xe theo đúng guideline). Khi tăng ngưỡng IoU lên cao hơn (ví dụ 0.75 hoặc 0.95), các box lệch này không còn được tính là chính xác nữa, kéo chỉ số mAP50-95 xuống thấp.
3. Lỗi class nào có thể liên quan guideline? Dữ kiện nào hỗ trợ nhận xét?
Có sự nhầm lẫn lớn giữa các lớp xe:
Lớp car có kết quả vượt trội hoàn toàn với mAP50 = 0.739 và Precision = 0.833.
Ngược lại, lớp van (mAP50 = 0.103) và bus (mAP50 = 0.0078) có kết quả rất tệ.
Mối liên quan tới Guideline:
Người gán nhãn rất dễ nhầm lẫn giữa các dòng xe SUV cỡ lớn, xe bán tải, xe cứu thương hay xe 16 chỗ giữa các lớp car, van, và truck nếu không bám sát định nghĩa kích thước/mục đích sử dụng của guideline.
Do tập train chỉ có 50 ảnh (số lượng mẫu của bus, van, truck cực kỳ ít), cộng với việc gán nhãn không đồng nhất giữa các học viên ở các ranh giới mập mờ này khiến mô hình bị nhiễu và không thể học được đặc trưng phân biệt giữa chúng.
4. Vì sao không dùng 10 ảnh eval để train? Vai trò validation của chúng trong notebook là gì?
Vì sao không dùng để train:
Để đảm bảo tính khách quan và khả năng tổng quát hóa (generalization) của mô hình. Nếu đưa cả 10 ảnh eval vào quá trình huấn luyện, mô hình sẽ học thuộc lòng (overfit) các đặc trưng của 10 ảnh này. Khi đó, kết quả đánh giá mAP sẽ cao một cách ảo tưởng nhưng mô hình sẽ thất bại khi gặp các dữ liệu thực tế mới.
Vai trò Validation trong notebook:
Làm thước đo độc lập để theo dõi hiệu năng của mô hình qua từng epoch.
Giúp thuật toán Early Stopping hoặc cơ chế lưu checkpoint tự động tìm ra điểm tối ưu để lưu lại trọng số tốt nhất (best.pt) trước khi mô hình bắt đầu bị quá khớp (overfitting) với tập train 50 ảnh.
## 6. Khó khăn và cách xử lý

| Bước/thời điểm | Lỗi quan sát | Cách đã thử | Kết quả/còn thiếu |
|---|---|---|---|
| | | | |

## 7. Tự đánh giá theo rubric

Đối chiếu [RUBRIC.md](../../RUBRIC.md): phần nào đã có kết quả, phần nào còn thiếu hoặc chưa kiểm được. Không tự nhận hoàn thành bước chưa chạy.

## 8. Công cụ AI và tài liệu tham khảo

Ghi công cụ đã dùng, dùng ở bước nào và phần đã tự kiểm tra; không dùng thì ghi rõ. Liệt kê nguồn ngoài repo được sử dụng. Không ghi mật khẩu/token hoặc đính kèm gold train.
