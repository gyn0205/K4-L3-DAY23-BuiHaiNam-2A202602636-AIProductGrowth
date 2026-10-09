# OPERATING DASHBOARD — SalesCoach AI

**Loại mô hình:** B2B (sàn môi giới BĐS trả tiền, sales của sàn dùng) · **Cập nhật:** 09/10/2026 · Bùi Hải Nam – 2A202602636
**NORTH STAR:** Time-to-first-value — hiện tại: chưa có sàn nào (n = 0) — mục tiêu: ≤ 8 ngày ở cả 2 sàn chạy thử Tháng 1
**Trạng thái thật:** 0 khách trả tiền · 0 pilot · 0 partner đã liên hệ. ⚪ = chưa có số đo; số trong ngoặc là số của mô hình Day 22, không phải kết quả.

### Đèn báo sớm (Leading — nhìn hằng ngày/tuần)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---|---|---|---|
| **1. TTFV** ⭐ — ngày từ lúc sàn xác nhận đến lúc ≥50% sales có ≥1 phiên hoàn tất trên kịch bản của sàn | ⚪ n = 0 | ≤ 8 ngày / 9–15 / > 15 | [MH] kịp dùng hết 220 phiên cam kết tháng đầu; 15 ngày = lứa sales mới đã ra thị trường | Đèn 5, đèn 6 |
| **2. Tỷ lệ hoàn tất phiên** — phiên hoàn tất ÷ mọi phiên đã bắt đầu | ⚪ (ước 75%) | ≥ 75% / 63,5–75% / < 63,5% | [MH] 63,5% = điểm GM rơi xuống 60% | Đèn 4 → đèn 7 |
| **3. Pipeline coverage** — sàn đủ điều kiện ÷ số sàn trả tiền còn thiếu (3) | 🔴 0× (0/12) | ≥ 4× / 2–4× / < 2× | [TB] 1 ÷ win rate 25% (số mượn); tính lại 11/12/2026 | Đèn 6 → đèn 8 |

### Đèn vận hành (Operating — nhìn hằng tuần/tháng)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn | Báo trước cho |
|---|---|---|---|---|
| **4. Chi phí AI biến đổi / phiên hoàn tất** — LLM + STT + TTS của mọi phiên, gồm retry và phiên bỏ dở | ⚪ ($0,077) | ≤ $0,085 / 0,085–0,194 / > $0,194 | [MH] GM ≥ 60% ở sàn 220 phiên / ở sàn 300 phiên | Đèn 7 |
| **5. Phiên hoàn tất / sàn / tuần** — từng sàn, không lấy trung bình | ⚪ n = 0 | ≥ 69 / 51–68 / < 51 | [MH] 51 = mức cam kết 220 phiên/tháng; 69 = mức ARPU $450 | Đèn 7, đèn 8 (gia hạn) |
| **6. Pilot → trả tiền** — sàn đã chuyển khoản đủ hoá đơn đầu ÷ pilot đã kết thúc ≥30 ngày | ⚪ 0/0 | ≥ 50% / 36–50% / < 36% | [BM] ICONIQ State of GTM 2026: ~50% (2026), ~36% (2025) — kiểm tra 09/10/2026 | Đèn 8 |

### Đèn kết quả (Lagging — nhìn hằng quý)

| Đèn | Hiện | 🟢 / 🟡 / 🔴 | Nguồn |
|---|---|---|---|
| **7. Gross Margin từng sàn** | ⚪ (67,8%) | ≥ 60% / 53–60% / < 53% | [MH] 60% = mức đặt giá Day 22 · [BM] 53% = GM AI 2026P, ICONIQ State of AI 2026 — kiểm tra 09/10/2026 |
| **8. CAC thực / sàn trả tiền** | ⚪ (dự tính $1.080) | ≤ $1.830 / 1.830–3.660 / > $3.660 | [MH] payback 12 tháng = $3.660; LTV:CAC ≥ 3 = $1.830 |

### 5 luật quyết định (⏹ = luật dừng)

1. ⏹ **NẾU** TTFV > 15 ngày **TRÊN** 2 sàn gần nhất **THÌ** dừng nhận sàn mới và cắt gói khởi động xuống 1 kịch bản cho 1 nhóm ≤10 sales, đến khi có một sàn đạt ≤ 8 ngày **KHÔNG THÌ** không ký thêm sàn để kịp mục tiêu 3 sàn, không dựng đủ 5 kịch bản trước khi sàn có phiên đầu tiên.
2. ⏹ **NẾU** tỷ lệ hoàn tất < 63,5% **TRONG** 2 tuần liên tiếp **VÀ** mỗi tuần ≥40 phiên bắt đầu **THÌ** dừng ký hợp đồng trả tiền mới, tuần kế tiếp sửa đúng lý do bỏ dở đứng đầu trong log rồi đo lại trên 40 phiên **KHÔNG THÌ** không nới định nghĩa "phiên hoàn tất", không tăng giá để bù biên.
3. **NẾU** chi phí AI / phiên hoàn tất > $0,194 **TRONG** 2 tuần liên tiếp **VÀ** mỗi tuần ≥50 phiên hoàn tất **THÌ** hạ trần phiên từ 15 xuống 10 phút ngay trong tuần và áp đơn giá phiên vượt mới cho hợp đồng ký sau đó **KHÔNG THÌ** không ngồi tối ưu prompt hay đổi LLM (LLM ~$0,004/phiên, TTS đắt gấp 8 lần), không tăng giá với sàn đang trong hợp đồng.
4. **NẾU** pipeline coverage < 4× **TRONG** 2 tuần liên tiếp **THÌ** tuần kế tiếp đặt 4 cuộc hẹn trực tiếp với người ký; đến 30/10/2026 Meey Group chưa hẹn thì gửi đề xuất cho partner dự phòng **KHÔNG THÌ** không bán dưới 39.000 ₫/phiên (giá sàn 37.708 ₫), không tặng tháng miễn phí, không mở kênh thứ hai.
5. **NẾU** một sàn trả tiền < 51 phiên hoàn tất/tuần **TRONG** 3 tuần liên tiếp sau TTFV **THÌ** trong 5 ngày làm việc đến sàn, gắn 2 phiên/sales/tuần có tên vào giao ban sáng thứ Hai và gửi báo cáo tuần cho người ký **KHÔNG THÌ** không chào thêm kịch bản hay tài khoản, không dựng kịch bản mới theo yêu cầu của sàn đó.

### Cổng gác 90 ngày (tính từ 12/10/2026)

| Ngày | Metric (1) | Ngưỡng qua cổng | Bằng chứng | Nếu trượt |
|---|---|---|---|---|
| 30 · 11/11/2026 | Tỷ lệ hoàn tất phiên đo thật | ≥ 63,5% trên ≥80 phiên bắt đầu ở 2 sàn chạy thử | File CSV xuất từ bảng `practice_session` (mã phiên, sàn, trạng thái, số lượt, lý do bỏ dở) | **FIX** — sửa lý do bỏ dở số 1, đo lại 80 phiên trước 11/12; trượt lần hai = PIVOT sang tính tiền theo phút |
| 60 · 11/12/2026 | TTFV | ≤ 15 ngày ở ≥2 sàn, ghi số ngày từng sàn | Bảng TTFV: ảnh tin nhắn/hợp đồng xác nhận ngày bắt đầu + log phiên của sàn | **FIX** — cắt gói khởi động còn 1 kịch bản, 1 nhóm ≤10 sales |
| 90 · 10/01/2027 | Số sàn đã chuyển khoản đủ hoá đơn cam kết 8,58 triệu ₫ | ≥ 2 sàn | Sao kê ngân hàng + hoá đơn + Pilot Report từng sàn | 1 sàn: **PIVOT** đổi partner hoặc ngách · 0 sàn: **KILL** |

**KILL CRITERIA:** Đến hết ngày 10/01/2027, nếu có 0 sàn đã chuyển khoản đủ một hoá đơn cam kết 8.580.000 ₫ thì dừng SalesCoach AI ở ngách sàn môi giới BĐS: tắt cụm server ~$70/tháng, không nhận pilot mới.

**CHƯA ĐO ĐƯỢC:** 7/8 đèn chưa có số thật. **Tỷ lệ hoàn tất** — cần 80 phiên thật ở 2 sàn chạy thử; có số 08/11/2026. **Chi phí AI/phiên** — chưa có usage log theo phiên, cần ghi token và giây STT/TTS theo mã phiên; có số 31/10/2026. **"Phiên hoàn tất"** — sự kiện mở feedback chưa được ghi nên hiện đếm thừa; 31/10/2026. **TTFV, phiên/sàn/tuần** — cần sàn đầu tiên; 08/11/2026. **Win rate** (đang mượn 25%) — cần 8 cơ hội đầu có kết quả; 11/12/2026. **Pilot → trả tiền, GM thật, CAC thật** — cần pilot trả tiền đầu tiên kết thúc; 05/01/2027. **NRR, vòng đời khách 18 tháng, % deal chết ở khâu duyệt** — không đo được trong 90 ngày; sớm nhất 02/2027.
