# Toolkit — Từ Evidence Đến Build Slice

Dùng sau khi nhóm đã có evidence. Mục tiêu là chốt một build slice đủ nhỏ cho Day 06.

## 1. Gom evidence thành cụm

Gom theo **workflow/pain**, không gom theo tên feature.

Ví dụ cụm của nhóm Ngũ Hổ Tướng (Vinpearl):
- "Tìm phòng cho gia đình (có trẻ em) gặp khó vì travel AI hiện có (Layla) không bóc tách được điều kiện."
- "Muốn hỏi thông tin tiện ích nội khu (hồ bơi, khu vui chơi) nhưng không có kênh nào hiểu context khách đang ở khách sạn nào."
- "Đổi ngày/hủy booking qua chat bị fail do luồng phức tạp, bị bắt gọi tổng đài."
- "Người dùng bị quăng link bắt tự đọc thay vì được hướng dẫn step-by-step."

## 2. Viết insight

```text
User [khách du lịch gia đình và khách đang lưu trú tại Vinpearl] không chỉ cần [một công cụ chat để thay thế thao tác click đặt phòng].
Họ thật ra cần [một người quản gia ảo (concierge) hiểu rõ ngữ cảnh của họ (đi với ai, đang ở khu nào) để hỗ trợ thông tin kịp thời],
vì [nhiều observation cho thấy việc ép đặt phòng qua chat rất rườm rà dễ lỗi, trong khi nhu cầu tra cứu dịch vụ nội khu và cá nhân hóa chuyến đi lại không được đáp ứng tốt].
```

## 3. Viết opportunity

```text
Cơ hội là dùng AI để [làm trợ lý Q&A theo ngữ cảnh booking có sẵn (conditional automation — AI tự trả lời trong phạm vi thông tin tĩnh) và trích xuất thông tin (số người, ngày) để điền sẵn form tìm kiếm (Augment)],
giúp user [rút ngắn thời gian thao tác và có cảm giác được chăm sóc tận tình khi lưu trú],
trong khi vẫn kiểm soát [rủi ro thao tác nhầm lẫn bằng cách dùng giao diện Native UI cho bước thanh toán/chọn phòng thay vì làm tất cả qua chat].
```

## 4. Chọn build slice

**Build slice đang xét:** AI Concierge & Booking Augmentation (Trợ lý lưu trú và Hỗ trợ bóc tách tìm kiếm)

| Câu hỏi | Đạt khi | Đánh giá |
|---|---|---|
| User cụ thể chưa? | Nói được ai dùng, trong bối cảnh nào. | Đạt. Khách đang tìm phòng gia đình & Khách đang ở tại resort. |
| Task đủ hẹp chưa? | Demo được trong 3-5 phút. | Đạt. Demo 2 luồng: (1) [Chính] Chat hỏi giờ mở cửa hồ bơi/xe điện theo context booking, (2) [Phụ] Chat để filter phòng. |
| AI decision rõ chưa? | AI gợi ý/tự làm một việc cụ thể. | Đạt. AI quyết định trích xuất parameter (ngày, người) và quyết định tra cứu Knowledge Base nội khu. |
| Failure path rõ chưa? | Có một case AI không chắc hoặc sai để test. | Đạt. Case: Yêu cầu đổi ngày booking phức tạp -> Bot xin lỗi và đưa Deep Link. |
| Có evidence không? | Có bằng chứng từ self-use/review/user/competitor. | Đạt. Đã có analog Layla AI, competitor Expedia/Agoda, và review Vinpearl trong Evidence Pack. |

**Đạt khi demo Day 06:** Concierge trả đúng ≥4/5 câu hỏi tiện ích test sẵn dựa context booking; 100% yêu cầu đổi/hủy/thanh toán rơi vào fallback Deep link. Cần chuẩn bị mock KB tiện ích + mock booking để có context demo.

## 5. Quyết định: giữ, giảm scope, hay đổi hướng?

| Tình huống của nhóm | Quyết định |
|---|---|
| Ý tưởng "Tự động Booking từ A-Z" quá rủi ro và rộng | Giữ domain, nhưng giảm scope xuống một flow: **Trợ lý Q&A nội khu** và **Hỗ trợ điền filter (Augmentation)**. |
| Rủi ro cao ở việc đổi/hủy/thanh toán qua chat | Không dùng Automate sâu. Chọn **Conditional automation**: AI tự trả lời Q&A tĩnh trong phạm vi hẹp + **Augment** điền form, mọi case rủi ro → **Fallback** (Deep link / Human-handoff). |

## 6. Câu chốt cuối

Điền câu này trước khi rời lớp:

```text
Dựa trên [các bằng chứng về việc user gặp khó khi filter phòng có trẻ em và thiếu context khi hỏi đáp tiện ích],
nhóm sẽ build [AI Concierge & Booking Augmentation],
cho [khách du lịch tự túc và khách gia đình],
để giải quyết [pain: thao tác rườm rà và không có kênh nào hiểu ngữ cảnh khách],
bằng cách AI [tự trả lời Q&A tiện ích dựa booking hiện tại trong phạm vi hẹp (conditional automation — luồng chính) và augment điền form filter (luồng phụ)],
và sẽ test failure path [khi user yêu cầu đổi ngày đặt phòng, bot sẽ xử lý rủi ro bằng cách đưa Deep link thay vì tự xử lý].
```

## 7. Backlog

Những thứ **không build trong Day 06**:

- Xử lý Thanh toán (Payment) toàn trình trực tiếp qua Chat.
- Luồng Đổi / Hủy booking (Cancel / Reschedule) tự động.
- Sinh lộ trình du lịch dài ngày phức tạp (Tour Itinerary Generation).
