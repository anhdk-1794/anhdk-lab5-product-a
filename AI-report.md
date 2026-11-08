# AI-report: cách dùng AI ở phần này

Báo cáo này viết từ những gì đã xảy ra trong phiên làm việc, đối chiếu với nhật ký làm việc của phiên. Chỗ nào chưa kiểm hoặc do AI tự xác nhận thì ghi rõ.

---

## 1. Skill, agent và prompt cho từng việc

Phiên chính (một phiên Claude Code) điều phối; các việc đọc nhiều tài liệu giao cho agent phụ. Mỗi agent chỉ nhận tài liệu cần dùng, không nhận cả lịch sử phiên. Không dùng công cụ Workflow (người phụ trách dự án không yêu cầu).

| Việc | Công cụ | Prompt, tóm tắt |
|---|---|---|
| Break task (phần 1) | Agent `general-purpose-high`, chạy một mình | Đọc bộ spec Lab 4 và prototype Lab 2, chia thành epic → issue → sub-issue; mỗi issue có mô tả, tiêu chí xong, phụ thuộc, nhãn nguồn `[spec]` hoặc `[suy luận]`; mỗi mã `T-…`, `W-…` phải có ít nhất một issue; ghi riêng các chỗ hở của spec. Kết quả là bảng 12 cột trong một file làm việc |
| `PREMISE.md` và `task-breakdown.md` (phần 2) | Agent `general-purpose-high` | Mở lại spec để xác minh từng dòng của bản phần 1, soạn tiền đề (thay đổi, giả định, hướng đã chốt) và bảng spec → issue; mỗi dòng phải dẫn nguồn cụ thể |
| Thêm loại ticket và 4 issue triển khai, nghiệm thu | Agent `general-purpose-high` | Sau nhận xét của người phụ trách dự án: dùng cấu trúc ticket của một dự án mẫu (viết lại, không chép), gán loại và nhãn team cho mọi issue, thêm việc còn thiếu |
| Thêm task viết test case, tách thiết kế và chạy test | Agent `general-purpose-high` | Mỗi thiết kế test một ticket, mỗi chạy test một ticket, thiết kế không phụ thuộc issue dev |
| Tạo repo, Project, 120 issue, sub-issue, nhãn, trường | `gh` (lệnh dòng lệnh) và script Python của phiên | Tôi viết script đọc bảng backlog, tạo issue, gắn sub-issue, thêm vào Project, đặt trạng thái; chạy lại được (idempotent) vì lần đầu dừng giữa chừng |
| Chấm SWP | Skill `tkm:sun-estimate` (bản 2.0.0-beta.1) và 5 agent `sun-estimate-worker` (mỗi lô 21 đến 22 issue) | Bộ chấm cho điểm phạm vi và độ khó theo bảng cố định. Worker **không có công cụ**, nên tôi nhét nguyên văn bằng chứng của từng issue vào prompt; kết quả JSON chỉ về qua tin nhắn |
| Quy đổi giờ | Agent `general-purpose-high` (đối chiếu estimate Lab 1) và script của tôi | Agent đọc estimate Lab 1, đối ứng phần Lab 1 với từng epic, đề nghị hệ số giờ/SWP có nguồn từng ô; tôi tính lại bằng script (làm tròn lên 0,5 giờ) |
| Báo cáo tiến độ | Tôi viết trực tiếp, không dùng skill | Dựng sprint mô phỏng từ đồ thị phụ thuộc thật của backlog (chọn việc theo thứ tự phụ thuộc, công suất 3 người), rồi viết bảng trạng thái, burndown, rủi ro; ghi rõ "mô phỏng" |

**Cách ghi SWP và giờ lên Project.** Skill chấm SWP chỉ đọc, không ghi lên Project (đó là ranh giới thiết kế của skill). Tôi viết SWP và giờ vào Project bằng `gh project item-edit` cho 108 issue, sau khi người phụ trách dự án đồng ý. Sau khi ghi, tôi đọc lại toàn bộ Project và so với bảng tính: 0 issue lệch. Trước khi ghi có chốt ba điểm với người phụ trách dự án: tỷ lệ dự phòng sửa lỗi, tách #89, và việc ghi hàng loạt.

---

## 2. Review output của AI

### 2.1 Ai review ở bước nào

| Bước | Người review | Cách review |
|---|---|---|
| Backlog | Người phụ trách dự án | Duyệt độ hạt, cách quy giờ, sprint, tên repo; sau đó chỉ ra thiếu việc triển khai và test case, và việc gộp thiết kế test với chạy test. Các sửa đổi đều đến từ người, không phải từ AI tự phát hiện |
| Backlog phần 1 | Agent phần 2 (AI) | Mở lại spec kiểm từng dòng. Bắt được 3 dòng sai (xem 2.2) |
| Kết quả chấm SWP | Bộ kiểm hợp lệ của skill và tôi | Kiểm cấu trúc và thứ tự mã |
| 5 issue mẫu của skill | **Chính AI (tôi)**, theo yêu cầu "bạn review là được" | Giữ nguyên cả 5 điểm. Không có người đọc lại; đây là điểm yếu (xem 3) |
| Giờ | Người phụ trách dự án | Chốt hệ số dự phòng 20% và việc tách #89 |
| Báo cáo tiến độ | Chưa có người rà soát | Chỉ tôi đọc lại. Số liệu là mô phỏng |

### 2.2 Lỗi của AI phát hiện được và cách xử lý

Các dòng dưới đây trùng với bảng "Chỗ AI sinh sai" của nhật ký làm việc.

| Lỗi của AI | Phát hiện bằng | Xử lý |
|---|---|---|
| Backlog phần 1 nói spec chưa nêu việc chờ ngày kế khi hôm nay đã khóa; ADR-003 mục 4.2 điểm 9 đã nêu | Agent phần 2 mở lại spec | Đổi từ "sửa thiết kế" sang giả định G-13 |
| Backlog phần 1 nói tra cứu audit "đã nằm trong architecture 12.9"; mục 12.9 không liệt kê | Agent phần 2 đối chiếu mục 12.9 | Ghi "để mở" ở `task-breakdown.md` |
| Backlog phần 1 dẫn nhầm điểm D001 (200 dòng) làm bằng chứng giới hạn 1.000 dòng của J002 | Agent phần 2 mở lại dòng | Dẫn lại J002 |
| Không có ticket triển khai, nghiệm thu, rà soát, tài liệu vận hành | Người phụ trách hỏi "task có implement, test chưa" | Thêm 4 issue, nhãn `[suy luận]`, ghi G-27 |
| Không có ticket viết test case; 12 ticket gộp thiết kế và chạy test | Người phụ trách chỉ ra từ ví dụ #80 | Tách thành 20 thiết kế + 20 chạy test |
| Script tạo issue: thừa dấu nháy, quên đổi mã tạm I-xxx trong mô tả (48 issue), dừng ở issue thứ 8 | Người phụ trách đọc #80; tôi quét 117 ticket; log lỗi | Sửa và chạy lại; quét lại 0 mã tạm sót |
| Worker chấm SWP trả mảng chữ ký sai thứ tự; #63 xếp `Unknown` nhưng tin cậy `medium` | Bộ kiểm hợp lệ của skill | Sắp lại bằng script (không đổi nội dung); đặt #63 tin cậy `low` |
| Worker không có công cụ nên transcript trống; tôi ghi lại tay 107 dòng và có lần ghi sai | Thấy file trống, lỗi cú pháp | Ghi lại bằng công cụ Write, kiểm 107/107 dòng |
| #89 chạy migration và triển khai ước 15 SWP (82 giờ), quá lớn | Tôi tính giờ rồi so với tiêu chí "vài ngày" | Tách thành #89 và #118 (14 SWP). Hai điểm mới do tôi tra bảng tay, không phải skill chấm lại |
| Bản đầu của `estimate.md`: ghi sai số issue test (34 thay vì 30), dẫn nhầm mã giả định (G-01 cho việc bảng chợ) | Đọc lại từng số với dữ liệu | Sửa đúng số, bỏ dẫn nhầm |
| Bản đầu `progress-report.md`: ghi "5 issue xong, 120 giờ" trong khi 120 giờ là giờ đã làm gồm phần dở (5 issue xong là 56,5 giờ); và không theo đúng cột bảng của đề | Đọc lại số với bảng chi tiết; đọc lại yêu cầu của đề | Viết lại đúng cột, đúng số |

### 2.3 Nhược điểm của phương pháp quy giờ

Chỗ này AI **không** sai theo nghĩa số liệu, nhưng phải nói rõ để không bị hiểu nhầm: hệ số giờ/SWP lấy từ phần của estimate Lab 1 đối ứng với Lab 5 rồi chia cho SWP của chính phần đó, nên tổng giờ Lab 5 gần giờ Lab 1 là **do cách làm**, không phải chứng cứ ước lượng đúng. Chỉ chênh lệch theo epic (xem `estimate.md`, mục 5) mới có giá trị đọc.

---

## 3. Điểm đặc biệt trong cách dùng AI

**Chỗ tôi tin là hiệu quả.**
- Giao agent phụ mở lại spec kiểm từng dòng: bắt được 3 lỗi trích dẫn của bản đầu mà phiên chính không thấy.
- Chạy script quét toàn bộ (mã tạm còn sót, đếm số issue, đọc lại Project sau khi ghi): đọc ngược từ nguồn thật bắt lỗi rẻ hơn đọc lại bằng mắt.
- Chia việc thành bước nhỏ có điểm dừng để người phụ trách duyệt (độ hạt, loại ticket, tách test, tách #89): các sửa đổi lớn nhất đều đến từ những lần dừng này.
- Dùng bảng chấm điểm cố định của skill thay vì tự cho SWP: hai issue tương tự có điểm giống nhau, có lý do viết ra.

**Chỗ chưa tốt, cần cải thiện khi làm thật.**
- **SWP chưa có người rà soát.** 5 issue mẫu do chính AI xác nhận theo yêu cầu "bạn review là được". Khi làm thật, 5 issue mẫu phải do người (tech lead) xem; nên mở rộng ra 10 đến 15 issue vì AI chấm đồng đều và dễ lệch cùng một hướng.
- **Giờ quy từ ước lượng thô.** Cần giờ thực của sprint đầu để hiệu chỉnh hệ số; sprint mô phỏng không cung cấp được thông tin này.
- **Sprint mô phỏng là mô phỏng.** Burndown, trạng thái và hai lỗi do tôi dựng để minh họa. Không nên dùng làm bằng chứng về năng suất.
- **Ghi lên Project phải làm bằng script do tôi viết**, vì skill chấm điểm không ghi. Mỗi lần ghi hàng loạt phải đọc lại để đối chiếu.
- **Bảng backlog lớn trong một file markdown** (12 cột, hơn 100 dòng) khó sửa tay và khó kiểm; nên có kho dữ liệu có cấu trúc (CSV hay JSON) ngay từ đầu.

**Quản lý context.** Phiên rất dài và bị tóm tắt một lần giữa chừng. Để không mất việc: mỗi agent chỉ nhận tài liệu cần dùng; kết quả trung gian ghi ra file làm việc ngoài repo (bảng backlog, ánh xạ mã sang số issue, bảng SWP và giờ); hai bước (ghi Project, dựng sprint) có script chạy lại được. Chưa có ý tưởng tốt hơn cho việc giữ nguyên văn kết quả của agent không có công cụ (worker chấm SWP): hiện phải ghi lại bằng tay từ tin nhắn.
