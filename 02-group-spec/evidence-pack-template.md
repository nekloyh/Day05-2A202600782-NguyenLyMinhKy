# Template — Evidence Pack

Nộp kèm thin SPEC cuối Day 05.

## 1. Nhóm và track

- **Tên nhóm:** Ngũ Hổ Tướng.
- **Track:** Travel & Hospitality.
- **Product/app đã chọn:** Vinpearl.
- **Build slice đang nghĩ:** AI Concierge (luồng chính — trợ lý Q&A theo context lưu trú) + Booking filter augmentation (luồng phụ).
- **Hiện trạng:** Vinpearl chưa có trợ lý AI trong app; nhóm dùng Layla AI làm analog để học pattern và thấy giới hạn.

## 2. Self-use / analog evidence (Layla AI)

Vinpearl chưa có trợ lý AI nên không có chatbot để "tự dùng". Nhóm test **Layla AI** như một analog du lịch để học pattern và ghi lại điểm gãy; khoảng trống thật của Vinpearl được đối chiếu qua review ở mục 3.

| Observation (trên Layla AI) | Screenshot / link | Path liên quan | Điều học được |
|---|---|---|---|
| Hỏi: "Resort đang ở có khu vui chơi cho bé không?" -> Q&A khá tốt nhưng bị **giới hạn lượt hỏi** và không gắn được context resort cụ thể (app bên thứ ba). | https://layla.ai/en/chat/01KT6F4S2AFEM00EZHM4TY75X9/trip/01KT6F7YATZ47A4V7N5XFC1PXY | Happy (Concierge — luồng chính) | Concierge Q&A khả thi; điểm cộng của ta là gắn **context booking thật** của Vinpearl. |
| Gõ: "Tìm phòng cho gia đình 4 người, có trẻ em 5 tuổi ở Phú Quốc" -> Bot trả link chung chung, không tự điền filter số người/trẻ em. | https://layla.ai/en/chat/01KT6FC1ESXRR5D1MSXTX707Y9/trip/01KT6F7YATZ47A4V7N5XFC1PXY | Happy (Filter — luồng phụ) | Bot chưa bóc tách (extract) được entity phức tạp để chuyển thành bộ lọc (filter) trên UI. Cần kết hợp Chat + UI. |
| Yêu cầu: "Đổi ngày đặt phòng cho booking XYZ" -> Bot báo không hiểu hoặc yêu cầu gọi Hotline. | https://layla.ai/en/chat/01KT6F4S2AFEM00EZHM4TY75X9/trip/01KT6F7YATZ47A4V7N5XFC1PXY | Failure | Flow thay đổi/hủy booking có risk cao, bot chưa dám xử lý. Cần human-handoff hoặc Deep link rõ ràng đến trang quản lý booking. |

## 3. User / review / social evidence

Nguồn có thể là review App Store/Play, group, comment, phỏng vấn nhanh, hoặc nguồn public khác.

*(Các quote dưới đây là paraphrase từ review/cộng đồng; nhóm sẽ bổ sung link gốc khi có.)*

| Quote / review / observation | Nguồn | User là ai? | Pain/failure mode |
|---|---|---|---|
| "App đặt phòng tiện nhưng muốn hỏi giờ mở cửa hồ bơi hay xe điện thì tìm hoài không thấy, chẳng có chỗ nào hỏi nhanh." | App Store / Play Store Review | Khách đang lưu trú tại Vinpearl | Không có kênh Q&A theo context — khách phải tự dò, không biết hỏi ở đâu. |
| "Muốn đặt combo vé máy bay và khách sạn nhưng thao tác rườm rà, hỏi support thì chỉ được gửi cái link bắt tự đọc." | Group Review Du Lịch Facebook | Khách du lịch tự túc (FIT) | Thiếu dẫn dắt step-by-step, không có trợ lý; quăng link gây đứt gãy UX. |
| "Đặt phòng có trẻ em thì báo lỗi không đủ giường mà không biết chọn hạng phòng nào, chẳng có ai tư vấn." | Group Cộng đồng Vinpearl | Gia đình có con nhỏ | Thiếu tư vấn/gợi ý option thay thế (VD: thêm extra bed). |

## 4. Competitor / analog evidence

| App / mô hình tham khảo | Họ xử lý task này thế nào? | Pattern học được | Có áp dụng trong 1 ngày không? |
|---|---|---|---|
| **Expedia (Tích hợp ChatGPT)** | Cho phép chat tự nhiên, AI tự nhận diện khách sạn được nhắc đến và lưu vào list "Saved". | Conversational Search -> Save to UI. Không ép user book trực tiếp qua chat. | Có, tách biệt phần Chat và phần UI hiển thị kết quả. |
| **Agoda** | Không dùng bot thuần, dùng Dynamic UI/Form filter cực mạnh và suggest thông minh. | Với tác vụ booking, UI truyền thống hiệu quả hơn Chat. Chat chỉ nên làm Support/Concierge. | Có, thay vì làm bot đặt phòng, hãy làm bot "Tư vấn chọn phòng". |

## 5. Evidence -> Insight

```text
Evidence nổi bật nhất:
User cực kỳ thiếu thông tin tiện ích nội khu khi đang lưu trú (Vinpearl chưa có trợ lý AI để hỏi), và chật vật khi yêu cầu booking phức tạp (có trẻ em) — ngay cả travel AI hiện có (analog Layla) cũng chỉ biết "quăng link".

Insight:
User không chỉ gặp [vấn đề về thao tác đặt phòng].
Thật ra họ cần [một người quản gia (concierge) hiểu ngữ cảnh: biết họ đi mấy người để tư vấn phòng, biết họ đang ở khu nào để báo giờ xe điện/hồ bơi].

Opportunity:
AI có thể giúp bằng cách [trả lời Q&A dựa trên context booking hiện tại của khách (Conditional automation — AI tự trả lời trong phạm vi thông tin tĩnh đã có)] và [tự động trích xuất yêu cầu (số người, địa điểm) để fill sẵn vào form tìm kiếm (Augment booking)].
```

## 6. Evidence đổi SPEC như thế nào?

- [ ] Đổi user chính.
- [ ] Đổi pain statement.
- [x] Đổi build slice.
- [x] Đổi Auto/Aug decision.
- [ ] Đổi 4 paths.
- [x] Đổi failure mode.
- [ ] Đổi owner/test plan.

Ghi rõ 1-2 thay đổi quan trọng:

```text
Trước evidence, nhóm định: 
Xây dựng một con Bot có thể tự động hoàn tất việc đặt phòng (Booking) từ A-Z hoàn toàn qua giao diện chat (Automate).

Sau evidence, nhóm đổi thành: 
Luồng CHÍNH = AI Concierge: bot lấy context booking để trả lời câu hỏi tiện ích nội khu (conditional automation, phạm vi hẹp). Luồng PHỤ = Booking filter augmentation: bot bóc tách entity (ngày, người, tuổi) và đẩy ra UI Native, user bấm "Tìm kiếm". Đổi/hủy/thanh toán không làm qua chat — đẩy Deep link Native UI.

Lý do: 
Việc thanh toán và chọn phòng qua Chat UX rất tệ và dễ lỗi (Failure cao). Việc kết hợp giữa Chat (để lấy intent) và Native UI (để hiển thị và thao tác) như Expedia làm sẽ mang lại trải nghiệm mượt mà và khả thi hơn.
```
