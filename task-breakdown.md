# Task breakdown: Lab 5, Product A

> Mã `#số` là số issue trên Project; mã `DBn` là dòng số n của bảng đối chiếu cơ sở dữ liệu (db-diff) của bộ spec Lab 4. Epic ghi là `E-xx` kèm tên.

File này giải thích cách chia việc cho Product A (hệ thống hỗ trợ vận hành chợ bán buôn thủy sản của một khách hư cấu) từ bộ spec LAB-4. Spec trỏ bằng mã: `T-B002-nn`, `T-D001-nn`, `T-J002-nn` (172 việc của ba màn), `W-…` (39 việc nhóm theo kiến trúc, architecture mục 12), `A-…`, `Q-…`, `QD-…`, `QJ-…`, `ADR-001` đến `ADR-003`. Các thay đổi và giả định của Lab 5 nằm ở `PREMISE.md` (mã `S-xx`, `G-xx`, `H-xx`); spec LAB-4 chưa được sửa.

---

## 1. Nguyên tắc chia task

**Độ hạt.** Một issue là việc một người làm xong trong vài ngày và có tiêu chí xong kiểm được. Kết quả: 109 issue (kể cả 5 sub-issue) dưới 9 epic; trong đó 79 issue lấy từ spec, 5 issue bổ sung (#89 đến #92 và #118, trong đó #118 tách từ #89 sau khi tính giờ vì #89 quá lớn) và 25 issue test bổ sung (#93 đến #117, do tách test design và test execution thành hai ticket riêng và thêm bộ test case thủ công) không có mã `T-…`, `W-…`. Các mã `T-…` và `W-…` trùng nhau được gộp vào một issue (trung bình mỗi issue có 2 đến 3 mã); ví dụ cột chợ cho dữ liệu theo ngày gộp `W-DATA-01`, `T-D001-01`, `T-J002-01` vào #2, và trigger khóa ngày gộp `W-LOCK-02`, `T-D001-16`, `T-J002-18` vào #36 và #37. Điểm yếu đã biết: vài issue lớn (#23, #53, #59, #72, #77, #83, #93, #94) có thể vượt "vài ngày" nếu người làm chưa quen; khi đó tách theo từng mã `T-…` bên trong.

**Khi nào tách sub-issue.** Chỉ hai trường hợp: (a) tiền khuyến khích thanh toán đúng hạn (完納奨励金): hàm tính #44 tách thành 5 sub-issue theo 5 ràng buộc khác nhau của spec (#45 chọn phiên bản và lưu bản sao tham số; #46 công thức, thuế, cắt xuống JPY; #47 hạn, ngưỡng, thứ tự xét; #48 ngày ghi nhận và chặn khi ngày đã khóa; #49 audit và mã lỗi); (b) task quá lớn. Hiện chỉ có nhóm (a). Năm sub-issue do Lab 5 tự chia: `T-J002-14` là một dòng việc của spec, không phải năm mã riêng. Issue cha xong khi cả 5 sub-issue xong và hàm qua test của #59 (chạy ở #106).

**Định nghĩa xong (áp cho mọi issue).** (1) Việc mô tả chạy được. (2) Có test ở lớp thấp nhất chứng minh được (test SQL, script gọi từng vai, hoặc Playwright theo spec). (3) Không vi phạm quy tắc đã chốt: không ghi đè lịch sử, người lập khác người duyệt, không tự bịa số liệu. (4) Cập nhật tài liệu nếu đổi quyết định.

**Epic, issue, sub-issue trên GitHub Project.** Epic là issue cha gom các issue con theo nhóm việc; 9 epic không tính vào con số 109. Mỗi issue thuộc đúng một epic. Sub-issue (chỉ 5 cái, dưới #44) tính vào epic của issue cha. Phụ thuộc giữa các issue ghi bằng ID trong thân issue; đã kiểm bằng công cụ: không có vòng phụ thuộc và không có phụ thuộc vào ID không tồn tại. Issue `[TEST DESIGN]` không phụ thuộc issue dev nào (viết được ngay từ spec, song song với dev); phụ thuộc vào dev chỉ nằm ở issue `[TEST EXECUTION]`, kèm phụ thuộc vào design cặp. Story point và số giờ của từng issue ghi ở `estimate.md` và trên Project.

| Epic | Tên | Issue | Trong đó sub-issue |
|---|---|---|---|
| E-01 | Nền dữ liệu và chợ | 8 | 0 |
| E-02 | Phân quyền, RLS, tách người lập và người duyệt | 3 | 0 |
| E-03 | Tư cách tham gia hai căn cứ và kiểm khi chốt giao dịch | 7 | 0 |
| E-04 | Quy tắc theo chợ và phiên bản | 5 | 0 |
| E-05 | Khóa ngày và điều chỉnh sau khóa | 12 | 0 |
| E-06 | Tiền khuyến khích thanh toán đúng hạn (完納奨励金) | 19 | 5 |
| E-07 | API và ánh xạ lỗi | 5 | 0 |
| E-08 | Giao diện hồ sơ người tham gia và giao dịch (gồm cả màn khuyến khích J002) | 15 | 0 |
| E-09 | Kiểm thử, nghiệm thu và triển khai | 35 | 0 |
| | **Cộng** | **109** | **5** |

**Loại ticket và nhãn team.** Mỗi issue có nhãn loại `type:`: `type:devtask` (người phát triển làm, gồm dữ liệu thử dùng chung; kể cả việc quản lý và tài liệu không phải test), `type:testdesign` (viết test case, kịch bản, script test; làm được ngay từ spec, không chờ code), `type:testexecution` (chạy bộ test đã viết trên bản đã triển khai, đo, báo kết quả). Test design và test execution là hai ticket riêng đi cặp (mỗi loại 20 issue); không issue nào mang cả hai nhãn. Epic gắn `type:epic` và không gắn team. Loại `type:bug` có trong danh sách nhãn nhưng chưa có issue nào. Mỗi issue có đúng một nhãn team: `team:database` (bảng, hàm SQL, trigger, migration, dữ liệu thử), `team:backend` (API, quyền, RLS, ánh xạ lỗi), `team:frontend` (giao diện), `team:qa` (kiểm thử, nghiệm thu), `team:pm` (làm rõ với khách, tài liệu). Nhãn gắn bằng cách đọc từng issue, không theo tên epic. Số issue theo loại và theo team (đã đếm bằng công cụ từ backlog; cột Cộng theo team, hàng Cộng theo loại):

| Team | `type:devtask` | `type:testdesign` | `type:testexecution` | `type:testdesign` + `type:testexecution` | Cộng |
|---|---|---|---|---|---|
| `team:database` | 41 | 1 | 1 | 0 | **43** |
| `team:backend` | 11 | 0 | 0 | 0 | **11** |
| `team:frontend` | 15 | 0 | 0 | 0 | **15** |
| `team:qa` | 0 | 19 | 19 | 0 | **38** |
| `team:pm` | 2 | 0 | 0 | 0 | **2** |
| **Cộng** | **69** | **20** | **20** | **0** | **109** |

**Tiền tố tiêu đề.** `[DEV TASK]`, `[TEST DESIGN]`, `[TEST EXECUTION]`, `[EPIC]`. Sub-issue của #44 giữ cấp sub-issue và vẫn có tiền tố theo loại. Bảng 2.4 ghi tên việc không kèm tiền tố.

**Mẫu thân issue.** Khi nạp lên Project, thân mỗi issue theo mẫu: dòng "Ticket cha: <#số hoặc epic E-xx>"; "Bối cảnh"; "Đặc tả" (DEV TASK) hoặc "Việc cần làm" (issue test); "Tiêu chí xong" (danh sách checkbox); "Phụ thuộc"; "Nguồn spec" (mã `T-…`, `W-…`). DEV TASK thêm "Loại việc" (Dev - Database, Dev - Backend, Dev - Frontend; việc của `team:pm` ghi "Quản lý và tài liệu"). Issue test thêm "Phạm vi kiểm" và "Điều kiện bắt đầu".

**Sửa lỗi không có issue dựng sẵn.** Backlog không tạo trước issue sửa lỗi. Estimate có khoản dự phòng sửa lỗi bằng 20% số giờ implement (`PREMISE.md`, G-27; xem `estimate.md`, mục 3.3); trong sprint giả lập, lỗi phát sinh được ghi thành issue `[BUG FIX]` mang nhãn `type:bug` và tính vào khoản dự phòng. Lý do: số lỗi chưa biết trước, dựng sẵn issue lỗi là bịa tiến độ.

**Không đưa vào issue:** story point và số giờ; số liệu hoặc quy tắc khách chưa trả lời (ghi giả định có nhãn ở `PREMISE.md`); mật khẩu, khóa, dữ liệu thật; issue sửa lỗi dựng sẵn. **Nhãn nguồn của issue:** [spec] (74 issue), [suy luận] (31), [giả định] (3).

**Giữ nguyên các điểm dễ đọc nhầm.** Giao dịch thỏa thuận trực tiếp (相対取引) là luồng chính, đấu giá công khai (せり) là nhánh phụ cùng bảng giao dịch. Giấy phép tham gia (許可) và chấp thuận tham gia (承認) là hai cấp quyền không gộp. Việc kiểm hiệu lực lúc chốt do người duyệt quyết định; hệ thống không tự nhận diện ký hiệu tay (手やり) hay thẩm định chất lượng bằng kinh nghiệm (目利き).

---

## 2. Spec → issue

Mọi mã `T-…` (172) và `W-…` (39) có ít nhất một issue; đã kiểm bằng công cụ: không mã nào bị bỏ, không mã nào thừa so với spec. Một mã có thể nằm ở nhiều issue (ví dụ `T-J002-14` ở 6 issue vì hàm tính chia thành 5 sub-issue). Ba mươi mốt issue không có mã nào (#42, #89 đến #92, #118 và 25 issue test #93 đến #117) chỉ có ở bảng 2.4. Bảng 2.1 gom theo nhóm của spec; bảng 2.2 và 2.3 phủ đối chiếu cơ sở dữ liệu (db-diff) và ADR; bảng 2.4 liệt kê từng issue với mã nguồn, nên tra ngược được từ mọi mã.

### 2.1 Ba màn và architecture mục 12

| Spec | Epic | Issue/sub-issue | Phần spec chưa thành issue và vì sao |
|---|---|---|---|
| B002 dữ liệu, migration (T-B002-01 đến 07) | E-01, E-03 | #11, #12, #13, #22 | Không có mã bị bỏ. Việc bỏ cột tạm `change_reason` chờ người phụ trách dự án xác nhận (B002 mục 12.2 điểm 1) |
| B002 hàm cơ sở dữ liệu, kiểm trong hàm (T-B002-08 đến 11, 33) | E-03 | #21, #22, #23, #24 | Hàm đổi trạng thái căn cứ chỉ viết sẵn và thu quyền gọi (#24): chưa có màn và chưa biết ai xét duyệt lại (Q-12) |
| B002 API (T-B002-12 đến 16, 32) | E-07 | #64, #65 | Không có route xóa; không có `GET /api/participants/[id]` (PATCH gửi đủ bộ), theo B002 mục 12.2 điểm 12 |
| B002 giao diện (T-B002-17 đến 29, 34) | E-08 | #70, #71, #72, #73 | Không có màn đổi trạng thái căn cứ, không có giao diện đa chợ; cảnh báo hồ sơ sắp hết hạn (FR-PARTY-03) thuộc việc chưa làm của architecture mục 12.9 |
| B002 quyền (T-B002-30, 31) | E-02, E-08 | #18, #69 | Không có phần bị bỏ |
| B002 kiểm thử (T-B002-35 đến 46) | E-09 | #79, #80, #83, #84, #86 | Không có phần bị bỏ |
| D001 dữ liệu, audit (T-D001-01 đến 06) | E-01, E-02, E-03, E-09 | #2, #17, #19, #20, #27, #87 | Chi tiết audit duyệt giao dịch (mã phiên bản, bản chụp điều kiện đã áp) chờ xác nhận (D001 mục 12.2 điểm 4); làm theo spec (G-17) |
| D001 hàm cơ sở dữ liệu, khóa ngày trong hàm (T-D001-07 đến 16, 42) | E-03, E-05, E-09 | #25, #26, #27, #33, #36, #38, #39, #81 | Không có hành động hủy giao dịch (FR-AITAI-03): người phụ trách dự án đã bỏ (H-10) |
| D001 API (T-D001-17 đến 19, 40) | E-07 | #66, #67 | Không có route xóa giao dịch |
| D001 giao diện (T-D001-20 đến 37, 41) | E-08 | #74, #75, #76, #77, #78 | Không có nút hủy và không có cảnh báo lúc lập, nộp (đã bỏ); danh sách theo S-02 (phân trang) thay cho dòng chú thích 200 dòng |
| D001 quyền (T-D001-38, 39) | E-02, E-08 | #19, #69 | Hiển thị tên người lập, người duyệt ở D001 chưa làm (D001 mục 12.2 điểm 7, chờ xác nhận) |
| D001 kiểm thử (T-D001-43 đến 60) | E-05, E-09 | #43, #79, #80, #81, #83, #85, #86, #88 | T-D001-59 và T-D001-60 là việc thử và đo (#43, #88), không phải mã sản phẩm; kết quả quyết định việc tiếp theo |
| J002 dữ liệu, migration (T-J002-01 đến 09) | E-01, E-06 | #2, #14, #15, #48, #49, #59 | Không có phần bị bỏ |
| J002 phiên bản quy tắc: hàm, API (T-J002-10 đến 13, 20, 24 đến 27) | E-04, E-06, E-07 | #28, #29, #30, #31, #32, #53, #67 | Không có màn quản lý phiên bản (K001 đến K003, J003 ngoài phạm vi): lập và duyệt chỉ qua API hoặc SQL, người quản lý quy tắc thật chưa tự thao tác được (#32) |
| J002 tính, nhập ngày nhận tiền, tổng hợp (T-J002-14 đến 16, 18, 19, 21 đến 23, 28, 47, 48) | E-05, E-06 | #37, #44, #45, #46, #47, #48, #49, #50, #51, #52, #53 | Không có màn tạo chênh lệch (J004 ngoài phạm vi): chỉ hiển thị phần chênh lệch đã duyệt (#52) |
| J002 giao diện (T-J002-29 đến 44, 49) | E-08 | #54, #55, #56, #57, #58 | Dải kỳ tháng chỉ hiển thị chênh lệch đã duyệt; có gồm chênh lệch hay không còn chờ xác nhận (J002 mục 12.2 điểm 6) |
| J002 quyền (T-J002-17, 45, 46) | E-02, E-08 | #19, #69 | Không có phần bị bỏ |
| J002 kiểm thử (T-J002-50 đến 66) | E-06, E-09 | #59, #60, #61, #62, #82, #83 | Không có phần bị bỏ |
| architecture 12.1 và 12.2 (W-DATA-01 đến 06, W-AUTH-01 đến 03) | E-01, E-02, E-04, E-08 | #2, #11, #12, #13, #14, #15, #16, #17, #18, #19, #20, #30, #31, #69 | RLS theo chợ chưa làm (architecture 12.9, rủi ro R8) |
| architecture 12.3 và 12.4 (W-ELIG-01 đến 04, W-RULE-01 đến 04) | E-03, E-04 | #21, #22, #23, #24, #26, #27, #28, #29, #30, #32 | W-ELIG-04 chỉ làm phần viết sẵn (chờ Q-12); W-RULE-04 chưa có màn |
| architecture 12.5 (W-LOCK-01 đến 06) | E-05 | #33, #34, #35, #36, #37, #38, #39, #40, #41, #43 | Không có màn khóa ngày, màn điều chỉnh (J004), vai trưởng bộ phận đối soát, hàm mở lại `day_reopen`: spec chưa thiết kế hoặc ngoài phạm vi (architecture mục 7.6, ADR-003 mục 5.1) |
| architecture 12.6 (W-INC-01 đến 07) | E-06, E-08 | #45, #48, #50, #52, #57, #59, #63 | W-INC-07 là câu hỏi mở (Q-07 đến Q-11), nên #63 là việc làm rõ với khách, không phải mã |
| architecture 12.7 (W-API-01 đến 03) | E-04, E-05, E-07 | #32, #38, #41, #65, #67, #68 | Không có phần bị bỏ; W-API-03 sửa theo S-04 |
| architecture 12.8 (W-TEST-01 đến 06) | E-06, E-09 | #59, #79, #80, #81, #82, #83, #85 | W-TEST-05 chỉ kiểm phía ghi audit; chưa có tra cứu audit (phần 4, dòng 17) |
| Việc bổ sung: triển khai, nghiệm thu, rà soát, tài liệu vận hành | E-09 | #89, #90, #91, #92, #118 | Spec LAB-4 không có mục này; Lab 5 thêm theo cách một dự án thật phân loại ticket, nhãn [suy luận] (`PREMISE.md`, G-27). Phạm vi nghiệm thu với khách là giả định; sửa lỗi sau nghiệm thu đi qua issue `[BUG FIX]` phát sinh, không dựng sẵn |
| Việc bổ sung: test case thủ công, tách test design và test execution | E-05, E-06, E-09 | #93 đến #117 | Spec LAB-4 không có mục này; Lab 5 thêm theo nhận xét của người phụ trách dự án là còn thiếu task viết test case (`PREMISE.md`, G-27). Mỗi AC của B002, D001, J002 thuộc đúng một issue viết test case (#93 đến #97); nhãn [spec] hoặc [suy luận]. Test design không phụ thuộc issue dev; phụ thuộc vào dev nằm ở issue chạy test |
| architecture mục 12.9 (việc có trong RFP nhưng chưa thiết kế) | không có | không có | Toàn bộ mục: hủy giao dịch trước khi khóa (FR-AITAI-03), nhánh yêu cầu sửa và chốt lại (FIG-012), xem lại và sửa kết quả đấu giá công khai (FR-SERI-03), cảnh báo hồ sơ sắp hết hạn (FR-PARTY-03), MFA, thông báo, xuất kế toán, tệp đính kèm, báo cáo cố định, lưu trữ lạnh, RLS theo chợ. Spec ghi "chưa thiết kế" nên không có issue và không viết như đã đáp ứng |

### 2.2 Đối chiếu cơ sở dữ liệu (db-diff, 93 dòng)

Mỗi nhóm dòng của db-diff trỏ tới issue làm việc đó. 31 dòng "Trùng khớp" giữa thiết kế và prototype không có việc.

| Spec | Epic | Issue/sub-issue | Phần spec chưa thành issue và vì sao |
|---|---|---|---|
| db-diff nhóm A: người tham gia và căn cứ (DB1 đến DB18) | E-01, E-02, E-03 | #11, #12, #13, #18, #21, #22, #23, #24, #26 | 4 dòng Trùng khớp (DB9, DB12, DB17, DB18) không có việc; DB11 (giá trị trạng thái `not_started`) chờ chốt (B002 mục 12.2 điểm 5), #12 làm theo B002 mục 4.2 |
| db-diff nhóm B: vai, quyền, RLS (DB19 đến DB32) | E-02, E-08 | #18, #19, #20, #69 | 10 dòng Trùng khớp không có việc, gồm DB32 (RLS theo chợ: hai bên cùng chưa làm vì chỉ có một chợ) |
| db-diff nhóm C: lô (DB33 đến DB41) | E-01, E-05 | #2, #36, #39, #43 | 6 dòng Trùng khớp (DB35, DB36, DB38 đến DB41) không có việc riêng; hệ quả của DB35 (tồn kho sau khóa) được quyết ở #43 phần b |
| db-diff nhóm D: giao dịch và hàm `trade_*` (DB42 đến DB56) | E-01, E-03, E-05, E-07 | #2, #25, #26, #27, #33, #36, #38, #39, #67, #68 | 4 dòng Trùng khớp (DB43, DB44, DB53, DB54) không có việc; DB55 (thêm `market_id` vào view tìm kiếm của màn D002) chưa làm vì D002 nằm ngoài ba màn |
| db-diff nhóm E: thanh toán, kết quả khuyến khích, quy tắc có phiên bản (DB57 đến DB77) | E-01, E-04, E-05, E-06 | #2, #14, #15, #16, #28, #29, #30, #31, #37, #45, #46, #47, #48, #49, #50, #51, #59 | 3 dòng Trùng khớp (DB73, DB74, DB75) không có việc; công thức vẫn là giả định (G-10); DB77 (dữ liệu mẫu do hai người lập và duyệt) thuộc #15 |
| db-diff nhóm F: chợ (DB78) | E-01 | #2 | Không có phần bị bỏ |
| db-diff nhóm G: khóa ngày, điều chỉnh (DB79 đến DB85) | E-01, E-05 | #16, #17, #33, #34, #35, #36, #37, #40, #43 | Không có phần bị bỏ; DB85 (xóa tài khoản nhân viên có bị trigger chặn không) là việc thử ở #17 và #51 |
| db-diff nhóm H: audit (DB86 đến DB90) | E-01, E-05, E-06, E-09 | #17, #27, #43, #49, #83 | 1 dòng Trùng khớp (DB89) không có việc; DB90 (log của lần ghi bị chặn) là việc thử ở #43 phần a |
| db-diff nhóm I: quy ước chung (DB91 đến DB93) | E-01, E-02, E-05 | #11, #14, #16, #18, #19, #36 | 2 dòng Trùng khớp (DB92, DB93) không có việc riêng: là quy ước áp cho bảng và hàm mới; DB93 (chặn xóa) kiểm ở #36 |

### 2.3 ADR

| Spec | Epic | Issue/sub-issue | Phần spec chưa thành issue và vì sao |
|---|---|---|---|
| ADR-001: hai căn cứ tham gia, không gộp | E-01, E-03 | #11, #12, #13, #21, #22, #23, #24, #26 | Quy trình cấp, gia hạn, gỡ tạm dừng (mục 4.2 điểm 9) và lịch sử cấp có cấu trúc (điểm 4): spec chỉ chừa hàm và dữ liệu, nên không có issue; điền bù khi Q-05 trả lời "có" (điểm 8) làm khi có đáp án |
| ADR-002: quy tắc theo chợ và phiên bản | E-01, E-04, E-06 | #2, #14, #15, #28, #29, #30, #31, #32, #45, #51 | Màn quản lý quy tắc (K001 đến K003, J003) không có; chỉ loại `incentive_rate` có cột chi tiết, ba loại còn lại (`participation_requirement`, `payment_term`, `business_day_definition`) chỉ nêu hoặc chừa chỗ (mục 2.1) |
| ADR-003: khóa ngày, cưỡng chế ở cơ sở dữ liệu | E-01, E-05 | #16, #17, #33, #34, #35, #36, #37, #38, #39, #40, #41, #42, #43 | Màn khóa ngày, màn điều chỉnh (J004), vai trưởng bộ phận đối soát, hàm mở lại: ngoài phạm vi. Ảnh chụp tồn kho lúc khóa chỉ có quyết định ở #43 phần b. Trả tồn kho khi đảo giao dịch (mục 4.2 điểm 7) chờ Q-16 |

### 2.4 Bảng tra ngược: từng issue và mã nguồn

| Issue | Epic | Cấp | Tiêu đề | Mã `T-…` và `W-…` chứa |
|---|---|---|---|---|
| #2 | E-01 | issue | Bảng chợ và cột chợ cho dữ liệu theo ngày | T-D001-01; T-J002-01; W-DATA-01 |
| #11 | E-01 | issue | Bảng căn cứ tham gia và bảng yêu cầu căn cứ theo loại | T-B002-01, 02; W-DATA-02 |
| #12 | E-01 | issue | Chuyển 18 hồ sơ mẫu sang dòng căn cứ và dựng view hiệu lực mới | T-B002-03, 06, 07; W-DATA-02 |
| #13 | E-01 | issue | Bỏ cột và view cũ của hồ sơ người tham gia | T-B002-04; W-DATA-02 |
| #14 | E-01 | issue | Khung phiên bản quy tắc: bảng, enum, ràng buộc | T-J002-02; W-DATA-03 |
| #15 | E-01 | issue | Chép phiên bản tỷ lệ cũ sang khung mới và bỏ bảng cũ | T-J002-03, 04, 06, 08; W-DATA-03 |
| #16 | E-01 | issue | Bảng khóa ngày, lịch chợ và điều chỉnh sau khóa | W-DATA-04, 05 |
| #17 | E-01 | issue | Mở rộng bảng audit: cột, chỉ mục, chỉ thêm | T-D001-03; W-DATA-06 |
| #18 | E-02 | issue | RLS và quyền cho bảng căn cứ; bỏ ghi trực tiếp hồ sơ | T-B002-30; W-AUTH-01 |
| #19 | E-02 | issue | RLS cho bảng quy tắc, khóa ngày, điều chỉnh; hàm mở tên người; thu quyền hàm nội bộ | T-D001-04, 38; T-J002-17, 45; W-AUTH-01 |
| #20 | E-02 | issue | Check người lập khác người duyệt ở bảng giao dịch | T-D001-02; W-AUTH-02 |
| #21 | E-03 | issue | Hàm kiểm hiệu lực theo căn cứ `participant_basis_ok` | T-B002-09; W-ELIG-01 |
| #22 | E-03 | issue | Trigger bảo vệ và audit cho hồ sơ và căn cứ | T-B002-05, 11; W-ELIG-03 |
| #23 | E-03 | issue | Hàm `participant_save`: ghi hồ sơ và căn cứ trong một giao dịch | T-B002-08, 33; W-ELIG-03 |
| #24 | E-03 | issue | Hàm đổi trạng thái căn cứ (bản chừa) | T-B002-10; W-ELIG-04 |
| #25 | E-03 | issue | `trade_transition`: dựng lại khung xử lý | T-D001-11 |
| #26 | E-03 | issue | Duyệt giao dịch dùng `participant_basis_ok` hai phía; gộp logic hạn mức | T-D001-10, 12; W-ELIG-02 |
| #27 | E-03 | issue | Mở rộng audit giao dịch và audit duyệt | T-D001-06, 14; W-RULE-02 |
| #28 | E-04 | issue | Hàm chọn phiên bản theo ngày `rule_version_at` | T-J002-10; W-RULE-02 |
| #29 | E-04 | issue | Hàm lập nháp và hoàn tác phiên bản | T-J002-11; W-RULE-01, 03 |
| #30 | E-04 | issue | Hàm duyệt và từ chối phiên bản | T-J002-12, 13; W-AUTH-02; W-RULE-01, 03 |
| #31 | E-04 | issue | Trigger bất biến cho phiên bản đã duyệt | T-J002-20; W-AUTH-02 |
| #32 | E-04 | issue | API phiên bản quy tắc (đọc, lập, duyệt, từ chối) | T-J002-24 đến 27; W-API-01; W-RULE-04 |
| #33 | E-05 | issue | Hàm `day_is_locked` và khóa tư vấn chia sẻ theo chợ và ngày | T-D001-15; W-LOCK-01 |
| #34 | E-05 | issue | Hàm lập bảng đối soát ngày và đánh dấu đã kiểm tra | W-LOCK-01 |
| #35 | E-05 | issue | Hàm `day_lock` và chặn mở lại ngày | W-LOCK-01, 05 |
| #36 | E-05 | issue | Trigger khóa ngày trên `trades` và `lots` | T-D001-16; W-LOCK-02 |
| #37 | E-05 | issue | Trigger khóa ngày trên `payments`, `incentive_results`, `adjustment_entries` | T-J002-18; W-LOCK-02 |
| #38 | E-05 | issue | `trade_create` và `trade_update_draft`: chợ, khóa ngày, kiểm đầu vào | T-D001-08, 09, 42; W-API-03; W-LOCK-03 |
| #39 | E-05 | issue | Nhánh ngày đã khóa cho `trade_transition`, `seri_record`, `lots_guard` | T-D001-13; W-LOCK-03 |
| #40 | E-05 | issue | Hàm đề nghị, duyệt, từ chối điều chỉnh sau khóa | W-LOCK-06 |
| #41 | E-05 | issue | Route khóa ngày và điều chỉnh | W-API-01; W-LOCK-04 |
| #42 | E-05 | issue | Làm rõ đường nhập muộn ngày nhận tiền sau khi khóa | không có mã (sinh ra khi break, xem phần 4 dòng 9) |
| #43 | E-05 | issue | Kiểm chứng hai giả định của thiết kế khóa ngày | T-D001-59; W-LOCK-02 |
| #44 | E-06 | issue | Tính tiền khuyến khích theo phiên bản (`incentive_calculate`) | T-J002-14 |
| #45 | E-06 | sub-issue của #44 | Chọn phiên bản theo ngày và lưu bản sao tham số | T-J002-14; W-INC-01 |
| #46 | E-06 | sub-issue của #44 | Công thức tiền khuyến khích: hệ số thuế và cắt xuống JPY | T-J002-14 |
| #47 | E-06 | sub-issue của #44 | Hạn thanh toán, ngưỡng tối thiểu và thứ tự xét | T-J002-14 |
| #48 | E-06 | sub-issue của #44 | Ngày ghi nhận, số ngày hạn lưu trong kết quả và chặn khi ngày đã khóa | T-J002-05, 14; W-INC-02 |
| #49 | E-06 | sub-issue của #44 | Audit và mã lỗi của tính tiền, nhập ngày nhận tiền, phiên bản | T-J002-07, 14, 48 |
| #50 | E-06 | issue | Nhập ngày nhận tiền (`payment_record`) theo ngày sở hữu | T-J002-15; W-INC-03 |
| #51 | E-06 | issue | Kết quả khuyến khích không ghi đè và không sửa | T-J002-19 |
| #52 | E-06 | issue | Hàm tổng hợp ngày và chênh lệch kỳ sau | T-J002-16; W-INC-04, 05 |
| #53 | E-06 | issue | API khuyến khích: đọc, tính, nhập ngày nhận tiền | T-J002-21 đến 23, 27, 28, 47 |
| #54 | E-08 | issue | Màn khuyến khích: khung trang, ô ngày, dải khóa và kỳ tháng | T-J002-29 đến 31, 37 |
| #55 | E-08 | issue | Màn khuyến khích: bảng kết quả, chi tiết, phân trang | T-J002-32, 36, 41, 42 |
| #56 | E-08 | issue | Màn khuyến khích: nhập ngày nhận tiền và nút tính | T-J002-33, 34 |
| #57 | E-08 | issue | Màn khuyến khích: bảng phiên bản và chế độ theo vai | T-J002-35, 40; W-INC-04 |
| #58 | E-08 | issue | Màn khuyến khích: trạng thái, nhãn lỗi, tablet, bàn phím | T-J002-38, 39, 43, 44, 49 |
| #59 | E-06 | issue | Dữ liệu thử và test SQL phép tính khuyến khích | T-J002-09, 50, 51; W-INC-06; W-TEST-04 |
| #60 | E-06 | issue | Test SQL: khóa ngày, nhập ngày nhận tiền, kết quả chỉ thêm, migration | T-J002-52, 53, 56, 59 |
| #61 | E-06 | issue | Script API, script vai và viết lại test của màn khuyến khích | T-J002-57, 58, 65 |
| #62 | E-06 | issue | Playwright cho màn khuyến khích | T-J002-60 đến 64, 66 |
| #63 | E-06 | issue | Chốt hoặc ghi giả định cho câu hỏi Q-07 đến Q-11 với khách | W-INC-07 |
| #64 | E-07 | issue | API đọc hồ sơ người tham gia | T-B002-12 |
| #65 | E-07 | issue | API tạo và sửa hồ sơ, bộ đọc đầu vào, bảng ánh xạ lỗi | T-B002-13 đến 16, 32; W-API-02 |
| #66 | E-07 | issue | API giao dịch: tạo, sửa nháp, chuyển trạng thái | T-D001-18, 19, 40 |
| #67 | E-07 | issue | Ánh xạ lỗi `day_locked`, `self_approval`, `invalid_business_day_future` | T-D001-17; T-J002-27; W-API-02 |
| #68 | E-07 | issue | Route hiện có nhận tham số chợ (mặc định chợ duy nhất) | W-API-03 |
| #69 | E-08 | issue | Helper vai dùng chung và ghi chú ánh xạ vai | T-B002-31; T-D001-39; T-J002-46; W-AUTH-03 |
| #70 | E-08 | issue | Màn hồ sơ: khung, bảng danh sách, bộ lọc, trạng thái | T-B002-17 đến 20 |
| #71 | E-08 | issue | Màn hồ sơ: panel tạo, sửa và vùng căn cứ | T-B002-21 đến 23 |
| #72 | E-08 | issue | Màn hồ sơ: lưu, kiểm tra, thành công, xung đột | T-B002-24 đến 26, 34 |
| #73 | E-08 | issue | Màn hồ sơ: liên kết theo mã, tablet, bàn phím | T-B002-27 đến 29 |
| #74 | E-08 | issue | Màn giao dịch: khung, bảng 11 cột, tab trạng thái | T-D001-21 đến 23, 30, 37 |
| #75 | E-08 | issue | Màn giao dịch: phân trang, không giới hạn cứng 200 dòng | T-D001-36 |
| #76 | E-08 | issue | Màn giao dịch: form tạo và sửa nháp | T-D001-24, 25, 41 |
| #77 | E-08 | issue | Màn giao dịch: thao tác theo vai, hộp thoại duyệt và từ chối, lý do | T-D001-20, 26 đến 28, 32 |
| #78 | E-08 | issue | Màn giao dịch: ngày đã khóa, xung đột, thành công, tablet, bàn phím | T-D001-29, 31, 33 đến 35 |
| #79 | E-09 | issue | Ma trận khóa ngày | T-B002-46; T-D001-46; W-TEST-01 |
| #80 | E-09 | issue | Ma trận tư cách tham gia | T-B002-35 đến 37; T-D001-48; W-TEST-03 |
| #81 | E-09 | issue | Test SQL hàm giao dịch | T-D001-07, 43 đến 45, 47; W-TEST-02 |
| #82 | E-09 | issue | Test người lập khác người duyệt ở phiên bản và điều chỉnh | T-J002-54; W-TEST-02 |
| #83 | E-09 | issue | Test audit chỉ thêm, script vai và script API của hồ sơ, giao dịch, khuyến khích | T-B002-38 đến 40; T-D001-49 đến 51; T-J002-55; W-TEST-05 |
| #84 | E-09 | issue | Playwright màn hồ sơ người tham gia | T-B002-41 đến 43 |
| #85 | E-09 | issue | Playwright luồng giao dịch thỏa thuận trực tiếp và nhánh đấu giá công khai | T-D001-52 đến 55; W-TEST-06 |
| #86 | E-09 | issue | Viết lại test cũ và hồi quy các màn liền kề | T-B002-44, 45; T-D001-56 đến 58 |
| #87 | E-09 | issue | Dữ liệu thử dùng chung cho hồ sơ và giao dịch | T-D001-05 |
| #88 | E-09 | issue | Đo thời gian đọc danh sách và duyệt ở cỡ tải tham chiếu | T-D001-60 |
| #89 | E-09 | issue | Lập thứ tự migration, kế hoạch sao lưu và hoàn tác cho môi trường chạy thật | không có mã (việc bổ sung, xem phần 1) |
| #90 | E-09 | issue | Hỗ trợ nghiệm thu với khách và sửa lỗi sau nghiệm thu (UAT) [phạm vi giả định] | không có mã (việc bổ sung, xem phần 1) |
| #91 | E-09 | issue | Review mã và rà soát cuối sprint | không có mã (việc bổ sung, xem phần 1) |
| #92 | E-09 | issue | Cập nhật tài liệu vận hành và hướng dẫn chạy | không có mã (việc bổ sung, xem phần 1) |
| #93 | E-09 | issue | Bộ test case thủ công màn hồ sơ người tham gia (B002) | không có mã (test bổ sung, xem phần 1) |
| #94 | E-09 | issue | Bộ test case thủ công màn giao dịch thỏa thuận trực tiếp (D001) | không có mã (test bổ sung, xem phần 1) |
| #95 | E-09 | issue | Bộ test case thủ công màn kết quả tiền khuyến khích (J002) | không có mã (test bổ sung, xem phần 1) |
| #96 | E-09 | issue | Bộ test case thủ công nghiệp vụ tiền khuyến khích thanh toán đúng hạn (完納奨励金) | không có mã (test bổ sung, xem phần 1) |
| #97 | E-09 | issue | Bộ test case thủ công nghiệp vụ khóa ngày, điều chỉnh sau khóa và tư cách tham gia hai căn cứ | không có mã (test bổ sung, xem phần 1) |
| #98 | E-09 | issue | Chạy bộ test case thủ công màn hồ sơ người tham gia (B002) | không có mã (test bổ sung, xem phần 1) |
| #99 | E-09 | issue | Chạy bộ test case thủ công màn giao dịch thỏa thuận trực tiếp (D001) | không có mã (test bổ sung, xem phần 1) |
| #100 | E-09 | issue | Chạy bộ test case thủ công màn kết quả tiền khuyến khích (J002) | không có mã (test bổ sung, xem phần 1) |
| #101 | E-09 | issue | Chạy bộ test case thủ công nghiệp vụ tiền khuyến khích thanh toán đúng hạn (完納奨励金) | không có mã (test bổ sung, xem phần 1) |
| #102 | E-09 | issue | Chạy bộ test case thủ công nghiệp vụ khóa ngày, điều chỉnh sau khóa và tư cách tham gia hai căn cứ | không có mã (test bổ sung, xem phần 1) |
| #103 | E-05 | issue | Kịch bản thử hai giả định của thiết kế khóa ngày | không có mã (test bổ sung, xem phần 1) |
| #104 | E-09 | issue | Kịch bản đo thời gian đọc danh sách và duyệt ở cỡ tải tham chiếu | không có mã (test bổ sung, xem phần 1) |
| #105 | E-09 | issue | Kịch bản nghiệm thu với khách [phạm vi giả định] | không có mã (test bổ sung, xem phần 1) |
| #106 | E-06 | issue | Chạy test SQL phép tính khuyến khích | không có mã (test bổ sung, xem phần 1) |
| #107 | E-06 | issue | Chạy test SQL: khóa ngày, nhập ngày nhận tiền, kết quả chỉ thêm, migration | không có mã (test bổ sung, xem phần 1) |
| #108 | E-06 | issue | Chạy script API, script vai và test viết lại của màn khuyến khích | không có mã (test bổ sung, xem phần 1) |
| #109 | E-06 | issue | Chạy Playwright cho màn khuyến khích | không có mã (test bổ sung, xem phần 1) |
| #110 | E-09 | issue | Chạy ma trận khóa ngày | không có mã (test bổ sung, xem phần 1) |
| #111 | E-09 | issue | Chạy ma trận tư cách tham gia | không có mã (test bổ sung, xem phần 1) |
| #112 | E-09 | issue | Chạy test SQL hàm giao dịch | không có mã (test bổ sung, xem phần 1) |
| #113 | E-09 | issue | Chạy test người lập khác người duyệt ở phiên bản và điều chỉnh | không có mã (test bổ sung, xem phần 1) |
| #114 | E-09 | issue | Chạy test audit chỉ thêm, script vai và script API của hồ sơ, giao dịch, khuyến khích | không có mã (test bổ sung, xem phần 1) |
| #115 | E-09 | issue | Chạy Playwright màn hồ sơ người tham gia | không có mã (test bổ sung, xem phần 1) |
| #116 | E-09 | issue | Chạy Playwright luồng giao dịch thỏa thuận trực tiếp và nhánh đấu giá công khai | không có mã (test bổ sung, xem phần 1) |
| #117 | E-09 | issue | Chạy test cũ đã viết lại và hồi quy các màn liền kề | không có mã (test bổ sung, xem phần 1) |
| #118 | E-09 | issue | Chạy migration trên môi trường chạy thật và kiểm tra sau triển khai (tách từ #89) | không có mã (việc bổ sung, xem phần 1) |

---

## 3. Hạng mục chịu ràng buộc đặc thù của đề

Đề Product A đòi tiền khuyến khích thanh toán đúng hạn (完納奨励金) phải là task riêng, không gộp vào việc khác. Nhóm này có epic E-06 (19 issue: 5 sub-issue của #44 và 4 issue chạy test #106 đến #109) và 5 issue giao diện #54 đến #58 nằm ở E-08 vì mọi giao diện gom vào một epic (E-08 có 15 issue). Các số 0,5%, 0,6%, ngưỡng 100.000 JPY, hạn 3 ngày là giả định (`PREMISE.md` dòng 34 đến 36, G-01 đến G-03), chỉ dùng làm dữ liệu thử, không phải số của khách.

| Ràng buộc | Issue | Vì sao tách riêng |
|---|---|---|
| Khung hàm tính (cha của 5 sub-issue); bộ phận đối soát bấm chạy, không tự động | #44 | Gom 5 ràng buộc bên dưới thành một hàm chạy trọn vẹn; xong khi 5 sub-issue xong và qua test #59 (viết) và #106 (chạy) (G-11) |
| Công thức: hệ số thuế 110/100 và cắt xuống theo đơn vị JPY, cắt đúng một lần ở cuối (BR-INC-01, ca INC-EDGE-03) | #46 (sub-issue của #44) | Sai một chỗ làm tròn là sai tiền. Có ca thử riêng: 100.580 JPY ra 553, còn cắt trước thuế sẽ ra 552 (J002 mục 3.5 ca j). Phải dùng số thập phân chính xác, không dùng số thực |
| Chọn phiên bản theo ngày hiệu lực, lưu bản sao tham số, không tính lại (FR-INC-01, ca INC-EDGE-01) | #45 (sub-issue của #44); thử ở #59 | Quyết định trong lịch sử không bị đổi; giả định về ngày dùng để chọn phiên bản (QJ-03, G-08) ảnh hưởng mọi kết quả |
| Hạn thanh toán, không ân hạn, ngưỡng tối thiểu, thứ tự xét (quá hạn trước, rồi ngưỡng) | #47 (sub-issue của #44) | Nhiều ca biên (ngày JST so với UTC, bằng đúng ngưỡng); phụ thuộc giả định N-02, P-02 (G-02, G-10) và câu hỏi Q-08, Q-09 |
| Ngày nhận tiền do người nhập; ngày sở hữu theo `received_at` (N-03) | #50 | Là điểm vào dữ liệu của tính tiền; ngày sở hữu quyết định việc khóa ngày có chặn hay không (S-01, G-12) |
| Kết quả không ghi đè, không sửa (FR-INC-01) | #51 | Ràng buộc ở cơ sở dữ liệu (trigger) khác với phần tính; có ngoại lệ khi xóa tài khoản nhân viên cần thử thật; chờ xác nhận (G-17) |
| Vòng đời phiên bản: người lập khác người duyệt, ngày hiệu lực, hoàn tác, đổi tối đa 4 lần mỗi năm (GOV-RULE-01, FR-INC-02) | #29, #30, #31, #32 (E-04) | Nằm ở E-04 vì dùng chung cho mọi loại quy tắc; tiền khuyến khích là loại đầu tiên dùng (H-05) |
| Khóa ngày: kết quả thuộc ngày chạy tính, thanh toán thuộc ngày nhận tiền | #48 (sub-issue của #44), #37 | Cột mới, điền bù và chặn khi khóa là phần migration nhiều rủi ro nhất của nhóm (H-04) |
| Audit: ai tính, lúc nào, bằng phiên bản nào (FR-AUDIT-03) | #49 (sub-issue của #44) | Cần ba cột audit mới và mã phiên bản; tách lỗi ngày tương lai khỏi lỗi định dạng ngày |
| Chênh lệch kỳ sau cho điều chỉnh sau khóa (FR-INC-03) | #52, #40 | Không có màn tạo chênh lệch; chỉ hiển thị phần đã duyệt; kỳ là tháng (G-04) |
| API đọc, tính, nhập ngày nhận tiền | #53 | Hợp đồng API đổi (khóa mới, mã lỗi mới), cần test từng mã |
| Giao diện màn khuyến khích (khung, kết quả, nhập ngày nhận tiền, phiên bản, trạng thái) | #54, #55, #56, #57, #58 (E-08) | Năm khối giao diện độc lập, mỗi khối có tiêu chí riêng; khung cảnh báo "giả định" bắt buộc |
| Kiểm thử: ca biên, khóa ngày, API và vai, Playwright | #59, #60, #61, #62 (viết); #106, #107, #108, #109 (chạy) | Bộ ca biên của RFP chỉ có lời mô tả, không có số; test kiểm theo cách hiểu của spec, không chứng minh khớp bảng tỷ lệ của khách (G-26) |
| Câu hỏi mở Q-07 đến Q-11 (nghĩa của (交付率), ngưỡng là tiền hay tỷ lệ, nguồn ngày nhận tiền, nghĩa của (完納), có áp cho đấu giá công khai không) | #63 | Việc làm rõ với khách, không phải mã; cần xong trước khi coi nhóm là xong (W-INC-07) |

---

## 4. Chỗ hở hoặc mâu thuẫn của spec LAB-4 phát hiện khi break

Mỗi dòng đã được mở lại bằng chứng ở spec trước khi ghi. Cột "Xử lý" dùng đúng một trong bốn giá trị: đã sửa thiết kế (trỏ dòng `PREMISE.md`); giả định (trỏ dòng `PREMISE.md`); tạo issue làm rõ (#số); để mở (kèm lý do). "Đã sửa thiết kế" ở đây là đề xuất của Lab 5, spec LAB-4 chưa sửa. Số dòng `PREMISE.md` tính theo bản hiện tại của file đó.

| Spec (bằng chứng: file, mục) | Chỗ hở/mâu thuẫn | Xử lý |
|---|---|---|
| 1. D001 mục 2.7 điểm 3, 12.1 (QD-02), 12.2 điểm 5, AC-D001-51; tiền đề LAB-4 mục 5.11 điểm 2; architecture mục 7.3 | Hủy giao dịch đã bị bỏ nên nháp bỏ không dùng chỉ thoát bằng cách nộp rồi nhờ người duyệt từ chối; trong lúc đó ngày không khóa được vì còn nháp | để mở (người phụ trách dự án đã chấp nhận rủi ro; hỏi khách ở QD-02; ca nháp tồn đọng chặn khóa có ở #79; `PREMISE.md` dòng 57, G-24) |
| 2. ADR-003 mục 4.2 điểm 3; db-diff DB35 | Sau khi khóa, tồn kho lô (`available_kg`, `status`) vẫn đổi theo giao dịch của ngày mở nên không tái lập được tồn kho cuối ngày từ bảng `lots`; chưa có ảnh chụp lúc khóa | tạo issue làm rõ (#43 phần b: quyết định lưu ảnh chụp hay dựa vào audit) |
| 3. ADR-003 mục 1.2 điểm 5 và 4.2 điểm 2; db-diff DB90 | Trigger chặn bằng cách ném lỗi nên có thể làm mất luôn dòng audit của lần thử bị chặn, trong khi RFP đòi mọi lần thử sửa ngày đã khóa đều để lại log (FR-CORR-03); test W-TEST-05 không đạt được cho lần bị trigger chặn | tạo issue làm rõ (#43 phần a: thử thật rồi chọn log độc lập giao dịch hoặc chấp nhận log máy chủ; #83 chưa đòi audit cho lần bị trigger chặn) |
| 4. Tiền đề LAB-4 mục 4.1 (A-06) và 5.11; ADR-003 mục 4.2 điểm 11 | Kỳ của tiền khuyến khích là tháng chỉ là giả định (Q-03 chưa có đáp án); đổi kỳ thì bản ghi điều chỉnh đã tạo phải xử lý lại | giả định (`PREMISE.md` dòng 37, G-04) |
| 5. ADR-003 mục 4.2 điểm 9; architecture mục 7.4 | Ngày ghi nhận của điều chỉnh là "ngày đang mở"; nếu hôm nay đã khóa thì không có ngày nào đang mở. Architecture không nói chọn ngày nào; ADR-003 chỉ nêu hệ quả "phải chờ ngày kế" và gắn nhãn suy luận | giả định (`PREMISE.md` dòng 46, G-13) |
| 6. J002 mục 3.5 và 12.1 (Q-07 đến Q-11); ADR-002 mục 4.2 điểm 10 | Tỷ lệ 0,5% và 0,6%, ngưỡng 100.000 JPY, hạn 3 ngày là giả định của prototype; RFP không đưa số nên không có đáp số của khách để so | giả định (`PREMISE.md` dòng 34 đến 36, G-01 đến G-03; giữ khung cảnh báo ở #54; hỏi khách ở #63) |
| 7. Tiền đề LAB-4 phần đầu (nhãn nguồn) và mục 7 | 47 câu Q&A chưa có đáp án nên nhiều quyết định chạy bằng giả định (kỳ, khóa kỳ, mở lại, người duyệt, loại bên mua) | để mở (cần khách trả lời; chỉ Q-07 đến Q-11 có issue làm rõ là #63; nên gửi gói câu hỏi trước khi làm E-05 và E-06; `PREMISE.md` dòng 38, G-05) |
| 8. db-diff phần dưới bảng mục 7; db-diagram mục 7 | Tên cột, khóa chính, enum của bảng mới là suy luận; ví dụ khóa ngoại sang phiên bản từng có hai tên | giả định (`PREMISE.md` dòng 39, G-06; riêng tên khóa ngoại đã chốt ở dòng 84, H-15) |
| 9. ADR-003 mục 2.4, 2.5 và 4.2 điểm 5; architecture mục 7.4; tiền đề LAB-4 mục 5.11 điểm 1 | Spec nói nhập muộn ngày nhận tiền của ngày đã khóa đi đường điều chỉnh có duyệt, nhưng điều chỉnh chỉ nhắm giao dịch hoặc kết quả khuyến khích, mà kết quả chỉ có sau khi có ngày nhận tiền. Ngày nhận tiền bị chặn thì không có kết quả để điều chỉnh: đường này hở | đã sửa thiết kế (đề xuất ở `PREMISE.md` dòng 19, S-01; quyết định và triển khai ở #42) |
| 10. D001 T-D001-02, V-20, V-37, AC-D001-13, AC-D001-57, T-D001-36; J002 T-J002-15 và 12.2 điểm 2; architecture W-INC-03 và mục 13.2 (R2); tiền đề LAB-4 mục 5.11 điểm 1, 3, 4 | Nhiều dòng spec còn ghi "chờ người phụ trách" hoặc "giữ 200 dòng" trong khi tiền đề LAB-4 mục 5.11 đã ghi chấp thuận (check người lập khác người duyệt ở bảng giao dịch; thanh toán thuộc ngày nhận tiền; phân trang). Architecture R2 vừa ghi "chốt" vừa ghi "cần xác nhận" | đã sửa thiết kế (đề xuất ở `PREMISE.md` dòng 20 và 21, S-02, S-03) |
| 11. Architecture mục 12.1 (W-DATA-02); ADR-001 mục 5.1; B002 mục 9.4 và AC-B002-50; báo cáo dùng AI LAB-4 mục 4 điểm 3; db-diff mục 6 điểm 1 | Architecture và ADR-001 vẫn ghi test "view bằng bản cũ cho 17 hồ sơ"; B002 ghi 18 hồ sơ và một ca khác có chủ ý (PRM-1006). Báo cáo AI nói đã sửa nhưng hai chỗ trên chưa sửa | đã sửa thiết kế (đề xuất ở `PREMISE.md` dòng 23, S-05) |
| 12. Architecture mục 12.7 (W-API-03); D001 mục 7.2 và 12.2 điểm 15 | Architecture nói API nhận `market_id`; D001 nói `trade_create` lấy chợ từ lô, không nhận từ máy khách | đã sửa thiết kế (đề xuất ở `PREMISE.md` dòng 22, S-04) |
| 13. Architecture mục 7.1, 3.4, 12.5 (W-LOCK-01); ADR-003 mục 1.2 điểm 2 và 2.1 | Sơ đồ khóa ngày có đường "đã kiểm → chuẩn bị" nhưng W-LOCK-01 chỉ liệt kê bốn hàm, không hàm nào nhận đường này. Bảng đối soát ngày (日次照合表) chỉ có trạng thái, chưa có nội dung, truy vấn hay xuất; "đủ điều kiện" (適格取引) chưa rõ nghĩa | giả định (`PREMISE.md` dòng 51, G-18; hỏi khách qua Q-02) |
| 14. B002 T-B002-04 và mục 9.3, 9.4; db-diff DB10, DB47, DB48, DB49 | T-B002-04 (bỏ cột và view cũ) chỉ ghi phụ thuộc T-B002-03, nhưng `trade_transition`, `seri_blocking_reasons`, `trade_limit_exceeded` vẫn đọc view và cột cũ (B002 mục 9.3 nói "sửa cùng lúc" nhưng dòng việc không ghi phụ thuộc); hàm plpgsql không báo lỗi lúc tạo nên bỏ cột trước khi sửa hàm sẽ hỏng lúc chạy | đã sửa thiết kế (đề xuất ở `PREMISE.md` dòng 24, S-06; thứ tự #13 sau #26) |
| 15. ADR-002 mục 2.2 điểm 2; tiền đề LAB-4 mục 4.1 (A-11); J002 V-J21 và 12.2 điểm 11, 15; architecture mục 13.3 điểm 4 | Ngày hiệu lực: A-11 nói "không ở quá khứ", đề xuất sau đó nói "sau ngày duyệt" (chưa được duyệt). Dữ liệu mẫu nạp bằng migration có ngày duyệt sau ngày hiệu lực nên không qua kiểm này | giả định (`PREMISE.md` dòng 48, G-15) |
| 16. Architecture mục 3.1 (bảng hàm chi tiết, dòng Error); D001 T-D001-13 và 12.2 điểm 6 | Khi ngày đã khóa: architecture nói "ưu tiên trả `reasons` chứa `day_locked`", D001 nói chỉ trả `day_locked` và không ghi dòng. D001 tự nêu khác nhau, chưa được xác nhận | giả định (`PREMISE.md` dòng 50, G-17; theo D001 ở #39) |
| 17. Architecture mục 11.1 (FR-AUDIT-02, FR-AUDIT-03) và 11.4 điểm 4 | Tra cứu audit cần tác nhân bộ phận hành chính – nghiệp vụ và kiểm toán nội bộ; hệ thống chưa có hai vai này, chỉ quản trị hệ thống đọc audit, không có API hay màn tra cứu. W-TEST-05 chỉ kiểm phía ghi. Mục 12.9 của architecture cũng không liệt kê việc này | để mở (ngoài phạm vi hiện tại, không có issue; nên bổ sung vào danh sách việc chưa làm của architecture) |
| 18. B002 mục 12.2 điểm 9; J002 mục 12.2 điểm 7 | Nhiều nhãn giao diện là nhãn tạm chưa có trong bảng thuật ngữ (ví dụ nhãn "chưa có căn cứ", "điều kiện bổ sung", MJ12 đến MJ27) | giả định (`PREMISE.md` dòng 52, G-19) |
| 19. ADR-003 mục 4.2 điểm 7; architecture mục 7.4; tiền đề LAB-4 mục 5.8 và Q-16 | Bản ghi đảo của một giao dịch chỉ ghi chênh lệch tiền và số lượng, chưa trả tồn kho lô (hủy giao dịch ngoài phạm vi); số liệu tồn kho và bản ghi đảo có thể lệch | để mở (chờ Q-16; hạn chế đã ghi vào mô tả #40) |
| 20. D001 AC-D001-62 và mục 9.4; db-diff mục 6 điểm 3; báo cáo dùng AI LAB-4 mục 7 | Số dòng cần điền bù chợ (14 lô, 30 giao dịch, 9 thanh toán) đếm từ migration; tài liệu prototype ghi 27 giao dịch; chưa đối chiếu cơ sở dữ liệu đang chạy | giả định (`PREMISE.md` dòng 40, G-07; đếm lại khi làm #2) |
| 21. J002 mục 12.2 điểm 5; báo cáo dùng AI LAB-4 mục 7; D001 mục 12.2 điểm 2 | Cổng dữ liệu mặc định có thể cắt ở 1.000 dòng (suy luận, chưa kiểm cấu hình) trong khi RFP cho tới 1.200 giao dịch mỗi ngày: phân trang và tính tổng ở phía cơ sở dữ liệu là bắt buộc, nhưng chưa kiểm được | giả định (`PREMISE.md` dòng 53, G-20; kiểm cấu hình khi làm #55, #75, #62) |
| 22. D001 mục 12.1 (Q-13); db-diff DB36 | `trade_code` và `lot_code` là duy nhất toàn cục, không theo chợ; nếu nhiều chợ dùng chung dãy mã thì phải xem lại | để mở (hiện chỉ có một chợ, N-01; hỏi khách ở Q-13; `PREMISE.md` dòng 56, G-23) |
