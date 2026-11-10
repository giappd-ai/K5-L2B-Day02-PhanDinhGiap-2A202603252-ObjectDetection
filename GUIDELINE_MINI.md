# Mini guideline — bài làm cá nhân

> Điền nội dung này trước khi bắt đầu gán nhãn. Giữ lại các quyết định sau khi self-QC.

## Định nghĩa lớp

| Lớp | Định nghĩa của bạn | Ví dụ/điểm phân biệt |
| --- | --- | --- |
| `car` |Phương tiện ô tô chở người, có thân xe tương đối thấp, khoang hành khách và khoang động cơ hoặc cốp được thiết kế theo kiểu ô tô con. |Sedan, hatchback, coupe, SUV, crossover. Phân biệt với van: car thường có thân xe thấp hơn, thiết kế thân xe theo kiểu ô tô con; van thường có mui cao, khoang xe dạng hộp và không gian phía sau kéo dài. |
| `truck` |Phương tiện cơ giới được thiết kế chủ yếu để vận chuyển hàng hóa, thường có kết cấu cabin lái và khu vực chở hàng phía sau. |Xe tải thùng kín, xe tải thùng bạt, xe ben, xe tải đông lạnh, xe bán tải (pickup). Phân biệt với van: truck thường có khu vực chở hàng tách biệt hoặc dễ nhận thấy so với cabin; van thường có thân xe liền khối và khoang hàng kín. |
| `bus` |Phương tiện được thiết kế chủ yếu để vận chuyển nhiều hành khách, có khoang hành khách lớn và thân xe thường dài, cao. |Xe buýt thành phố, xe khách đường dài, xe buýt trường học, xe buýt hai tầng. Phân biệt với van: bus thường có thân xe lớn hơn, nhiều cửa sổ hành khách, không gian hành khách rộng và kết cấu chuyên chở số lượng lớn người. |
| `van` | Phương tiện có thân xe dạng hộp, mui cao, khoang phía sau rộng và thường kéo dài liền với cabin. Có thể được thiết kế để chở người hoặc vận chuyển hàng hóa| Cargo van, passenger van, minivan, xe van chở khách. Phân biệt với car: van thường cao và vuông vức hơn, có khoang phía sau rộng. Phân biệt với truck: van thường có thân xe liền khối, khoang hàng nằm bên trong thân xe thay vì thùng hàng tách biệt phía sau cabin. Phân biệt với bus: van nhỏ gọn hơn và có sức chứa hành khách thấp hơn.|

## Quy tắc chung

- Box bám sát vật thể nghĩa là: Vẽ hình chữ nhật bao quanh sát toàn bộ phần thân xe nhìn thấy được. Box phải bao gồm thân xe, không lấy thêm mặt đường, vỉa hè hoặc phương tiện bên cạnh. Mỗi phương tiện phải có một box riêng và đúng class.
- Vật thể bị che khuất: Nếu xe bị phương tiện khác che khuất một phần nhưng vẫn nhận diện được loại xe, vẫn tiến hành gán nhãn. Box chỉ bao quanh phần thân xe nhìn thấy được, không ước lượng hoặc tự vẽ phần bị che khuất.
- Vật thể bị cắt bởi rìa ảnh: Nếu xe nằm một phần ngoài khung hình, vẽ box đến đúng cạnh ảnh tại vị trí xe bị cắt. Không mở rộng box vượt quá kích thước ảnh và không tự suy đoán phần xe nằm ngoài khung hình.
- Vật thể rất nhỏ/mơ hồ: Với xe ở xa, có kích thước nhỏ hoặc hình ảnh bị mờ, chỉ gán nhãn khi có đủ đặc điểm trực quan để xác định chắc chắn một trong bốn class car, truck, bus, van. Nếu không đủ căn cứ, không tự đoán nhãn; ghi lại Case ID và hỏi giảng viên.

## Ghi lại quyết định cho ít nhất ba ca mơ hồ

| Ảnh | Tình huống | Quyết định cuối | Lý do theo schema |
| --- | --- | --- | --- |
| `drive_005.jpg` |Hai xe ô tô màu trắng nằm gần nhau ở phía dưới bên trái ảnh, một phần xe bị cắt bởi cạnh trái và cạnh dưới.| Gán hai box riêng với class car. Box của từng xe chạm đúng rìa ảnh tại vị trí bị cắt.|Mỗi phương tiện là một instance độc lập. Xe vẫn nhận diện được là car nên phải gán nhãn phần thân xe nhìn thấy được, không bỏ sót hoặc gộp hai xe. |
| `drive_005.jpg` |	Các phương tiện ở khu vực giữa ảnh nằm gần nhau và bị che khuất một phần. | Gán box riêng cho từng phương tiện nếu vẫn xác định được class. Không gộp các phương tiện vào một box.|Theo quy tắc Instance Separation và Occlusion, mỗi xe hợp lệ phải có một bounding box riêng, chỉ bao quanh phần nhìn thấy được. |
| `drive_005.jpg` | Các phương tiện ở khu vực phía trên ảnh có kích thước rất nhỏ, một số bị mờ do khoảng cách và chất lượng ảnh.| Chỉ gán nhãn khi xác định chắc chắn class. Trường hợp không thể phân biệt car, van hoặc bus thì không tự suy đoán, ghi nhận và hỏi giảng viên.|Đảm bảo Class Accuracy. Không gán nhãn dựa trên suy đoán khi hình ảnh không cung cấp đủ đặc điểm nhận diện. |
