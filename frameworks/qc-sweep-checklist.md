# QC Sweep Checklist

**Owner:** Thắng Trương + Khải Lâm | **Tạo:** 2026-05-04
**Review với Dung:** Trước khi chạy sweep thật đầu tiên

> **Đây là checklist của QC Engineer khi thực hiện sweep sản phẩm.**
> QA có checklist audit riêng: xem [QA Audit Checklist](qa-audit-checklist.md)
> Sự khác biệt: QC sweep = test trực tiếp sản phẩm | QA audit = kiểm tra evidence + quy trình

> **Hướng dẫn tạo checklist:** File này là skeleton. Thắng và Khải cần fill in domain-specific items cho từng loại sản phẩm dựa trên kinh nghiệm thực tế và top bugs lịch sử trong Redmine.
>
> **Deadline:** 19/05/2026

---

## Cách dùng

1. Xác định **loại sản phẩm** đang sweep → chọn đúng checklist section
2. Xác định **trigger** (routine release / override / post-incident)
3. Ưu tiên theo thứ tự trong checklist — không bỏ bước
4. Log kết quả vào Post-Sweep Report template

---

## Checklist chung — Áp dụng cho tất cả sản phẩm

> Luôn kiểm tra trước khi vào domain-specific checks

### Pre-sweep (5 phút)

- [ ] Build đang sweep là đúng version cần sweep (kiểm tra với Dev)
- [ ] STG environment đang up và healthy
- [ ] Test data đã chuẩn bị (account test, voucher test, v.v.)
- [ ] DoR evidence của ticket đã được review qua (SA pipeline attached?)
- [ ] Biết scope thay đổi: file/module nào đã thay đổi trong release này

### Security basics (áp dụng khi có auth/permission change)

- [ ] User không có quyền không truy cập được resource của user khác
- [ ] API không expose sensitive data trong response khi không cần
- [ ] Token / session expire đúng behavior
- [ ] Input validation: script injection, SQL không được accept

### Data integrity

- [ ] Số tiền / điểm / voucher không bị sai sau transaction
- [ ] Rollback đúng khi transaction fail giữa chừng
- [ ] Không có duplicate record khi submit nhiều lần
- [ ] Không mất data khi timeout hoặc network interrupt

---

## Checklist theo loại sản phẩm

### A. Consumer Apps (GotIt App, Voucher Link, GI Mini App, Cover Link, GI Web)

**Mục đích:** User-facing, financial transaction, high DAU

#### Core flows (luôn check)

- [ ] Tìm voucher / browse sản phẩm: hiển thị đúng, filter đúng
- [ ] Xem chi tiết voucher: thông tin đầy đủ, không sai giá / hạn sử dụng
- [ ] Mua voucher: checkout → payment → nhận voucher → verify trong "Kho"
- [ ] Sử dụng voucher: quy trình dùng voucher đúng, số dư sau khi dùng đúng
- [ ] Lịch sử giao dịch: hiển thị đúng, đủ thông tin
- [ ] Notification: push notification đúng event, đúng content

#### Financial critical (khi release liên quan đến payment/voucher)

- [ ] Số tiền hiển thị = số tiền thực tế bị trừ
- [ ] Voucher amount đúng sau khi apply discount
- [ ] Không thể dùng voucher đã expired hoặc đã used
- [ ] Refund / hoàn điểm đúng khi transaction fail
- [ ] Không thể double-spend voucher

#### UX / Regression

- [ ] App không crash trên critical flows
- [ ] Loading state hiển thị đúng (không blank screen)
- [ ] Error message rõ ràng khi fail (không lộ stack trace)

#### _(TODO: Thắng / Khải bổ sung dựa trên top 10 bugs lịch sử Consumer)_

---

### B. Campaign (Campaign Tool, Campaign CMS, Campaign Loyalty)

**Mục đích:** Campaign logic phức tạp, rule nhiều, financial (điểm / thưởng)

#### Campaign configuration

- [ ] Tạo campaign mới: tất cả field bắt buộc được validate đúng
- [ ] Rule phân bổ: logic đúng theo config (quota, tier, time window)
- [ ] Scheme / prize: số lượng đúng, không vượt quá quota
- [ ] Golive flow: campaign activate đúng thời gian, đúng scope

#### Campaign execution

- [ ] User tham gia campaign: eligibility check đúng
- [ ] Điểm / thưởng được cộng đúng sau action
- [ ] Không được nhận thưởng nhiều hơn limit (daily / total cap)
- [ ] Campaign hết quota: dừng đúng, không cộng thêm

#### Loyalty

- [ ] Điểm tích lũy đúng theo rule
- [ ] Điểm trừ đúng khi redeem
- [ ] History tracking đầy đủ
- [ ] Tier upgrade / downgrade logic đúng (nếu có)

#### _(TODO: Khải bổ sung dựa trên top 10 bugs Campaign lịch sử)_

---

### C. Core / Integration API (Biz API, POS API, SMS Gateway)

**Mục đích:** B2B API, contract với partners, no UI

#### API contract

- [ ] Request schema validation: required fields, data types, size limits
- [ ] Response schema: fields đầy đủ, format đúng, không thêm/bớt field unexpected
- [ ] HTTP status codes: 200/201 khi success, 4xx khi client error, 5xx khi server error
- [ ] Error messages: rõ ràng, không expose internal details

#### Business logic

- [ ] Authentication / API key validation đúng
- [ ] Rate limiting hoạt động (nếu có)
- [ ] Idempotency: gọi lại API cùng request không tạo duplicate
- [ ] Voucher redemption via API: deduct đúng, confirm đúng

#### Integration

- [ ] Bên thứ 3 gọi vào: webhook / callback nhận và xử lý đúng
- [ ] GotIt gọi ra bên thứ 3: handle timeout, handle error response đúng
- [ ] Không có data loss khi retry

#### _(TODO: Thắng bổ sung dựa trên API docs và top bugs Integration lịch sử)_

---

### D. BackOffice / Admin (nếu cần sweep — exception case)

> Tier 3 — Dev self-certify. Chỉ sweep khi có override trigger.

- [ ] CRUD operations đúng cho entity đang thay đổi
- [ ] Permission / role không bị bypass
- [ ] Bulk action (nếu có) không ảnh hưởng record ngoài scope
- [ ] Export / import data đúng format, không corrupt

---

## Post-sweep action

1. Điền **Post-Sweep Report** (template: `templates/report-post-sweep.md`)
2. Nếu phát hiện **DoR gap** lặp ≥2 lần → trigger Dev Feedback (template: `templates/report-dev-feedback.md`)
3. Cập nhật **checklist này** nếu phát hiện loại bug mới chưa có trong checklist

---

## Liên kết

- [QC Classification Matrix](qc-classification-matrix.md) — Tier nào cần sweep
- [Post-Sweep Report template](../templates/report-post-sweep.md)
- [Dev Feedback template](../templates/report-dev-feedback.md)
- [AI Subagent Guide](ai-subagent-guide.md) — Dùng SA4 Bug Hunter khi sweep sâu
