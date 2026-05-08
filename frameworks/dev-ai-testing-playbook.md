# Dev AI Testing Playbook — SA0 đến SA5

**Owner:** Dung Nguyễn | **Tạo:** 2026-05-04
**Áp dụng cho:** Tất cả Dev trong 3 Scrum Teams
**Mục đích:** Dev tự test chất lượng cao trước khi handover — không cần QC hướng dẫn từng lần

---

## Tổng quan pipeline

```
Nhận ticket
    │
    ▼
[SA0] Context Setup — Xác lập context, hiểu ticket trước khi dùng AI
    │
    ▼
[SA1] Requirement Clarifier — Phát hiện mâu thuẫn, assumption ẩn trong spec
    │
    ▼
[SA2] Risk Scout — Xác định vùng nguy hiểm, edge cases
    │
    ▼
[SA3] Test Designer — Sinh test scenarios theo risk
    │
    ▼
    Execute test + log evidence
    │
    ▼
[SA4] Bug Hunter — Đào sâu những gì dễ bỏ sót
    │
    ▼
[SA5] RCA Assistant — Chỉ dùng khi tìm thấy bug S1/S2
    │
    ▼
Hoàn thành DoR checklist → Handover QC
```

**Thời gian ước tính:** 1–2 giờ cho ticket thông thường | 3–4 giờ cho feature lớn

---

## SA0 — Context Setup

**Mục đích:** Trước khi dùng bất kỳ AI subagent nào, Dev cần xác lập context rõ ràng — ticket hiểu đúng chưa, scope thay đổi là gì, ảnh hưởng đến đâu.

**Khi nào dùng:** Ngay đầu tiên khi nhận ticket, trước SA1.

**Các bước thực hiện (không cần AI):**

1. **Đọc ticket** — Acceptance criteria, mô tả, comment của BA/PM
2. **Trả lời 3 câu hỏi bằng lời của mình:**
   - "Ticket này yêu cầu tôi làm gì?" — nếu không trả lời được → chưa đủ hiểu, hỏi BA trước
   - "Code tôi sẽ thay đổi ở đâu?" — liệt kê file/module/API endpoint cụ thể
   - "Thay đổi này có thể ảnh hưởng đến luồng nào khác không?" — nghĩ về dependency
3. **Viết summary ngắn vào comment ticket** (3–5 dòng):
   ```
   SA0 Context:
   - Làm gì: [1 câu]
   - Thay đổi ở: [file/module/API]
   - Có thể ảnh hưởng đến: [module khác / luồng liên quan]
   - Điểm chưa rõ cần hỏi: [nếu có]
   ```

**Tại sao quan trọng:** SA1–SA5 chỉ hiệu quả nếu context input chính xác. SA0 không tốn thời gian nhưng loại bỏ phần lớn lý do AI sinh ra test case không relevant.

**Evidence cần attach:** Comment SA0 trong ticket (3–5 dòng như format trên)

---

## SA1 — Requirement Clarifier

**Mục đích:** Đọc spec và tìm ra những chỗ mơ hồ, mâu thuẫn, thiếu định nghĩa — trước khi bắt tay test.

**Khi nào dùng:** Ngay sau khi nhận ticket, TRƯỚC khi viết test plan.

**Prompt cho Dev:**

```
## Context
[Mô tả ngắn tính năng: tên feature, luồng chính, liên quan đến module nào]
Sản phẩm: [GotIt App / Campaign / Voucher Service / ...]

## Task
Phân tích tài liệu đặc tả dưới đây và:
1. Liệt kê tất cả assumption ẩn (những điều spec giả định nhưng không nói rõ)
2. Phát hiện mâu thuẫn logic giữa các requirement
3. Điểm thiếu thông tin — không biết thì không test được
4. Câu hỏi cần hỏi BA/PO TRƯỚC khi code/test

## Constraints
- Ngôn ngữ: Tiếng Việt
- Format: Bullet points, nhóm theo loại
- Focus: GotIt business context (voucher, e-gift, điểm, B2B)

## Spec
[Paste toàn bộ acceptance criteria / mô tả ticket ở đây]
```

**Output mong đợi:** List câu hỏi + điểm mâu thuẫn
**Hành động tiếp theo:** Hỏi BA/PO để làm rõ TRƯỚC khi code. Đừng assume.
**Evidence cần attach:** Screenshot kết quả + note "Đã confirm với [BA/PO tên] ngày [date]"

---

## SA2 — Risk Scout

**Mục đích:** Vẽ bản đồ rủi ro — biết test cái gì quan trọng nhất, không test dàn trải.

**Khi nào dùng:** Sau khi spec đã rõ (SA1 xong), trước khi viết test cases.

**Prompt cho Dev:**

```
## Context
Feature: [Tên feature]
Sản phẩm: [Tên product]
Business context: [Feature này làm gì, ai dùng, ảnh hưởng gì nếu sai]
Thay đổi: [Mô tả ngắn code changes — endpoint nào, module nào, DB nào]

## Task
Identify risk matrix mở rộng:
1. Business risk — sai nghiệp vụ, sai tính toán, sai rule
2. Financial risk — ảnh hưởng tiền/voucher/điểm: mất tiền, nhân đôi, âm số dư
3. Data risk — corrupt, mất, duplicate, sai user
4. Security risk — bypass auth, injection, data leak
5. Integration risk — API bên thứ 3, module khác trong hệ thống
6. Edge cases — boundary, concurrent request, timeout, network fail

## Output
Risk matrix: High / Medium / Low — kèm lý do ngắn

## Constraints
- Tập trung vào risk thực tế, không phải lý thuyết
- High = nếu xảy ra sẽ ảnh hưởng user hoặc revenue ngay lập tức
```

**Output mong đợi:** Risk matrix phân loại High/Medium/Low
**Hành động tiếp theo:** Test High risk TRƯỚC, Medium sau, Low nếu còn thời gian
**Evidence cần attach:** Risk matrix đã review — note rõ High nào đã test

---

## SA3 — Test Designer

**Mục đích:** Từ risk matrix → sinh test scenarios đủ coverage, không bỏ sót.

**Khi nào dùng:** Sau SA2, khi đã có risk list.

**Prompt cho Dev:**

```
## Context
Feature: [Tên feature]
Risk list từ SA2: [Copy risk matrix ở đây]

## Task
Sinh test scenarios theo risk-based approach:
1. Test scenarios cho từng High risk (bắt buộc)
2. Happy path — luồng chính pass
3. Negative cases — input sai, thiếu field, sai type
4. Boundary conditions — giá trị min/max, 0, null, âm
5. Integration points — mock fail API bên thứ 3 xem hệ thống xử lý thế nào
6. Concurrent scenarios — 2 user làm cùng lúc (nếu relevant)

## Format
Nhóm theo risk level: High → Medium → Low
Mỗi scenario: [ID] [Mô tả] [Expected result]

## Constraints
- Không sinh quá nhiều — tập trung vào risk thực tế
- Scenario phải test được, không phải lý thuyết
```

**Output mong đợi:** Danh sách test scenarios đã phân nhóm
**Hành động tiếp theo:** Execute từng scenario, record pass/fail
**Evidence cần attach:** Danh sách scenarios + kết quả thực hiện

---

## SA4 — Bug Hunter

**Mục đích:** Tìm những bug khó thấy mà happy path testing không bắt được.

**Khi nào dùng:** Sau khi đã execute test scenarios từ SA3 và thấy ổn — SA4 là "lớp phòng thủ thứ 2".

**Prompt cho Dev:**

```
## Context
Feature: [Tên feature]
Đã test: [Mô tả những gì đã test từ SA3]
Hệ thống: [Tech stack ngắn gọn nếu biết — Node/Java, MySQL, Redis, ...]

## Task
Gợi ý cách "phá" hệ thống để tìm bug khó thấy:
1. Attack vectors chưa thử (authentication bypass, privilege escalation)
2. Race condition — 2 request đồng thời vào cùng resource
3. State manipulation — thay đổi state bất thường giữa chừng
4. Data pollution — inject dữ liệu lạ (emoji, script, ký tự đặc biệt, số âm)
5. Timeout / network scenarios — chậm, mất kết nối giữa transaction
6. Rollback scenarios — transaction fail giữa chừng thì data như thế nào

## Constraints
- Tập trung vào GotIt context: voucher redemption, điểm, campaign rule, payment
- Ưu tiên scenarios có thể thực hiện được trên STG
```

**Output mong đợi:** List "attack vectors" cụ thể để thử
**Hành động tiếp theo:** Thử từng vector, log bug nếu phát hiện
**Evidence cần attach:** List vectors đã thử + kết quả

---

## SA5 — RCA Assistant

**Mục đích:** Khi tìm ra bug — phân tích root cause để fix đúng, không chỉ fix triệu chứng.

**Khi nào dùng:** Khi phát hiện bug S1 hoặc S2 trong quá trình dev/test. KHÔNG bắt buộc cho S3/S4.

**Prompt cho Dev:**

```
## Context
Hệ thống: [Module liên quan]

## Bug
Mô tả: [Bug xảy ra khi nào, ở đâu]
Steps to reproduce: [1. ... 2. ... 3. ...]
Expected: [Hành vi đúng phải là gì]
Actual: [Hành vi sai đang thấy]
Logs: [Paste error log nếu có]

## Task
1. Liệt kê top 3 giả thuyết root cause (theo xác suất cao → thấp)
2. Bước reproduce từng giả thuyết để xác nhận
3. Hướng fix đề xuất cho từng case
4. Prevention: sau này test case nào sẽ bắt được loại bug này?

## Constraints
- Tập trung vào code-level analysis, không phải process
- Fix phải giải quyết root cause, không phải workaround
```

**Output mong đợi:** Root cause analysis + hướng fix
**Hành động tiếp theo:** Fix theo đúng root cause, viết regression test cho scenario đó
**Evidence cần attach:** RCA summary trong ticket comment

---

## Checklist hoàn thành pipeline

Trước khi tạo PR hoặc request QC handover:

- [ ] SA0: Context comment đã viết vào ticket (3–5 dòng)
- [ ] SA1: Đã clarify tất cả ambiguity với BA/PO — screenshot attached
- [ ] SA2: Risk matrix đã review — High risks xác định rõ
- [ ] SA3: Test scenarios đã execute — pass/fail recorded
- [ ] SA4: Ít nhất 3 attack vectors đã thử
- [ ] SA5: Chỉ khi có bug S1/S2 — RCA đã làm
- [ ] Unit test ≥90% coverage cho code mới/changed (SonarQube)
- [ ] DoR checklist đã điền đủ

---

## Câu hỏi thường gặp

**Q: SA mất bao lâu?**
SA1+SA2+SA3: ~30–45 phút cho ticket bình thường. SA4: ~20 phút thêm. SA5: khi có bug S1/S2 thì cần làm kỹ.

**Q: Nếu AI output sai thì sao?**
Luôn review output AI — không tin blindly. Nếu AI miss risk quan trọng mà bạn biết → thêm vào, note lại prompt chưa cover được gì.

**Q: Nếu không có spec rõ ràng?**
SA1 output sẽ show ngay điều này. Dừng lại, hỏi BA/PO trước khi code tiếp.

---

## Liên kết

- [Dev Test DoR](dev-test-dor.md) — Checklist hoàn chỉnh trước khi handover
- [AI Subagent Guide](ai-subagent-guide.md) — Phiên bản đầy đủ dành cho QC
- [QC Classification Matrix](qc-classification-matrix.md) — Sản phẩm của bạn thuộc tier nào

---

