# Estimate: Lab 5, Product A

File này ghi cách chấm điểm công việc (story point, viết tắt SWP) và quy ra giờ cho 108 issue của backlog. Mã `#N` là số issue trên GitHub Project. Nhãn `[giả định]` là điều tôi chọn vì không có dữ liệu; `[suy luận]` là rút ra từ số liệu bằng một bước tính.

---

## 1. Báo cáo HTML của skill chấm SWP

- File: [`reports/sun-estimate-lab5-product-a.html`](reports/sun-estimate-lab5-product-a.html). Mở bằng trình duyệt, không cần máy chủ.
- Skill dùng: `tkm:sun-estimate` (bản 2.0.0-beta.1). Skill chỉ đọc issue và chấm điểm, không ghi gì lên Project. SWP và số giờ do tôi ghi bằng lệnh `gh`, kiểm lại bằng cách đọc ngược từ Project (xem mục 3.5).
- Cách skill chấm: mỗi issue có hai điểm. Phạm vi (Scope) thuộc {1, 2, 3, 5, 8, 13}; độ khó (Complexity) thuộc {0, 1, 2, 3, 5, 8}. SWP lấy từ một bảng tra cố định theo hai điểm này (ví dụ phạm vi 3, độ khó 2 → 5 SWP). Epic là ticket cha nên không chấm riêng, chỉ cộng các issue con.
- Báo cáo chấm **107 issue**. Sau đó tôi tách #89 thành hai issue (#89 và #118) theo quyết định của người phụ trách dự án, nên **báo cáo HTML chưa có #118 và vẫn ghi #89 là 15 SWP**. Hai điểm mới của #89 và #118 do tôi tra bảng tay, không phải skill chấm lại (xem mục 2). Bộ số trên Project là bộ cuối cùng.

---

## 2. Issue mẫu: điểm skill chấm và quyết định cuối

Skill chọn 5 issue đại diện (chọn theo cách cố định, không ngẫu nhiên) để người rà soát xác nhận hoặc sửa điểm. **Người rà soát ở đây là chính AI (tôi), theo yêu cầu của người phụ trách dự án ("bạn review là được").** Không có người đọc lại từng điểm, nên các điểm này chưa có kiểm chứng độc lập [ghi vào `AI-report.md`].

| Issue | Skill chấm (phạm vi / độ khó → SWP) | Quyết định cuối | Lý do |
|---|---|---|---|
| #20 Kiểm tra "người lập khác người duyệt" trên một bảng | 1 / 1 → 1, tin cậy cao | Giữ 1 SWP | Một ràng buộc trên một bảng cộng test SQL, hàm hiện có không đổi |
| #63 PM chốt năm câu hỏi với khách | 2 / 1 → 3, tin cậy **thấp** | Giữ 3 SWP | Việc điều phối, không viết mã. Tin cậy thấp vì thời gian chờ khách không đo được, không phải vì khối lượng |
| #81 Test đồng thời (nhiều kiểu: cùng giao dịch, cùng lô, cùng bên mua, trùng mã) | 5 / 3 → 10, tin cậy cao | Giữ 10 SWP | Nhiều kiểu đồng thời cộng phát hiện bế tắc; khó hơn test ma trận (#80) một bậc |
| #90 Hỗ trợ nghiệm thu, chạy lại kịch bản bị ảnh hưởng | 3 / 1 → 4, tin cậy vừa | Giữ 4 SWP | Phần sửa lỗi nằm ở khoản dự phòng riêng, kịch bản nghiệm thu nằm ở issue thiết kế riêng nên không cộng thêm |
| #89 Chạy migration và triển khai lên môi trường chạy thật | 5 / 5 → 15, tin cậy cao | **Tách đôi** (quyết định của người phụ trách dự án) | Xem dưới |

**Về #89.** Với hệ số giờ ở mục 3, 15 SWP = 82 giờ, lớn nhất backlog và vượt tiêu chí "một người làm xong trong vài ngày". Tách thành hai ticket nối tiếp nhau:

| Issue | Việc | Phạm vi / độ khó → SWP | Giờ |
|---|---|---|---|
| #89 | Lập thứ tự migration, kế hoạch sao lưu và hoàn tác | 3 / 2 → 5 | 27,5 |
| #118 | Chạy migration trên môi trường chạy thật, đếm dòng trước và sau, kiểm sau triển khai | 3 / 5 → 9 | 49,5 |

Hai điểm này tôi tra từ cùng bảng của skill, theo ý tôi cho độ khó 2 (lập kế hoạch, có tài liệu) và 5 (chạy thật, có migration không hoàn tác được). Tổng 14 SWP, 77 giờ, thấp hơn ticket gộp (15 SWP, 82 giờ) một chút. Đây là chấm tay của AI, độ tin cậy vừa. #118 vẫn trên 40 giờ, giữ nguyên và ghi vào rủi ro.

---

## 3. Quy đổi SWP ra giờ

### 3.1 Hệ số giờ trên mỗi SWP

Lab 1 không có SWP, nên không có hệ số "gốc". Cách suy: lấy giờ Lab 1 của phần đã đối ứng (xem mục 5), chia cho tổng SWP Lab 5 của đúng phần đó, theo từng nhóm.

| Nhóm (nhãn team) | Giờ / SWP | Căn cứ | Tin cậy |
|---|---|---|---|
| `team:database` và `team:backend` | 5,45 | 1.010,4 giờ viết mã và test đơn vị của Lab 1 (phần đối ứng E-01 đến E-07) chia 185 SWP Lab 5 = 5,46 | Vừa đến thấp. Lab 1 không tách database và backend, nên **dùng chung một hệ số là [giả định]** |
| `team:frontend` | 2,10 | 127,1 giờ Lab 1 (dòng giao diện của các màn tương ứng) chia 60 SWP = 2,12 | Thấp. Đối ứng màn dựa trên tên issue |
| `team:qa` | 5,35 | 1.157,0 giờ kiểm thử Lab 1 chia 216 SWP = 5,36 | Vừa. Lab 1 chỉ có test thủ công, Lab 5 có thêm test tự động |
| `team:pm` | 3,80 | [giả định] trung bình giao diện (2,10) và backend (5,45). Lab 1 không có việc tương đương | Rất thấp, nhưng ảnh hưởng nhỏ: 7 SWP ≈ 27 giờ |

Không tách hệ số riêng cho thiết kế test và chạy test: tách chỉ chuyển giờ giữa hai loại, tổng không đổi, và nhiều issue "chạy test" của Lab 5 là chạy lệnh tự động, rẻ hơn chạy tay.

**Độ nhạy:** nếu chọn cách tính khác thì hệ số database và backend nằm trong khoảng 3,25 đến 5,45, giao diện 1,20 đến 2,10, kiểm thử 5,35 đến 7,45. Tức giờ có thể thấp hơn khoảng 40% so với con số cuối. Tôi chọn đầu trên vì hệ số suy ra từ Lab 1 đã gồm cả test đơn vị.

### 3.2 Làm tròn

Giờ issue = SWP × hệ số, **làm tròn lên bội số 0,5 giờ**. Ví dụ 5 SWP database = 27,25 → 27,5 giờ. Làm tròn cộng thêm khoảng 0,8% tổng (khoảng 21 giờ), chấp nhận được.

### 3.3 Dự phòng sửa lỗi: 20%, không nằm trong giờ từng issue

Backlog không dựng sẵn issue sửa lỗi (lỗi phát sinh khi test sẽ được tạo thành `[BUG FIX]` khi gặp, xem PREMISE dòng 86). Nên cần một khoản dự phòng riêng, nếu không giờ Lab 5 thấp hơn Lab 1 chỉ vì cách trình bày.

- Lab 1 tính sửa lỗi bằng 1/3 giờ viết mã, đo trên phần đối ứng ra khoảng **20%** của (viết mã + test đơn vị) [suy luận]. Người phụ trách dự án đã xác nhận dùng 20%, không dùng 15%.
- Áp cho issue `type:devtask` của team database, backend, frontend (66 issue, 1.271,5 giờ). Không áp cho test, epic và `team:pm`.
- Kết quả: 1.271,5 × 20% = **254,3 giờ**. Con số 20% không có dữ liệu thật của khách làm cơ sở.

### 3.4 Điều chưa chắc

1. Tổng giờ gần phần đối ứng của Lab 1 là **do cách làm, không phải bằng chứng ước lượng đúng**: hệ số suy ra từ chính phần Lab 1 đó. Điều có giá trị để đọc nằm ở chênh lệch theo epic (mục 5), không phải ở tổng.
2. Lab 1 là ước lượng thô (sai số ±30%) và phần kiểm thử chưa có người đứng đầu QA rà soát.
3. SWP do AI chấm, 5 issue mẫu cũng do AI xác nhận (mục 2).

### 3.5 Số liệu đã ghi lên Project và kiểm lại

Ghi **108 issue có điểm** (SWP và giờ ước lượng). 9 epic và issue cha #44 (gom 5 sub-issue) để trống vì là ticket cha, giá trị là tổng các con. Đọc lại toàn bộ Project sau khi ghi: **0 issue lệch** so với bảng tính; tổng 108 issue là 497 SWP và 2.496,0 giờ, khớp với mục 4.

Sau đó, khi dựng sprint mô phỏng (xem [progress-report.md](progress-report.md)), tôi thêm 2 issue lỗi mô phỏng (#119: 1 SWP, 5,5 giờ; #120: 2 SWP, 11,0 giờ). Hai issue này **không nằm trong estimate** và không cộng vào bảng mục 4: chúng minh họa việc tiêu khoản dự phòng sửa lỗi. Vì vậy tổng đọc trực tiếp từ Project là 500 SWP và 2.512,5 giờ; trừ hai lỗi mô phỏng thì còn 497 SWP và 2.496,0 giờ.

---

## 4. Tổng theo epic

Giờ chưa gồm dự phòng. "Giờ dev" là giờ của issue lập trình (database, backend, frontend), không gồm test và PM.

| Epic | Số issue | SWP | Giờ | Giờ dev | Ghi chú |
|---|---|---|---|---|---|
| E-01 Nền dữ liệu và chợ | 8 | 36 | 198,0 | 198,0 | Có bảng chợ nhiều chợ và chuyển hồ sơ mẫu |
| E-02 Phân quyền, chính sách theo hàng (RLS) | 3 | 12 | 66,0 | 66,0 | |
| E-03 Tư cách tham gia và kiểm khi chốt giao dịch | 7 | 31 | 170,5 | 170,5 | |
| E-04 Quy tắc theo chợ và phiên bản | 5 | 13 | 71,5 | 71,5 | |
| E-05 Khóa ngày và điều chỉnh sau khóa | 12 | 49 | 269,5 | 236,5 | 33,0 giờ là hai issue test cơ sở dữ liệu |
| E-06 Tiền thưởng thanh toán đúng hạn (完納奨励金) | 18 | 77 | 414,0 | 165,0 | Tách riêng đúng yêu cầu; 237,5 giờ test, 11,5 giờ PM (#63) |
| E-07 API và ánh xạ lỗi | 5 | 20 | 110,0 | 110,0 | |
| E-08 Giao diện | 15 | 60 | 127,5 | 127,5 | |
| E-09 Kiểm thử, nghiệm thu, triển khai | 35 | 199 | 1.069,0 | 126,5 | 927,0 giờ test (30 issue, test của mọi epic còn lại), 15,5 giờ PM (#92). Có #89 và #118 sau khi tách |
| **Tổng** | **108** | **497** | **2.496,0** | **1.271,5** | |
| Dự phòng sửa lỗi 20% | | | **254,3** | | Mục 3.3 |
| **Tổng có dự phòng** | | | **2.750,3** | | |

Theo loại việc: lập trình 68 issue, 275 SWP, 1.298,5 giờ; thiết kế test 20 issue, 144 SWP, 774,5 giờ; chạy test 20 issue, 78 SWP, 423,0 giờ. Test chiếm 222 / 497 = 44,7% SWP.

**Issue lớn cần chú ý (trên 40 giờ):** 17 issue, lớn nhất là #81, #96, #97 (53,5 giờ), #118 (49,5 giờ), #19 (44 giờ). 84 trong 108 issue vượt 16 giờ (2 ngày) và 40 vượt 24 giờ. Nên một số issue còn thô so với tiêu chí "vài ngày"; đã ghi vào rủi ro ở `progress-report.md`.

---

## 5. So với estimate Lab 1

### 5.1 Phạm vi hai bản khác nhau (lý do chính của chênh lệch)

- Lab 1 ước từ đầu cho toàn hệ thống (khoảng 12.100 giờ, 145 tính năng). Lab 5 chỉ ước phần **thay đổi** cho ba màn của spec LAB-4, trên prototype đã chạy được. Chỉ một phần nhỏ của Lab 1 đối ứng được với Lab 5.
- Lab 1 có mà Lab 5 không tính giờ: thiết kế và tài liệu, quản lý dự án, hạ tầng, chuyển dữ liệu hệ thống cũ, đào tạo, vận hành sau triển khai, và các nhóm chức năng ngoài ba màn (lô, giao hàng, thông báo, báo cáo, xuất kế toán, chứng từ).
- Lab 5 có mà Lab 1 không có hoặc khác bản chất: bảng chợ nhiều chợ, chuyển 18 hồ sơ mẫu sang dòng căn cứ, chính sách theo hàng (RLS) ở tầng cơ sở dữ liệu, test tự động, rà soát mã. Các giả định liên quan nằm ở mục 2 của [PREMISE.md](PREMISE.md) (dòng 28 đến 60; G-27 ở dòng 60 nói về các issue bổ sung và khoản dự phòng sửa lỗi), cách chia loại ticket ở H-17 (dòng 86).

### 5.2 So theo epic (giờ lập trình, cùng cách tính: viết mã và test đơn vị của Lab 1)

| Phần | Lab 1 (giờ) | Lab 5 (giờ dev) | Chênh | Vì sao |
|---|---|---|---|---|
| E-01 Nền dữ liệu | 147,9 | 198,0 | +34% | Lab 5 thêm bảng chợ và chuyển hồ sơ mẫu; Lab 1 giả định một chợ |
| E-02 Phân quyền | 25,9 | 66,0 | +155% | Lab 1 chỉ có một dòng ma trận quyền ở tầng ứng dụng; Lab 5 làm RLS và thu quyền hàm ở tầng cơ sở dữ liệu. Đối ứng yếu |
| E-03 Tư cách tham gia | 175,1 | 170,5 | −3% | Cùng nội dung, khớp |
| E-04 Quy tắc theo phiên bản | 106,8 | 71,5 | −33% | Lab 1 chi tiết hơn |
| E-05 Khóa ngày, điều chỉnh | 259,4 | 236,5 | −9% | Lab 1 rộng hơn (có cả hủy giao dịch sau chốt) |
| E-06 Tiền thưởng thanh toán đúng hạn | 295,3 | 165,0 | −44% | Lab 1 chi tiết hơn nhiều về tính thưởng (công thức, thuế, giờ JST, delta kỳ sau). Có thể Lab 5 chấm nhẹ phần này |
| E-07 API | không có dòng riêng | 110,0 | | Lab 1 tính API trong dòng "Backend" từng tính năng, nên so E-01 đến E-07 cùng nhau |
| **E-01 đến E-07** | **1.010,4** | **1.017,5** | **+0,7%** | Gần nhau vì hệ số suy ra từ chính phần này |
| E-08 Giao diện | 127,1 | 127,5 | +0,3% | Như trên, giống nhau do cách làm. Ba màn Lab 5 gộp nhiều màn Lab 1 |
| E-09 việc khác (dữ liệu thử, migration, rà soát mã) | khoảng 128 (tham khảo) | 126,5 | | Độ tin cậy thấp; hai bên khác bản chất |

### 5.3 So kiểm thử

| Phần | Lab 1 (giờ) | Lab 5 (giờ) | Chênh |
|---|---|---|---|
| Test của E-06 (tiền thưởng) | 291,0 | 237,5 | −18% |
| Test của các epic còn lại | 866,0 | 960,0 | +11% |
| **Cộng** | **1.157,0** | **1.197,5** | **+3,5%** |

Điểm lệch cần ghi:
- **Tỷ lệ chạy / thiết kế bị ngược.** Lab 1: chạy 665 / thiết kế 492 giờ = 1,35. Lab 5: chạy 423,0 / thiết kế 774,5 giờ = 0,55. Có thể do Lab 5 có nhiều test tự động (chạy rẻ), cũng có thể do chấm thiết kế test hơi nặng. Chưa đủ dữ liệu để phân xử.
- **Test chồng lớp.** Mỗi màn vừa có bộ test thủ công vừa có test Playwright; tiền thưởng có bốn lớp (SQL, script API và vai, Playwright, thủ công). Lab 1 chỉ có một lớp thủ công. Mỗi lớp bắt lỗi khác nhau nên chưa nói là thừa, nhưng có nguy cơ chấm thừa. Lớp Playwright riêng khoảng 31 SWP, 166 giờ.
- Tỷ trọng test 44,7% SWP. Lab 1 khoảng 48% khi tính trên cùng mẫu số (viết mã + test đơn vị + kiểm thử), nên không lệch lớn.

### 5.4 So tổng

| | Giờ |
|---|---|
| Lab 1, phần đối ứng "cứng" (viết mã và test đơn vị 1.137,5 + kiểm thử 1.157,0 + việc khác tính riêng) | 2.523,9 (không gồm sửa lỗi) |
| Lab 1, sửa lỗi của phần đối ứng | 229,4 |
| Lab 5, chưa dự phòng | 2.496,0 |
| Lab 5, có dự phòng sửa lỗi 20% | 2.750,3 |

Hai con số này gần nhau là **do cách làm** (mục 3.4, điểm 1). Giá trị của bản so sánh là: epic nào Lab 5 chấm nặng hơn (E-01, E-02) hoặc nhẹ hơn (E-04, E-06) so với Lab 1, và các lệch về cấu trúc test ở trên. Toàn bộ Lab 1 còn khoảng 454 giờ thiết kế, hạ tầng và quản lý mà Lab 5 không tính, thuộc Lab 4 và phần quản lý ngoài backlog.
