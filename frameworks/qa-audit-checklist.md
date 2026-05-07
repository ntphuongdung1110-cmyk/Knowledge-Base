# QA Audit Checklist

**Owner:** QA Team | **Tạo:** 2026-05-07
**Áp dụng:** Trước mỗi Tier 1 release + Spot-check Tier 2 hàng sprint

> **Nguyên tắc:** QA không test lại toàn bộ. QA kiểm tra **evidence** và **quy trình** để xác nhận QC và Dev đã làm đúng tiêu chuẩn.

---

## Cách dùng

1. Xác định **tier** sản phẩm → xác định depth audit cần thiết
2. Với Tier 1: chạy cả 3 Layer
3. Với Tier 2 (spot-check): Layer 1 + 2, bỏ Layer 3 nếu không có release
4. Ghi kết quả vào **QA Audit Report** (`templates/report-qa-audit.md`)

---

## Pre-audit (5 phút)

- [ ] Xác định đúng build/version đang audit
- [ ] Lấy danh sách tickets trong release/sprint này từ Redmine
- [ ] Xác định scope thay đổi: module nào, feature nào
- [ ] Có ticket nào có override trigger (financial, migration, incident)? → audit kỹ hơn

---

## Layer 1 — Dev Evidence Audit

> Kiểm tra những gì Dev phải cung cấp theo DoR. Không cần hỏi Dev — evidence phải tự nói lên được.

### A. Requirement Coverage

- [ ] **SA0 comment** có trong ticket? Dev có tóm tắt đúng scope không? (3–5 dòng)
- [ ] **SA1 evidence**: Câu hỏi với BA/PO đã có confirmation không?
  - Nếu không có SA1 evidence → flag: Dev skip hoặc spec đã rõ sẵn?
- [ ] **Acceptance criteria** trong ticket được đánh pass/fail từng item rõ ràng chưa?
- [ ] Có dấu hiệu Dev implement thứ gì ngoài scope spec không? (scope creep check)

### B. Test Coverage — SA3 Evidence

So sánh spec ↔ SA3 test scenarios:

- [ ] Happy path có trong danh sách?
- [ ] Ít nhất 3 negative cases (input sai, thiếu field, sai type)?
- [ ] Boundary conditions (null, empty, min, max, âm số)?
- [ ] Mỗi **High risk** từ SA2 có ít nhất 1 test scenario tương ứng?
- [ ] Integration points có được test không (nếu có external API)?

**QA Gap Scan:** Đọc spec → liệt kê scenarios còn thiếu trong SA3:

| # | Case còn thiếu | Severity | Action |
|---|---|---|---|
| | | | |

### C. SonarQube Compliance

- [ ] Code coverage của code mới/changed **≥90%**?
- [ ] **0 Blocker** issue?
- [ ] **0 Critical** issue?
- [ ] Security Hotspots: đã được review và mark Resolved/Accepted (không để Open)?
- [ ] Link SonarQube scan được đính kèm trong ticket hoặc PR?

### D. Security Baseline

> Chỉ check các mục có liên quan đến thay đổi trong release này

- [ ] Không có hardcoded credentials, API keys, passwords trong code (SonarQube Security check)
- [ ] Input validation tại API boundary (nếu có new/changed endpoint)?
- [ ] Auth/permission không bị bypass (nếu có auth-related change)?
- [ ] SQL injection prevention: parameterized queries (nếu có DB query change)?
- [ ] XSS prevention: output encoding (nếu có rendering change)?
- [ ] Sensitive data không bị log hoặc expose trong API response?

---

## Layer 2 — QC Work Audit

> Chỉ khi process vẫn có QC Engineer thực hiện sweep. Kiểm tra evidence của QC — không test lại.

### A. TC Format Compliance

- [ ] Test cases theo đúng **template chuẩn** trên SharePoint?
- [ ] Mỗi TC có đủ: ID, Title, Preconditions, Steps, Expected Result, Actual Result, Status?
- [ ] Status (Pass/Fail/Blocked) được đánh cho **mọi TC** — không bỏ trống?
- [ ] Bug tickets được link đúng vào TC/story trong Redmine?
- [ ] Retest evidence có không? (sau khi Dev fix, QC confirm pass)

### B. TC Coverage vs Spec

So sánh spec (acceptance criteria + requirements) ↔ TC list:

- [ ] Mỗi acceptance criteria có ít nhất 1 TC tương ứng?
- [ ] Edge cases quan trọng có TC? (concurrent, timeout, rollback)
- [ ] Negative/failure scenarios có TC?
- [ ] Nếu có API: API contract test có không?

**QA Coverage Gap Scan:** Đọc spec → đọc TC list → liệt kê cases trong spec nhưng không có TC:

| # | Spec case không có TC | Rủi ro | Đề xuất |
|---|---|---|---|
| | | | |

> Gap rate = số case thiếu / tổng case trong spec × 100%
> **Threshold:** Gap rate >20% → flag, cần bổ sung TC trước release

### C. QC Process Compliance

- [ ] QC test trên STG (không chỉ local)?
- [ ] DoR gap (nếu có) đã được ghi nhận và flag cho Dev Lead?
- [ ] Post-sweep report đã gửi trong ngày sweep xong?

---

## Layer 3 — QA Smoke Test (Staging)

> Chỉ cho Tier 1 release. QA tự thực hiện — không phải full regression, chỉ critical flows.

### Critical Flow Check

Chọn flows liên quan đến thay đổi trong release này:

**Consumer flows (GotIt App, Voucher Link, GI Mini App):**
- [ ] Browse → xem chi tiết voucher: thông tin đúng
- [ ] Checkout → payment → nhận voucher: end-to-end pass
- [ ] Sử dụng voucher: amount deduct đúng, không double-spend
- [ ] Lịch sử giao dịch: hiển thị đúng

**Campaign/Loyalty flows:**
- [ ] User tham gia campaign: eligibility đúng
- [ ] Điểm/thưởng cộng đúng sau action
- [ ] Không vượt quota/cap

**API flows (B2B):**
- [ ] Auth validation hoạt động
- [ ] Core request–response đúng schema
- [ ] Idempotency: gọi lại không tạo duplicate

### Sanity Check

- [ ] Không có broken screen hoặc 500 error trên critical flows
- [ ] Error state xử lý đúng — không lộ stack trace ra user
- [ ] Load time cảm quan ổn (<3 giây cho main actions)
- [ ] Không có regression rõ ràng trên flows không liên quan đến release

---

## Go/No-Go Decision

Sau khi hoàn thành tất cả layer liên quan:

| Tiêu chí | Status | Ghi chú |
|---|---|---|
| S1/S2 bug: không có open | ✅ / ❌ | |
| SonarQube: ≥90% coverage | ✅ / ❌ | |
| SonarQube: 0 Blocker, 0 Critical | ✅ / ❌ | |
| TC coverage gap: <20% | ✅ / ❌ | |
| Security issues: resolved | ✅ / ❌ | |
| QA Smoke test: pass | ✅ / ❌ | |

**Quyết định:** `🟢 GO` / `🔴 NO-GO` / `🟡 GO WITH CONDITIONS`

**NO-GO triggers (bất kỳ một trong các điều sau):**
- S1 bug còn open
- SonarQube Blocker hoặc Critical chưa clear
- Security issue nghiêm trọng chưa resolve
- TC gap >30% spec cases
- Smoke test fail trên critical flow

---

## Post-audit actions

1. Điền **QA Audit Report** (`templates/report-qa-audit.md`) → gửi Dev Lead + PM
2. Gap về TC/process → feedback cho QC Lead (không blame cá nhân)
3. Pattern lặp lại ≥2 sprint → trigger process improvement
4. Update checklist này nếu phát hiện loại gap mới chưa cover

---

## Liên kết

- [QA Role Definition](qa-role-definition.md) — Scope và trách nhiệm QA
- [Dev Test DoR](dev-test-dor.md) — Evidence QA kiểm tra ở Layer 1
- [QC Classification Matrix](qc-classification-matrix.md) — Tier nào audit ở mức nào
- [QC Sweep Checklist](qc-sweep-checklist.md) — Checklist riêng của QC (khác với QA audit)
- [QA Audit Report template](../templates/report-qa-audit.md)
