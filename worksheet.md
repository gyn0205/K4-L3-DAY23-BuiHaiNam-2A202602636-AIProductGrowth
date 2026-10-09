# Worksheet — SalesCoach AI

Họ tên: Bùi Hải Nam · MSSV: 2A202602636 · Ngày làm: 09/10/2026

**Sản phẩm:** SalesCoach AI — sales bất động sản luyện gọi khách bằng giọng nói với khách hàng AI, nhận điểm và nhận xét theo scorecard (dự án Build Phase P-067).

**Số liệu dùng trong bài lấy từ đâu:** mô hình Day 22 của tôi (`BuiHaiNam_Day22_model.xlsx`, giá API chốt 08/10/2026, tỷ giá 26.000 ₫/USD). Phần lớn đầu vào của mô hình đó là **ước tính chưa đo** — tôi ghi lại đúng như vậy ở từng chỗ. Tính đến hôm nay sản phẩm có **0 khách trả tiền, 0 pilot, 0 partner đã liên hệ**; bằng chứng thật duy nhất là 5 ca eval thủ công ngày 03/10/2026 (5/5 PASS).

| Số của mô hình Day 22 | Giá trị | Mức độ thật |
|---|---|---|
| Giá bán | 39.000 ₫ = $1,50 / phiên luyện hoàn tất | Giả thuyết giá, chưa sàn nào ký |
| Cam kết tối thiểu | 220 phiên/tháng = 8.580.000 ₫ ($330) | Giả thuyết |
| Khách tham chiếu | 1 sàn 40 sales · 400 phiên bắt đầu → 300 phiên hoàn tất/tháng | Ước tính (giả định pilot của PRD) |
| ARPU | $450/tháng · ACV $5.400 | Suy từ dòng trên |
| Tỷ lệ hoàn tất | 75% | **Ước tính, chưa đo** |
| Cost/Job (chưa overhead) | $0,4834 / phiên hoàn tất | Giá API thật 08/10/2026; token, thời lượng, infra, giờ người là ước tính |
| Gross Margin | 67,8% | Suy từ dòng trên |
| Chi phí giờ trainer/QA | $4,858/giờ | Từ tin tuyển dụng 08/10/2026 |
| Ngân sách CAC (payback 12 tháng) | $3.660 / sàn | Suy từ ARPU × GM |
| Vòng đời khách | 18 tháng | Ước tính, chưa có dữ liệu churn |

---

## Trạm 1 — Loại mô hình

Ba câu hỏi, trả lời theo thực tế hôm nay:

1. **Ai trả tiền?** Sàn môi giới 30–100 sales — người ký là Giám đốc kinh doanh hoặc Trưởng phòng đào tạo, tiền từ ngân sách đào tạo & onboarding sales. Là doanh nghiệp → không phải B2C.
2. **Ai dùng?** Sales của sàn đó (nhân viên của bên trả tiền) và trưởng nhóm/trainer duyệt đánh giá. Không ai trong số họ là *khách hàng của* sàn → không phải B2B2C.
3. **Bên trung gian?** Kế hoạch Day 22 đi qua Meey CRM, nhưng đó là kênh giới thiệu + một nút nhúng; tôi chưa liên hệ, chưa có hợp đồng. HANDBOOK §3.3 xếp "giới thiệu / phân phối" là kênh bán hàng, dùng bảng B2B.

**Câu chốt loại:** Chúng tôi là **B2B** vì tiền đến từ sàn môi giới (ngân sách đào tạo do Giám đốc kinh doanh ký), người dùng là sales và trainer — nhân viên của chính sàn đó chứ không phải khách của sàn — và Meey CRM chỉ là kênh giới thiệu chưa tồn tại ngoài kế hoạch, nên không có lớp "partner đẩy tới end-user" nào để gọi là B2B2C.

**Đèn bật trước của tôi:** Time-to-first-value.

**Ghép chu kỳ bán với runway (biến số 3 của §3.2).** Nhóm là học viên, chưa trả lương; tiền mặt đốt mỗi tháng chỉ là cụm server ~$70 (ước tính). "Runway" thật của tôi là thời gian: 90 ngày tự đặt, 12/10/2026 → 10/01/2027. Chu kỳ bán tôi ước ~2 tháng (Day 22, Tab 4 — chưa có số thật; ICONIQ ghi trung bình 19 tuần cho B2B software). 90 − 60 = **30 ngày**: sàn nào chưa vào pipeline trước 11/11/2026 thì gần như không kịp trả tiền trước ngày 90. Vì vậy pipeline coverage phải là đèn báo sớm, và cổng ngày 30 không thể là cổng doanh thu.

**Bảng đèn §3.2 (B2B)** — đủ 11 đèn. ✅ đo được hôm nay · 🔧 đã biết cách đo, ghi rõ cần gì và khi nào có số · ❌ chưa đo được.

| Đèn | ✅ / 🔧 / ❌ | Số nằm ở đâu / cần gì để đo |
|---|---|---|
| **TTFV** ⭐ | 🔧 | Cần (a) ghi ngày sàn xác nhận chạy vào một sheet, (b) xuất `practice_session` theo từng sàn. Chưa có sàn nào → số đầu tiên từ 2 sàn chạy thử Tháng 1, dự kiến 08/11/2026. |
| Pipeline coverage | ✅ | Google Sheet danh sách sàn của tôi. Hôm nay = **0** sàn đủ điều kiện. |
| % deal chết ở security/procurement | ❌ | Chưa có deal nào đóng (thắng hay thua). Tôi cũng chưa biết khâu "duyệt" ở một sàn 30–100 sales trông ra sao — có thể chỉ là một người quyết. Cần ≥5 deal đã đóng; không có số trong 90 ngày. |
| POC → paid | 🔧 | Đếm tay được. Số đầu tiên chỉ có sau khi pilot đầu kết thúc 30 ngày: dự kiến 05/01/2027 — muộn hơn 2 tuần nhiều. |
| Sales cycle (tuần) | 🔧 | Ghi ngày gặp đầu tiên và ngày ký trong cùng sheet. Số đầu tiên khi có chữ ký đầu tiên. |
| Usage depth | 🔧 | Đếm phiên `completed` theo sàn theo tuần. Sự kiện `feedback_viewed` chưa được ghi (Tech Lead P-067, hạn 31/10/2026) nên trước đó con số đếm thừa. |
| Chi phí triển khai ÷ ACV | 🔧 | Bảng chấm giờ theo từng sàn × $4,858/giờ. Chưa có sàn. |
| Tập trung doanh thu | ❌ | Doanh thu = 0. Sàn đầu tiên luôn là 100%; đèn chỉ có nghĩa khi có ≥3 sàn. |
| NRR | ❌ | Cần ít nhất 2 kỳ hoá đơn của ≥3 sàn. Sớm nhất 02/2027. |
| Gross Margin | 🔧 | Cần doanh thu thật + hoá đơn OpenRouter/Soniox + usage log theo phiên (AC-09 của PRD chưa làm). Hôm nay chỉ có số mô hình 67,8%. |
| CAC payback | ❌ | Cần sàn trả tiền đầu tiên và bảng chấm giờ bán hàng. Hôm nay chỉ có ngân sách tính từ mô hình. |

---

## Trạm 2 — Thẻ đèn

**North Star:** TTFV — hiện tại: chưa có sàn nào (n = 0) — mục tiêu: ≤ 8 ngày ở cả 2 sàn chạy thử Tháng 1.

"Phiên hoàn tất" trong mọi thẻ dùng đúng định nghĩa job của Day 22: phiên ở trạng thái `completed`, sales có ≥4 lượt nói, evaluation lưu thành công kèm bằng chứng trích từ transcript, sales mở feedback trong 24 giờ.

| # | Tầng | Đèn | Định nghĩa (đếm gì · **không** đếm gì) | Công thức | Nhịp · ai lấy số | Báo trước cho |
|---|---|---|---|---|---|---|
| 1 | L | **TTFV** ⭐ | Số ngày lịch từ *ngày bắt đầu* (sàn xác nhận bằng văn bản — hợp đồng, email hoặc Zalo — và gửi danh sách sales) đến *ngày đạt* (ngày đầu tiên ≥50% sales trong danh sách có ≥1 phiên hoàn tất trên kịch bản dựng theo dự án của chính sàn). **Không** đếm: tài khoản đã tạo xong, buổi demo/tập huấn, phiên trên kịch bản mẫu, phiên của tài khoản test hay của tôi. | ngày đạt − ngày bắt đầu; tính riêng từng sàn, không lấy trung bình | Mỗi sàn, cập nhật hằng ngày khi đang onboarding · Bùi Hải Nam ghi ngày bắt đầu, Tech Lead xuất log | #5 và #6, sớm 3–6 tuần |
| 2 | L | **Tỷ lệ hoàn tất phiên** | Phiên hoàn tất ÷ mọi phiên đã bấm "Bắt đầu luyện tập" trong tuần. **Không** được loại phiên lỗi kỹ thuật hay phiên bỏ dở khỏi mẫu số; **không** đếm tài khoản test. | phiên hoàn tất ÷ phiên bắt đầu | Tuần · Tech Lead P-067 | #4 → #7, sớm 2–4 tuần |
| 3 | L | **Pipeline coverage** | Số sàn *đủ điều kiện* đang mở ÷ số sàn trả tiền còn thiếu so với mục tiêu 90 ngày (3 sàn). Đủ điều kiện = cả bốn: sàn căn hộ sơ cấp 30–100 sales · đã gặp người ký · người ký đã xem demo · có đợt tuyển sales hoặc mở bán trong 60 ngày tới. **Không** đếm: sàn mới nhắn tin, sàn chỉ gặp sales/trưởng nhóm, sàn partner "hứa giới thiệu", sàn đã từ chối. | sàn đủ điều kiện ÷ (3 − sàn đã trả tiền) | Tuần · Bùi Hải Nam | #6 → #8, sớm 4–8 tuần |
| 4 | O | **Chi phí AI biến đổi / phiên hoàn tất** | Tiền LLM + STT + TTS của **mọi** phiên bắt đầu trong tuần (gồm retry và phiên bỏ dở) chia cho số phiên hoàn tất. **Không** gồm infra cố định, giờ người, chi phí chạy lại bộ eval. Tính riêng từng sàn. | Σ(token × giá + giây STT/TTS × giá) ÷ phiên hoàn tất | Tuần · Tech Lead (usage log theo `session_id`; đối chiếu hoá đơn nhà cung cấp cuối tháng) | #7, sớm 4–8 tuần so với hoá đơn tháng |
| 5 | O | **Phiên hoàn tất / sàn / tuần** | Số phiên hoàn tất của **từng** sàn trả tiền, thứ Hai–Chủ nhật, tính từ sau ngày đạt TTFV. **Không** lấy trung bình các sàn; **không** đếm phiên bắt đầu mà chưa hoàn tất hay lượt mở lại feedback cũ. | đếm theo sàn | Tuần · Tech Lead | #7 (doanh thu vượt cam kết) và #8 (sàn có ở lại đủ lâu để hoàn vốn không) |
| 6 | O | **Pilot → trả tiền** | Số sàn đã **chuyển khoản đủ** hoá đơn cam kết đầu tiên (8,58 triệu ₫) trong 30 ngày sau khi pilot kết thúc ÷ số pilot đã kết thúc ≥30 ngày. **Không** đếm: đồng ý miệng, hợp đồng ký chưa thanh toán, pilot đang chạy, pilot gia hạn miễn phí. | sàn đã trả ÷ pilot đã kết thúc ≥30 ngày | Tháng, cộng dồn · Bùi Hải Nam | #8 (pilot trượt là chi phí dồn lên sàn thắng) |
| 7 | G | **Gross Margin từng sàn** | (Doanh thu đã xuất hoá đơn − COGS của sàn) ÷ doanh thu. COGS = API + retry + infra chia theo số phiên + giờ QA/kịch bản/hỗ trợ × $4,858. **Không** gồm overhead và phần chia cho partner (tính vào CAC). | (DT − COGS) ÷ DT | Tính theo tháng, nhìn theo quý · Bùi Hải Nam | — |
| 8 | G | **CAC thực / sàn trả tiền** | (Giờ bán hàng và chạy pilot của tôi × $4,858 + phần chia cho partner năm đầu + chi phí các pilot không chuyển đổi trong kỳ) ÷ số sàn trả tiền mới. **Không** đếm giờ làm sản phẩm. | Σ chi phí bán ÷ sàn trả tiền mới | Quý · Bùi Hải Nam | — |

Đèn chi phí AI là đèn số: **4** (đèn 2 báo trước cho nó: phiên bỏ dở vẫn đốt phút STT/TTS nhưng không được tính tiền).

Đếm tầng: 3 Leading · 3 Operating · 2 Lagging.

**Đèn trong bảng §3.2 tôi cố ý không đưa lên dashboard**

- *Chi phí triển khai ÷ ACV:* dựng 5 kịch bản ban đầu ước 10 giờ × $4,858 = $48,6 = 0,9% ACV. Muốn chạm ngưỡng vàng 15% phải tốn 167 giờ cho một sàn. Với giá nhân công ở Việt Nam đèn này sẽ xanh mãi, tức là không đo gì cả. Thứ thực sự khan hiếm là số ngày, và TTFV đã bắt nó.
- *Sales cycle, tập trung doanh thu, NRR, % deal chết ở procurement:* chưa có sàn nào nên không có số trong 90 ngày; để lên dashboard chỉ làm đầy chỗ.

---

## Trạm 3 — Ngưỡng

| # | Đèn | 🟢 | 🟡 | 🔴 | Nguồn | Lý do (1 câu) · ngày kiểm tra nếu [BM] |
|---|---|---|---|---|---|---|
| 1 | TTFV | ≤ 8 ngày | 9–15 ngày | > 15 ngày | [MH] 1 | Sàn chỉ dùng hết 220 phiên đã cam kết trong tháng tính tiền đầu nếu bắt đầu luyện trước ngày thứ 8; quá 15 ngày thì lứa sales mới đã ra thị trường và sàn trả 32% hoá đơn cho phiên không dùng. |
| 2 | Tỷ lệ hoàn tất | ≥ 75% | 63,5–75% | < 63,5% | [MH] 2 | 63,5% là điểm GM rơi xuống 60% ở giá 39.000 ₫; 75% là mức mà ARPU $450 và GM 67,8% của mô hình đang đứng trên. |
| 3 | Pipeline coverage | ≥ 4× | 2–4× | < 2× | [TB] | Coverage cần = 1 ÷ win rate; 25% là số mượn từ ví dụ của lab Day 22, chưa phải của tôi, nên đây là baseline tạm — tính lại ngày 11/12/2026 khi 8 cơ hội đầu có kết quả. Dưới 2× thì chỉ đạt mục tiêu nếu thắng một nửa số sàn, điều tôi không có căn cứ để tin. |
| 4 | Chi phí AI / phiên hoàn tất | ≤ $0,085 | $0,085–0,194 | > $0,194 | [MH] 3 | Trên $0,194 thì GM < 60% ngay cả ở sàn chạy đủ 300 phiên; dưới $0,085 thì GM ≥ 60% kể cả ở sàn chỉ chạy đúng mức cam kết 220 phiên. |
| 5 | Phiên hoàn tất / sàn / tuần | ≥ 69 | 51–68 | < 51 | [MH] 4 | Dưới 51 phiên/tuần là sàn dùng không hết 220 phiên/tháng đã trả tiền — dấu hiệu sẽ không gia hạn; 69 là mức 300 phiên/tháng mà ARPU $450 giả định. |
| 6 | Pilot → trả tiền | ≥ 50% | 36–50% | < 36% | [BM] | ICONIQ *State of Go-to-Market 2026*: POC/trial → paid "roughly 50%" năm 2026, "about 36%" năm 2025, mẫu 150+ lãnh đạo GTM B2B software — **kiểm tra 09/10/2026**. Dưới 36% là kém cả mức trung bình năm ngoái; mẫu không phải SME Việt Nam nên chỉ là điểm bắt đầu. |
| 7 | Gross Margin từng sàn | ≥ 60% | 53–60% | < 53% | [MH] + [BM] | 60% là mức tôi dùng để đặt giá sàn và mức cam kết ở Day 22; 53% là GM dự phóng 2026 của công ty AI theo ICONIQ *State of AI 2026* (07/2026, ~300 lãnh đạo) — **kiểm tra 09/10/2026** — thấp hơn mức đó là tệ hơn mặt bằng của một ngành vốn đã mỏng biên. |
| 8 | CAC thực / sàn | ≤ $1.830 | $1.830–3.660 | > $3.660 | [MH] 5 | $3.660 là mức hoàn vốn đúng 12 tháng; $1.830 là mức giữ LTV:CAC ≥ 3 với vòng đời 18 tháng (ước tính). Mốc 12 tháng cho SMB lấy theo Bessemer qua HANDBOOK §8 mục 11 — đối chiếu nguồn thứ cấp 09/10/2026, tôi không mở sách gốc. |

Ghi chú về đèn 1: HANDBOOK gợi ý <30 / 30–60 / >60 ngày [TB]. Tôi siết hơn nhiều vì hợp đồng của tôi tính tiền theo tháng và việc cần làm chỉ có giá trị trong 15 ngày đầu của một lứa sales mới; ngưỡng 30 ngày sẽ để đèn xanh trong lúc sàn đã mất trọn tháng đầu.

### Phụ lục [MH] — phép tính

**[MH] 1 — TTFV**

```
Đầu vào: cam kết 220 phiên hoàn tất/tháng · sàn tham chiếu 300 phiên hoàn tất/tháng (ước tính)
         · sales mới cần 15 ngày đào tạo trước khi ra thị trường (phỏng vấn của nhóm P-067)
Tốc độ luyện khi đã chạy  = 300 ÷ 30                 = 10 phiên/ngày
Số ngày cần để dùng hết   = 220 ÷ 10                 = 22 ngày
TTFV tối đa               = 30 − 22                  = 8 ngày
Nếu TTFV = 15 ngày        : dùng được 15 × 10 = 150 phiên → thừa 70 phiên
                            = 70 × 39.000 = 2.730.000 ₫ = 31,8% hoá đơn tháng đầu
Kết quả → 🟢 ≤ 8 ngày · 🟡 9–15 ngày · 🔴 > 15 ngày
```

**[MH] 2 — Tỷ lệ hoàn tất phiên**

```
Đầu vào (Tab 1–2): P = $1,50 · N = 400 phiên bắt đầu · F = $89,43 cố định/tháng
         · v = $0,0985/phiên bắt đầu · s = $0,1619/phiên không hoàn tất · GM mục tiêu 60%
Điều kiện: F + N·v + (N − R)·s ≤ 0,4 · P · R
        ⇒ R/N ≥ (F/N + v + s) ÷ (0,4·P + s)
              = (0,2236 + 0,0985 + 0,1619) ÷ (0,60 + 0,1619) = 0,4840 ÷ 0,7619 = 63,5%
Kết quả → 🟢 ≥ 75% (mức mô hình giả định) · 🟡 63,5–75% · 🔴 < 63,5%
```

**[MH] 3 — Chi phí AI biến đổi / phiên hoàn tất**

```
Đầu vào: Cost/Job tối đa để GM ≥ 60% = 0,4 × $1,50 = $0,60
         · infra $70/tháng · giờ người (QA + kịch bản + hỗ trợ) $51,82/tháng ở 400 phiên bắt đầu
Sàn 300 phiên: phần không phải AI = (70 + 51,82) ÷ 300          = $0,4061/phiên
               ngân sách AI       = 0,60 − 0,4061               = $0,194
Sàn 220 phiên (đúng mức cam kết, 293 phiên bắt đầu):
               phần không phải AI = (89,43 + 11,87 + 11,87) ÷ 220 = $0,5145/phiên
               ngân sách AI       = 0,60 − 0,5145               = $0,085
Mô hình hiện ước: (21,10 + 2,11) ÷ 300 = $0,077 — chỉ cách ngưỡng xanh 10%
Kết quả → 🟢 ≤ $0,085 · 🟡 $0,085–0,194 · 🔴 > $0,194
```

**[MH] 4 — Phiên hoàn tất / sàn / tuần**

```
Đầu vào: cam kết 220 phiên/tháng · sàn tham chiếu 300 phiên/tháng (ARPU $450)
220 × 12 ÷ 52 = 50,8 phiên/tuần   → dưới mức này sàn trả tiền cho phiên không dùng
300 × 12 ÷ 52 = 69,2 phiên/tuần   → mức mà ARPU $450 giả định
Kết quả → 🟢 ≥ 69 · 🟡 51–68 · 🔴 < 51
```

Phí cam kết làm cho GM không xấu đi khi sàn dùng ít (150 phiên vẫn thu $330, GM ≈ 64%). Vì vậy đèn này không báo trước cho biên — nó báo trước cho việc sàn có gia hạn hay không.

**[MH] 5 — CAC thực / sàn**

```
Đầu vào: ARPU $450/tháng · GM 67,77% · payback tối đa 12 tháng (SMB) · vòng đời 18 tháng (ước tính)
Lãi gộp/sàn/tháng        = 450 × 0,6777        = $304,96
CAC tối đa theo payback  = 304,96 × 12         = $3.660
LTV                      = 304,96 × 18         = $5.489
CAC tối đa để LTV:CAC ≥ 3 = 5.489 ÷ 3          = $1.830
Đối chiếu: chia 20% doanh thu năm đầu cho partner = 0,2 × 5.400 = $1.080 (đề xuất, chưa đàm phán)
Kết quả → 🟢 ≤ $1.830 · 🟡 $1.830–3.660 · 🔴 > $3.660
```

---

## Trạm 4 — 5 luật quyết định

⏹ = luật dừng. Có 2 luật dừng (1 và 2).

1. ⏹ **NẾU** TTFV > 15 ngày **TRÊN** 2 sàn gần nhất **THÌ** dừng nhận sàn mới (kể cả sàn do partner giới thiệu) và cắt gói khởi động xuống 1 kịch bản cho 1 nhóm ≤10 sales, giữ như vậy đến khi có một sàn đạt TTFV ≤ 8 ngày **KHÔNG THÌ** không ký thêm sàn để kịp mục tiêu 3 sàn, và không dựng đủ 5 kịch bản riêng trước khi sàn có phiên hoàn tất đầu tiên.

2. ⏹ **NẾU** tỷ lệ hoàn tất < 63,5% **TRONG** 2 tuần liên tiếp **VÀ** mỗi tuần có ≥40 phiên bắt đầu **THÌ** dừng ký hợp đồng trả tiền mới, dành trọn tuần kế tiếp sửa đúng lý do bỏ dở đứng đầu trong log rồi đo lại trên 40 phiên **KHÔNG THÌ** không nới định nghĩa "phiên hoàn tất" (bỏ điều kiện ≥4 lượt nói, loại phiên lỗi khỏi mẫu số) và không tăng giá để bù biên.

3. **NẾU** chi phí AI biến đổi / phiên hoàn tất > $0,194 **TRONG** 2 tuần liên tiếp **VÀ** mỗi tuần có ≥50 phiên hoàn tất **THÌ** hạ trần thời lượng phiên từ 15 xuống 10 phút (`SESSION_MAX_SECONDS` 900 → 600) ngay trong tuần đó và áp đơn giá phiên vượt mới cho mọi hợp đồng ký sau ngày đó **KHÔNG THÌ** không dành thời gian tối ưu prompt hay đổi LLM rẻ hơn (LLM chỉ ~$0,004/phiên, TTS đắt gấp 8 lần) và không tăng giá với sàn đang trong hợp đồng.

4. **NẾU** pipeline coverage < 4× (dưới 12 sàn đủ điều kiện khi còn thiếu 3 sàn) **TRONG** 2 tuần liên tiếp **THÌ** tuần kế tiếp đặt 4 cuộc hẹn trực tiếp với người ký ở các sàn căn hộ sơ cấp cùng thành phố, và nếu đến 30/10/2026 Meey Group chưa hẹn gặp thì gửi đề xuất cho partner dự phòng (đơn vị đào tạo chứng chỉ môi giới) **KHÔNG THÌ** không bán dưới 39.000 ₫/phiên (giá sàn là 37.708 ₫, chỉ còn 3,4% khoảng trống), không tặng tháng miễn phí và không mở kênh thứ hai.

5. **NẾU** một sàn trả tiền có < 51 phiên hoàn tất/tuần **TRONG** 3 tuần liên tiếp kể từ ngày đạt TTFV **THÌ** trong 5 ngày làm việc đến sàn gặp trưởng nhóm, gắn 2 phiên/sales/tuần có tên từng người vào buổi giao ban sáng thứ Hai và gửi báo cáo tuần cho người ký **KHÔNG THÌ** không chào thêm kịch bản hay tài khoản cho sàn đó, và không dựng kịch bản mới theo yêu cầu của sàn đó cho tới khi vượt lại 51 phiên/tuần.

**Phản xạ sai mà mỗi vế KHÔNG THÌ chặn**

| Luật | Khi đèn đỏ tôi sẽ muốn làm gì | Vì sao sai với sản phẩm này |
|---|---|---|
| 1 | Ký thêm sàn cho đủ số | Mỗi sàn mới lại cần tôi dựng kịch bản và ngồi cạnh sales; thêm sàn vào lúc đang chậm chỉ làm mọi sàn chậm hơn. |
| 2 | Sửa định nghĩa cho số đẹp lên | Phiên bỏ dở vẫn đốt phút STT/TTS; đổi định nghĩa không đổi hoá đơn, chỉ làm tôi hết nhìn thấy nó. |
| 3 | Ngồi tối ưu token | Day 22 đã tính: LLM ~1% COGS. Tiết kiệm 50% token chỉ được ~$0,003/phiên. |
| 4 | Giảm giá, tặng tháng đầu | Giá đang chỉ cao hơn sàn 3,4%; giảm là bán lỗ biên. Sàn không mua vì chưa thấy kịch bản đúng dự án của họ, không phải vì 39.000 ₫. |
| 5 | Bán thêm, làm thêm kịch bản theo yêu cầu | Sàn chưa dùng hết thứ đã mua thì luôn có thêm một kịch bản nữa để chờ; bán thêm vào đó là mất cả hợp đồng. |

---

## Trạm 5 — Ghi chú cho cổng gác

Mốc ngày tính từ 12/10/2026 (ngày bắt đầu kế hoạch 90 ngày của Day 22): ngày 30 = 11/11/2026 · ngày 60 = 11/12/2026 · ngày 90 = 10/01/2027.

- **Ngày 30 — vì sao là tỷ lệ hoàn tất, và vì sao n ≥ 80.** Đây là con số ước tính mà cả ARPU, GM và breakeven của Day 22 đứng trên; đo được nó là học được nhiều nhất trong 30 ngày, và nó không phải doanh thu. 80 = 2 sàn × 10 sales × 4 phiên. Với n = 80, sai số chuẩn quanh 75% là √(0,75 × 0,25 ÷ 80) ≈ 4,8 điểm, nên nếu thật sự là 75% thì gần như không thể đo ra dưới 63,5%; còn nếu thật sự chỉ 55% thì xác suất lọt cổng oan khoảng 6%.
- **Ngày 60 — TTFV ≤ 15 ngày ở ≥2 sàn.** Cổng đặt ở biên đỏ chứ không ở biên xanh: qua cổng nghĩa là "chưa hỏng", chưa phải "đã tốt".
- **Ngày 90 — vì sao 2 sàn chứ không phải 3.** Mục tiêu Day 22 là 3 sàn; cổng là mức thấp nhất còn đáng đi tiếp. 2 = 50% (mức POC → paid của ICONIQ) × 4 pilot tôi cần bắt đầu trước 11/12. Một sàn có thể là quan hệ cá nhân; sàn thứ hai mới cho thấy lặp lại được.
- **FIX chỉ một lần.** Trượt cổng 30 lần đầu là FIX; đo lại trước 11/12 mà vẫn dưới 63,5% thì đơn vị tính tiền "phiên hoàn tất" sai → PIVOT sang tính theo phút luyện.
- **Kế hoạch Day 22 phải sửa một chỗ.** Day 22 ghi phỏng vấn 8 sàn trong Tháng 1. Với win rate 25% thì 3 sàn trả tiền cần 12 sàn đủ điều kiện; 8 là thiếu ngay từ kế hoạch.
