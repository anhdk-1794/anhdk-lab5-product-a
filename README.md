# anhdk-lab5-product-a

- **Dự án:** Product A, hệ thống hỗ trợ vận hành chợ bán buôn thủy sản (khách hư cấu, không dùng dữ liệu của khách thật). Phần này break task, dựng GitHub Project và báo cáo tiến độ một sprint mô phỏng.
- **Mã học viên:** `anhdk-1794` (tài khoản GitHub, trùng tiền tố `anhdk` của các repo Lab 2 và Lab 4).
- **Đề:** Product A.

---

## 1. Tổng quan

- **Backlog break từ bộ spec Lab 4** (repo `anhdk-lab4-product-a`, nhánh `main`, commit `76f9a66`, nộp ngày 2026-11-09).
- **`PREMISE.md` có thay đổi.** Có 6 đề xuất sửa spec (S-01 đến S-06), 27 giả định bổ sung (G-01 đến G-27) và 17 hướng đã chốt (H-01 đến H-17). Spec Lab 4 chưa được sửa: mọi thay đổi chờ người phụ trách dự án xác nhận.
- **9 epic**, **109 issue** dưới epic (gồm 1 issue cha có 5 sub-issue), cộng 2 issue lỗi mô phỏng thêm trong sprint: Project có **120 mục** tổng cộng. Epic E-06 (tiền thưởng thanh toán đúng hạn, 完納奨励金) tách riêng thành 19 issue (gồm 1 issue cha có 5 sub-issue).
- **Estimate:** 108 issue có điểm, **497 SWP, 2.496,0 giờ**, cộng dự phòng sửa lỗi 20% (254,3 giờ) = **2.750,3 giờ** (xem `estimate.md`). Hai lỗi mô phỏng nằm ngoài estimate (3 SWP, 16,5 giờ).
- **Sprint giả lập đã chạy:** "Sprint 1 (mô phỏng)", 09/10/2026 đến 22/10/2026, báo cáo tại ngày làm việc thứ 8. Mọi trạng thái, người phụ trách, ngày bắt đầu và kết thúc thực tế (Actual), giờ đã làm và burndown trong sprint là **mô phỏng**, không có ai làm việc thật.

---

## 2. Link

| Mục | Link |
|---|---|
| Repo | https://github.com/anhdk-1794/anhdk-lab5-product-a |
| GitHub Project | https://github.com/users/anhdk-1794/projects/1 |
| Báo cáo HTML của skill chấm SWP | [`reports/sun-estimate-lab5-product-a.html`](reports/sun-estimate-lab5-product-a.html) |

**Mở thử khi không đăng nhập:** đã thực hiện ngày 2026-11-09. Repo, README, một issue và trang Project đều mở được (HTTP 200) khi không gửi thông tin đăng nhập. Đây là kiểm bằng yêu cầu web không đăng nhập, không phải cửa sổ ẩn danh của trình duyệt. Cả repo và Project đều ở chế độ public.

---

## 3. Cấu trúc repo

| File / thư mục | Nội dung |
|---|---|
| `README.md` | File này: tổng quan, link, cấu trúc, đối chiếu spec với epic |
| `PREMISE.md` | Tiền đề: thay đổi so với spec Lab 4, giả định bổ sung, hướng đã chốt |
| `task-breakdown.md` | Cách chia task, bảng spec → issue, hạng mục tách riêng (tiền thưởng thanh toán đúng hạn), chỗ hở của spec phát hiện khi chia |
| `estimate.md` | Cách chấm SWP, quy ra giờ, tổng theo epic, so với estimate ban đầu |
| `progress-report.md` | Báo cáo tiến độ Sprint 1 (mô phỏng): trạng thái, burndown, rủi ro trễ |
| `AI-report.md` | Cách dùng AI trong phần này: skill, prompt, review, lỗi của AI |
| `reports/` | Báo cáo HTML do skill chấm SWP tạo ra |
| `skills/` | Không có: không viết hay chỉnh skill nào cho phần này |
| `.gitignore` | Loại file tạm, khóa và biến môi trường khỏi repo |

---

## 4. Spec → epic

Mã `T-…` (bảng việc của từng màn), `W-…` (nhóm việc kiến trúc), B002, D001, J002 là mã của bộ spec Lab 4. Ba màn: B002 hồ sơ người tham gia, D001 giao dịch thỏa thuận trực tiếp (相対取引), J002 tiền thưởng thanh toán đúng hạn. Chi tiết từng mã nằm ở `task-breakdown.md`, mục 2.

| Spec (mã hoặc file) | Epic trên Project | Ghi chú |
|---|---|---|
| `W-DATA-01` đến `06`; dữ liệu và migration của B002, D001, J002 (`T-B002-01`..`07`, `T-D001-01`..`06`, `T-J002-01`..`09`) | #1 — Nền dữ liệu và chợ | Có bảng chợ nhiều chợ và chuyển hồ sơ mẫu (xem `PREMISE.md`, phần 1) |
| `W-AUTH-01` đến `03`; phần quyền của ba màn | #3 — Phân quyền, RLS, tách người lập và người duyệt | Chính sách theo hàng (RLS) theo chợ chưa làm (kiến trúc mục 12.9, rủi ro R8) |
| `W-ELIG-01` đến `04`; hàm kiểm tư cách (`T-B002-08`..`11`, `T-B002-33`, `T-D001-07`..`16`) | #4 — Tư cách tham gia hai căn cứ và kiểm khi chốt giao dịch | Giấy phép (許可) và chấp thuận (承認) là hai căn cứ riêng. Đổi trạng thái căn cứ chỉ viết sẵn, chưa có màn (chờ Q-12) |
| `W-RULE-01` đến `04`; phiên bản quy tắc (`T-J002-10`..`13`, `T-J002-20`, `T-J002-24`..`27`) | #5 — Quy tắc theo chợ và phiên bản | Chưa có màn quản lý phiên bản. Tỷ lệ 0,5% và 0,6% kể từ 2026-10-25 (giả định G-01) |
| `W-LOCK-01` đến `06`; khóa ngày và điều chỉnh | #6 — Khóa ngày và điều chỉnh sau khóa | Chưa có màn khóa ngày và màn điều chỉnh (spec chưa có) |
| `W-INC-01` đến `07`; tính tiền thưởng, nhập ngày nhận tiền, tổng hợp (`T-J002-14`..`23`, `T-J002-28`, `T-J002-47`, `T-J002-48`) | #7 — Tiền khuyến khích thanh toán đúng hạn (完納奨励金) | **Ràng buộc đặc thù của Product A, tách riêng 19 issue**; `W-INC-07` là câu hỏi mở với khách (Q-07 đến Q-11), làm thành issue làm rõ #63 |
| `W-API-01` đến `03`; API của ba màn (`T-B002-12`..`16`, `T-D001-17`..`19`) | #8 — API và ánh xạ lỗi | `W-API-03` sửa theo S-04 trong `PREMISE.md` |
| Giao diện của ba màn (`T-B002-17`..`29`, `T-D001-20`..`37`, `T-J002-29`..`44`) | #9 — Giao diện hồ sơ người tham gia và giao dịch | Không có nút hủy giao dịch (đã bỏ); danh sách phân trang theo S-02 |
| `W-TEST-01` đến `06`; kiểm thử của ba màn (`T-B002-35`..`46`, `T-D001-43`..`60`, `T-J002-50`..`66`) | #10 — Kiểm thử, nghiệm thu và triển khai | Gồm 20 cặp thiết kế test và chạy test tách thành hai ticket riêng. Việc bổ sung (triển khai, nghiệm thu, rà soát, tài liệu vận hành) là suy luận, xem G-27 |
| Kiến trúc mục 12.9 (việc có trong RFP nhưng spec chưa thiết kế: hủy giao dịch trước khóa, nhánh yêu cầu sửa và chốt lại…) | Không có epic | Chưa thiết kế nên chưa có issue; xem `task-breakdown.md`, phần 4 |
