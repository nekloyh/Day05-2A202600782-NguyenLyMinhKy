# Template — Thin SPEC Cuối Day 05

Thin SPEC không phải PRD đầy đủ. Đây là bản cam kết đủ rõ để sáng Day 06 nhóm build ngay.

## 1. Track, product/app và user

**Track:** Travel & Hospitality  
**Product/app thật:** Vinpearl  
**Hiện trạng:** Vinpearl CHƯA có trợ lý AI trong app; nhóm dùng Layla AI như một analog du lịch để học pattern và thấy giới hạn.  
**User cụ thể:** Khách gia đình tìm phòng (có kèm trẻ em) và Khách đang lưu trú tại resort cần hỏi thông tin.  
**Nhóm có phải user thật không? Nếu không, khác ở đâu?** Không hoàn toàn. Nhóm có chuyên môn tech nên khi chat thường dùng từ khóa rõ ràng, trong khi user thật thường dùng ngôn ngữ đời thường, mơ hồ và hay sai lỗi chính tả.

## 2. Evidence summary

| Evidence | Nguồn | User/pain nói lên điều gì? | SPEC phải đổi gì? |
|---|---|---|---|
| Vinpearl CHƯA có trợ lý AI; khách hỏi tiện ích nội khu (hồ bơi, xe điện) phải tự dò thủ công. | Review App Store/Play | Khách lưu trú bực vì tìm thông tin hoài không thấy. | Làm AI Concierge dùng booking hiện tại làm context để trả lời (luồng chính). |
| Layla AI (analog du lịch) không bóc tách được số lượng + độ tuổi trẻ em để lọc phòng, bắt user tự click. | Test Layla AI (analog) | Kể cả travel AI hiện có vẫn yếu khâu này, user mệt mỏi. | Dùng AI bóc tách text (Augment) điền sẵn form filter (luồng phụ). |
| Trên Layla, yêu cầu đổi ngày/hủy phòng qua chat bị từ chối/bắt gọi hotline. | Test Layla AI (analog) | Luồng nghiệp vụ rủi ro cao, AI không nên tự xử. | Đưa vào Failure Path, xử lý bằng Fallback Deep link. |

## 3. Pain statement

```text
User [Khách gia đình hoặc khách đang lưu trú] đang gặp khó ở [tra cứu tiện ích nội khu và lọc tìm phòng phức tạp],
vì [Vinpearl chưa có trợ lý AI nên khách phải tự dò thủ công; và các travel AI hiện có (analog Layla) vẫn không bóc tách được yêu cầu chi tiết như độ tuổi trẻ em, lại thiếu ngữ cảnh khách đang ở đâu],
dẫn tới [tốn thời gian, luồng giao tiếp đứt gãy, khách phải tự đọc link hoặc gọi tổng đài].
Bằng chứng chính là [review than phiền không tìm được thông tin nội khu, và self-test trên Layla cho thấy bot báo không hiểu / trả link chung chung khi gặp câu hỏi ghép nhiều điều kiện].
```

## 4. Build slice

```text
Cho [khách hàng] đang [lưu trú cần tư vấn tiện ích resort (chính), hoặc tìm phòng (phụ)],
prototype sẽ dùng AI để [LUỒNG CHÍNH: tự trả lời câu hỏi tiện ích dựa trên booking có sẵn; LUỒNG PHỤ: bóc tách thông tin chat để điền sẵn form filter],
tạo ra [output là câu trả lời Q&A đúng context, hoặc một form tìm kiếm đã được điền sẵn],
và xử lý [failure mode (vd yêu cầu thay đổi lịch rủi ro cao)] bằng [mitigation: cung cấp Deep link điều hướng tới màn hình quản lý Native].
```

**Đạt khi (demo Day 06):** Concierge trả lời đúng ≥4/5 câu hỏi tiện ích nội khu (test sẵn) dựa trên context booking; mọi yêu cầu đổi/hủy/thanh toán đều rơi vào fallback Deep link (không tự xử lý nhầm).

## 5. Auto/Aug decision

Chọn một:

- [ ] **Augmentation:** AI gợi ý/draft/phân loại, user quyết cuối.
- [x] **Conditional automation:** AI tự làm trong case hẹp; case mơ hồ/rủi ro chuyển người.
- [ ] **Automation:** AI tự quyết và tự hành động.

**Lý do chọn:** Concierge Q&A — AI tự trả lời trong phạm vi hẹp (thông tin tĩnh nội khu đã có trong KB). Filter — AI chỉ điền sẵn form, user bấm "Tìm kiếm" (augment). Mọi case rủi ro (đổi/hủy/thanh toán) AI KHÔNG tự xử mà đẩy Deep link/human, vì đặt sai phòng hay thanh toán nhầm gây rủi ro tài chính cao.  
**Human role:** Decider cho booking/thanh toán; AI chỉ tự quyết với câu hỏi thông tin tĩnh trong phạm vi hẹp.

## 6. Four paths

| Path | Prototype phải thể hiện gì? |
|---|---|
| Happy | (Chính) Khách đang lưu trú hỏi "hồ bơi mở mấy giờ?" → AI dựa booking hiện tại trả lời đúng giờ/khu resort khách đang ở. (Phụ) AI trích xuất đúng ngày, số người, độ tuổi trẻ em và điền sẵn form UI. |
| Low-confidence | Khách gõ "đi cùng con nhỏ", AI không rõ độ tuổi nên hỏi lại: "Dạ bé nhà mình năm nay mấy tuổi để em tìm phòng phù hợp ạ?". |
| Failure | Khách yêu cầu: "Hủy phòng tuần sau đi". AI nhận diện rủi ro cao, không tự xử lý mà hiển thị: "Việc hủy phòng có thể phát sinh phí, bạn thao tác trực tiếp tại [Link Quản lý Booking] nhé". |
| Correction | Khách bảo: "Không, đi 3 người lớn chứ không phải 2 lớn 1 nhỏ". AI nhận diện đính chính, tự động cập nhật lại biến (variables) và fill lại form UI ngay lập tức. |

## 7. Failure mode nguy hiểm nhất

```text
Nếu user [yêu cầu thay đổi ngày đi, hủy phòng, hoặc thanh toán trực tiếp qua chat],
AI có thể [failure: hiểu sai ý, thực hiện lệnh sai dẫn đến mất tiền hoặc mất phòng],
hậu quả là [impact: tổn thất tài chính cho user và khủng hoảng CSKH].
Prototype sẽ xử lý bằng [fallback: nhận diện intent rủi ro cao và đưa Deep link chuyển user về màn hình quản lý Native của app].
Owner kiểm thử path này là [Thành viên 4 - Test/failure path].
```

## 8. Owner plan cho sáng Day 06

| Thành viên | Việc phụ trách | Bằng chứng cần có trong repo |
|---|---|---|
| [Thành viên 1] | Research / evidence | Bổ sung link ảnh test Layla AI (analog) + screenshot review Vinpearl chứng minh khoảng trống. |
| [Thành viên 2] | SPEC | Hoàn thiện file Thin SPEC & cập nhật quyết định. |
| [Thành viên 3] | Prototype | (Chính) Bot Q&A concierge tra KB nội khu theo context booking; (phụ) extract entity điền form. Chuẩn bị mock KB tiện ích + mock booking data. |
| [Thành viên 4] | Test / failure path | Trigger thử luồng Hủy phòng để ép fallback. |
| [Thành viên 5] | Demo script / repo | Viết kịch bản demo 3 phút (mở đầu, happy path, failure). |
