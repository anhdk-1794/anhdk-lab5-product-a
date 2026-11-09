# Báo cáo tiến độ Sprint 1: Product A

> **Số liệu trong báo cáo này là mô phỏng.** Dự án là hư cấu và chưa có ai thực sự làm việc trong sprint này. Ngày, trạng thái, người phụ trách, số giờ đã làm và hai lỗi (#119, #120) do bên đề xuất dựng lại để minh họa cách theo dõi tiến độ. Ba người phụ trách là giả định.

---

## 1. Iteration và khoảng thời gian

- Iteration trên Project: **Sprint 1 (mô phỏng)**.
- Khoảng thời gian giả lập: 09/10/2026 đến 22/10/2026 (14 ngày lịch, 10 ngày làm việc). Báo cáo lập tại **cuối ngày làm việc thứ 8 (20/10/2026)**.
- Đội hình giả định: 3 người ("Người 1 - dữ liệu và hàm DB", "Người 2 - API và quyền", "Người 3 - giao diện và kiểm thử"), mỗi người 6 giờ làm việc hiệu quả mỗi ngày, công suất sprint 180 giờ.
- Cam kết ban đầu: 7 issue, **138,0 giờ** (77% công suất; phần còn lại để sửa lỗi). Phát sinh trong sprint: 2 lỗi, 16,5 giờ, lấy từ khoản dự phòng sửa lỗi (không nằm trong estimate của 108 issue).
- Mã `#N` là số issue trên Project. Giờ là giờ ước lượng trong [estimate.md](estimate.md), không phải giờ làm thật.
- Nhận định chung: **vàng**, đúng hướng nhưng sát hạn. Việc còn lại 34,5 giờ so với công suất còn lại 36 giờ.

---

## 2. Trạng thái

### 2.1 Theo trạng thái

| Trạng thái | Số issue | Tổng SWP | Ghi chú |
|---|---|---|---|
| New | 102 | 469 | Phần còn lại của backlog (không tính 9 epic); chưa gán người, chưa vào sprint. Số issue gồm 1 issue cha (#44) không có SWP riêng |
| In Progress | 3 | 14 | #11, #59, #120 |
| In Review | 1 | 4 | #68 |
| QA Testing, QA Failed, QA Passed, UAT, Ready for Delivery | 0 | 0 | Sprint này chỉ có việc nền dữ liệu, API và thiết kế test, chưa có bản phát hành để test chạy hay nghiệm thu |
| Done | 5 | 13 | #2, #20, #63, #69, #119 |
| **Cộng** | **111** | **500** | Gồm 2 lỗi mô phỏng (3 SWP) ngoài 497 SWP của estimate |

### 2.2 Chi tiết issue trong sprint

Giờ còn lại là giờ ước tính còn phải làm tại thời điểm báo cáo.

| Issue | Việc | Người phụ trách | SWP | Giờ | Trạng thái | Giờ còn lại |
|---|---|---|---|---|---|---|
| #2 | Bảng chợ và cột chợ cho dữ liệu theo ngày | Người 1 | 5 | 27,5 | Done | 0 |
| #20 | Check "người lập khác người duyệt" ở bảng giao dịch | Người 1 | 1 | 5,5 | Done | 0 |
| #11 | Bảng căn cứ tham gia và bảng yêu cầu căn cứ theo loại | Người 1 | 4 | 22,0 | In Progress | 12,5 |
| #69 | Helper vai dùng chung và ghi chú ánh xạ vai | Người 2 | 3 | 6,5 | Done | 0 |
| #68 | Route hiện có nhận tham số chợ (mặc định chợ duy nhất) | Người 2 | 4 | 22,0 | In Review | 5,0 |
| #63 | Chốt hoặc ghi giả định cho câu hỏi Q-07 đến Q-11 với khách | Người 3 | 3 | 11,5 | Done (còn chờ khách xác nhận, xem R1) | 0 |
| #59 | Dữ liệu thử và test SQL phép tính tiền thưởng thanh toán đúng hạn (完納奨励金) | Người 3 | 8 | 43,0 | In Progress | 7,0 |
| #119 | [BUG FIX] Dòng giao dịch mẫu thiếu `market_id` sau migration (mô phỏng) | Người 1 | 1 | 5,5 | Done | 0 |
| #120 | [BUG FIX] Route nhận tham số chợ lỗi 500 khi bỏ trống (mô phỏng) | Người 2 | 2 | 11,0 | In Progress | 10,0 |
| | **Cộng** | | **31** | **154,5** | | **34,5** |

Đã làm: 5 issue xong hoàn toàn (56,5 giờ); tính cả phần dở của các issue đang làm thì đã làm 120,0 giờ trên 154,5 giờ.

Ngày và giờ đã làm của từng issue (mô phỏng) cũng nằm trên Project, ở ba trường "Ngày bắt đầu (mô phỏng)", "Ngày kết thúc (mô phỏng)" và "Giờ đã làm (mô phỏng)". Ngày kết thúc chỉ điền cho issue đã xong.

| Issue | Bắt đầu | Kết thúc | Giờ đã làm |
|---|---|---|---|
| #2 | 09/10 | 15/10 | 27,5 |
| #20 | 15/10 | 16/10 | 5,5 |
| #11 | 19/10 | | 9,5 |
| #69 | 09/10 | 12/10 | 6,5 |
| #68 | 16/10 | | 17,0 |
| #63 | 09/10 | 12/10 | 11,5 |
| #59 | 12/10 | | 36,0 |
| #119 | 16/10 | 19/10 | 5,5 |
| #120 | 20/10 | | 1,0 |
| **Cộng** | | | **120,0** |

Giờ đã làm của mỗi issue bằng giờ ước tính trừ giờ còn lại ở bảng trên. Ngày được xếp theo công suất 6 giờ mỗi người mỗi ngày, khớp với bảng burndown.

---

## 3. Burndown (mô phỏng)

Giờ còn lại cuối mỗi ngày, tính cả việc phát sinh khi nó xuất hiện (+5,5 giờ ở ngày 5, +11,0 giờ ở ngày 7). Đường lý tưởng là 138 giờ giảm đều về 0 trong 10 ngày, không tính việc phát sinh.

| Ngày | Ngày tháng | Lý tưởng (giờ) | Thực tế mô phỏng (giờ) | |
|---|---|---|---|---|
| 0 | trước sprint | 138,0 | 138,0 | ███████████████████████ |
| 1 | 09/10 | 124,2 | 120,0 | ████████████████████ |
| 2 | 12/10 | 110,4 | 108,0 | ██████████████████ |
| 3 | 13/10 | 96,6 | 96,0 | ████████████████ |
| 4 | 14/10 | 82,8 | 84,0 | ██████████████ |
| 5 | 15/10 | 69,0 | 77,5 | █████████████ (+5,5 lỗi #119) |
| 6 | 16/10 | 55,2 | 59,5 | ██████████ |
| 7 | 19/10 | 41,4 | 52,5 | █████████ (+11,0 lỗi #120) |
| 8 | 20/10 | 27,6 | 34,5 | ██████ |

**Thực tế lệch kế hoạch ở đâu.**
- Ngày 1 đến 4 đội bám sát đường lý tưởng.
- Từ ngày 5 chậm dần vì việc phát sinh: hai lỗi cộng 16,5 giờ vào phạm vi, trong khi đường lý tưởng không có khoản này.
- Người 2 không có việc phù hợp từ ngày 2 đến ngày 5 (khoảng 23,5 giờ) vì #68 phải chờ #2 xong. Đây là lỗi sắp xếp thứ tự của sprint, không phải thiếu người.
- Cuối ngày 8, còn thiếu khoảng 7 giờ so với đường lý tưởng, tương đương nửa ngày công của cả đội.
- Hai ngày cuối công suất không chia đều: Người 1 cần 12,5 giờ (công suất 12), Người 2 cần 15 giờ (công suất 12), Người 3 cần 7 giờ (công suất 12).

---

## 4. Rủi ro trễ

| Rủi ro | Issue | Ảnh hưởng tới mốc nào | Hành động | Ai làm, khi nào |
|---|---|---|---|---|
| R1. Khách chưa trả lời Q-07 đến Q-11, #63 mới chỉ ghi giả định. Nếu khách trả lời khác thì phải sửa các giả định và những issue dựa trên đó | #63 | Hết Sprint 1 (22/10); các issue ở sprint sau dựa trên giả định | Đề nghị khách xác nhận bằng văn bản | Người phụ trách dự án gửi nhắc ngay 20/10, hạn trả lời 22/10 |
| R2. Đường găng đi qua #2 rồi #11: Người 2 chờ khoảng 23,5 giờ, #18 (phụ thuộc #11) không thể vào sprint này | #2, #11, #68, #18 | Bắt đầu #18 ở Sprint 2 muộn hơn dự kiến | Sprint sau đưa việc không phụ thuộc lên trước; cho Người 2 viết test trong lúc chờ | Người phụ trách dự án, lúc lập backlog Sprint 2 (23/10) |
| R3. Công suất hai ngày cuối không chia đều: Người 1 thiếu 0,5 giờ, Người 2 thiếu 3 giờ, Người 3 dư 5 giờ | #11, #68, #120 | Hết Sprint 1 (22/10): #11 trễ khoảng nửa ngày | Người 3 nhận phần test của #120 (5 giờ); phần 0,5 giờ còn lại của #11 chuyển sang Sprint 2 nếu không kịp | Người 3 từ sáng 21/10; Người 1 báo lại cuối ngày 21/10 |
| R4. Việc lớn không vừa một sprint: 17 issue trên 40 giờ; lớn nhất #81, #96, #97 (53,5 giờ), #118 (49,5 giờ), #19 (44 giờ). Một người làm cả sprint chỉ được khoảng 60 giờ | #81, #96, #97, #118, #19 | Sprint 2 và sau | Tách #81, #96, #97 thành hai phần trước khi đưa vào sprint | Người phụ trách dự án, trước 23/10 |
| R5. Hai lỗi phát sinh dùng 16,5 giờ trên 154,5 giờ cam kết (khoảng 11%, dự phòng 20%). Nếu tỷ lệ lỗi tăng khi vào việc phức tạp, dự phòng không đủ | #119, #120; epic E-05 và E-06 | Sprint 2 và sau | Theo dõi tỷ lệ lỗi sau Sprint 2, điều chỉnh hệ số nếu cần | Người phụ trách dự án, hết Sprint 2 |
| R6. Tỷ lệ thưởng thanh toán đúng hạn đổi từ 0,5% sang 0,6% kể từ **25/10/2026**, ngay sau sprint; quy tắc theo phiên bản (epic E-04) chưa bắt đầu. Nếu chưa có phiên bản mới thì tính thưởng sau 25/10 sai | #14 và các issue E-04 | 25/10/2026 | Đưa #14 (khung phiên bản quy tắc) lên đầu Sprint 2; hỏi khách xác nhận ngày áp dụng 0,6% | Người 1 làm #14 từ 23/10; người phụ trách dự án hỏi khách trước 22/10 |
| R7. Giờ ước lượng dựa trên hệ số rút từ ước lượng thô ban đầu (sai số ±30%) và SWP do AI chấm, chưa có người rà soát từng điểm. Giờ thực có thể lệch nhiều | cả backlog | Kế hoạch các sprint sau | Đo giờ thực sau Sprint 1 để hiệu chỉnh hệ số | Người phụ trách dự án, 23/10 |

### Nháp backlog Sprint 2 (chưa cam kết)

Ứng viên xếp theo phụ thuộc, sau khi #2 và #11 xong:

| Issue | Việc | Giờ |
|---|---|---|
| #14 | Khung phiên bản quy tắc: bảng, enum, ràng buộc (phụ thuộc #2; xử lý R6) | 22,0 |
| #16 | Bảng khóa ngày, lịch chợ và điều chỉnh sau khóa (phụ thuộc #2) | 22,0 |
| #17 | Mở rộng bảng audit: cột, chỉ mục, chỉ thêm (phụ thuộc #2) | 16,5 |
| #18 | RLS và quyền cho bảng căn cứ; bỏ ghi trực tiếp hồ sơ (phụ thuộc #11) | 16,5 |
| #60, #61, #62 | Thiết kế test cho tiền thưởng (không phụ thuộc issue dev nào) | 43,0 mỗi issue |

Quyết định cần từ khách: xác nhận Q-07 đến Q-11 (R1) và ngày bắt đầu áp dụng tỷ lệ 0,6% (R6).
