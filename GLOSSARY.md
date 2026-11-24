# GLOSSARY.md

Thuật ngữ của bài toán phát hiện bất thường trên đồ thị động có dấu của mạng giao dịch Bitcoin-OTC.

## Language

### Dữ liệu

**User** (Người dùng):
Một tài khoản trên sàn Bitcoin OTC, là một nút của đồ thị.
_Avoid_: **Trader** (Nhà giao dịch), **Account** (Tài khoản)

**Rating** (Lượt đánh giá):
Một lần người dùng u chấm điểm tin cậy cho người dùng v, có điểm đánh giá từ −10 đến +10 và có thời điểm; là một cạnh có hướng u→v.
_Avoid_: **Transaction** (Giao dịch), **Trade** (Giao dịch)

**Snapshot** (Ảnh chụp đồ thị):
Đồ thị có hướng, có dấu gồm mọi lượt đánh giá rơi vào một tuần lịch (thứ Hai–Chủ nhật, giờ UTC); mỗi cạnh mang dấu và điểm đánh giá của lượt đánh giá đó. Chuỗi snapshot theo thứ tự thời gian tạo thành đồ thị động.
_Avoid_: **Time step** (Bước thời gian), **Frame** (Khung)

**Edge-count snapshot** (Snapshot theo số cạnh):
Đồ thị gồm một số lượng cố định các lượt đánh giá liên tiếp theo thời gian, bất kể chúng trải dài bao lâu; chỉ dùng trong biến thể đối chiếu với số đã công bố.
_Avoid_: **Snapshot** khi không kèm chữ "theo số cạnh"

**Discrete-time dynamic graph** (Đồ thị động rời rạc):
Đồ thị động biểu diễn bằng một chuỗi snapshot theo thứ tự thời gian; mọi lượt đánh giá trong cùng một snapshot được coi là đồng thời.
_Avoid_: **Static graph sequence** (Chuỗi đồ thị tĩnh)

**Continuous-time dynamic graph** (Đồ thị động liên tục):
Đồ thị động biểu diễn bằng luồng các lượt đánh giá, mỗi lượt giữ nguyên thời điểm của nó; không gom thành snapshot.
_Avoid_: **Event stream** (Luồng sự kiện) khi muốn nói về cách biểu diễn đồ thị

### Bất thường

**Anomalous edge** (Cạnh bất thường):
Cạnh giả được chèn nhân tạo vào một snapshot, thuộc một trong hai loại: bất thường cấu trúc hoặc bất thường thời gian. Đây chưa phải gian lận thật.
_Avoid_: **Fraud** (Gian lận), **Fake rating** (Đánh giá giả)

**Structural anomaly** (Bất thường cấu trúc):
Cạnh bất thường nối một cặp người dùng chưa từng liên kết, hoặc thuộc một cụm nhỏ nối dày đặc bất thường.

**Temporal anomaly** (Bất thường thời gian):
Cạnh bất thường thể hiện một người dùng có lịch sử ổn định đột ngột nối tới nhiều người dùng lạ.

**Anomalous node** (Nút bất thường):
Người dùng là nút nguồn (người chấm) của ít nhất một cạnh bất thường trong một snapshot; tính theo từng snapshot, không cố định cho cả lịch sử. Nút đích của cạnh bất thường không bị coi là bất thường.
_Avoid_: **Fraudster** (Kẻ gian lận), **Malicious user** (Người dùng độc hại)

**Trusted user** (Người dùng tin cậy):
Người sáng lập sàn Bitcoin OTC và những người dùng được người sáng lập chấm từ +5 trở lên.

**Fraudulent user** (Người dùng gian lận):
Người dùng bị những người dùng tin cậy đồng loạt chấm từ −5 trở xuống; là nhãn thật cho cả lịch sử, khác với nút bất thường vốn do chèn nhân tạo và tính theo từng snapshot.
_Avoid_: **Anomalous node** (Nút bất thường) khi nói về nhãn thật

**Flag week** (Tuần bị đánh dấu):
Tuần đầu tiên một người dùng gian lận bị một người dùng tin cậy chấm từ −5 trở xuống.

**Node anomaly score** (Điểm bất thường nút):
Con số gán cho mỗi người dùng trong một snapshot: điểm bất thường cao nhất trong các cạnh mà người dùng đó là nút nguồn.

**Current-snapshot scoring** (Chấm snapshot hiện tại):
Cách chấm điểm trong đó biểu diễn dùng để chấm các cạnh của snapshot t được tính từ chính snapshot t, kể cả các cạnh đang được chấm.
_Avoid_: **Reconstruction** (Tái tạo) khi muốn nói về cách chấm chứ không phải hàm mất mát

**Next-snapshot prediction** (Dự đoán snapshot kế tiếp):
Cách chấm điểm trong đó các cạnh của snapshot t được chấm chỉ bằng thông tin đến snapshot t−1; cạnh càng khó dự đoán thì càng bất thường.
_Avoid_: **Link prediction** (Dự đoán liên kết) khi muốn nói về cách chấm điểm bất thường

**Temporal attention** (Attention theo thời gian):
Cơ chế học xem trạng thái của tuần quá khứ nào trong một cửa sổ gần đây đáng nhìn lại nhất khi cập nhật trí nhớ của một người dùng.
_Avoid_: **Attention** đứng một mình, **Neighbor attention** (Attention trên hàng xóm)

**Edge anomaly score** (Điểm bất thường cạnh):
Con số mô hình gán cho mỗi cạnh trong một snapshot; càng cao thì cạnh càng khó xảy ra so với lịch sử kết nối.
_Avoid_: **Anomaly probability** (Xác suất bất thường)
