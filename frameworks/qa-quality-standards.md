# Quality Standards — GotIt PD

**Owner:** QA | **Tạo:** 2026-05-07
**Áp dụng:** Toàn bộ Dev + QC trong 3 Scrum Teams
**Review định kỳ:** Quarterly

> **Single source of truth** cho tất cả tiêu chuẩn chất lượng.
> Khi conflict với tài liệu khác, file này ưu tiên hơn.

---

## 1. Bug Severity — Định nghĩa thống nhất

| Severity | Tên      | Định nghĩa                                                     | Ví dụ GotIt                                            |
| -------- | -------- | -------------------------------------------------------------- | ------------------------------------------------------ |
| **S1**   | Critical | System down, data loss, security breach, sai số tiền           | Không mua được voucher, trừ sai tiền, data leak user   |
| **S2**   | Major    | Luồng chính broken, sai nghiệp vụ quan trọng                   | Không dùng được voucher đã mua, campaign tính sai điểm |
| **S3**   | Minor    | Luồng phụ bị ảnh hưởng, UX sai nhưng không ảnh hưởng tài chính | Filter không hoạt động, notification trễ               |
| **S4**   | Trivial  | UI cosmetic, typo, non-functional                              | Sai font, typo label, icon lệch nhẹ                    |

### Release action theo severity

| Severity | Dev được handover QC? | Release được phép? |
|---|---|---|
| S1 | ❌ Phải fix trước | ❌ Block — QA NO-GO |
| S2 | ❌ Phải fix trước | ❌ Block — QA NO-GO (hoặc GO WITH CONDITIONS nếu PM ack) |
| S3 | ✅ Có — log Redmine + Dev ack risk | ✅ Có thể release |
| S4 | ✅ Có — log Redmine | ✅ Có thể release |

### Bug SLA

| Severity | SLA fix                      | SLA verify                  |
| -------- | ---------------------------- | --------------------------- |
| S1       | ≤8 giờ làm việc (trong ngày) | QC/QA verify ngay sau fix   |
| S2       | ≤2 ngày làm việc             | QC/QA verify trong ngày fix |
| S3       | Trong sprint hiện tại        | QC verify khi fix xong      |
| S4       | Backlog — plan trong quarter | Khi nào fix thì verify      |

---

## 2. Code Quality Standards

> Đo bằng SonarQube. Dev đạt trước khi handover ticket.

| Tiêu chí | Threshold | Ghi chú |
|---|---|---|
| Unit test coverage | **≥90%** | Áp dụng cho code mới/changed trong ticket — không phải toàn project |
| Blocker issues | **0** | Không có exception |
| Critical issues | **0** | Không có exception |
| Security Hotspots | **0 Open** | Phải Resolved hoặc Accepted (có lý do) |
| Hardcoded credentials | **0** | API key, password, token |
| Debug statements | **0** | console.log, print debug trong production code |

> **Evidence:** Link SonarQube scan result đính kèm trong Redmine ticket hoặc PR.

---

## 3. Testing Standards

### TC coverage requirement theo tier

| Tier | Test types bắt buộc | Min coverage so spec |
|---|---|---|
| **Tier 1 — Critical** | Happy path + Negative (≥3) + Boundary + Integration + Security basics | >80% acceptance criteria |
| **Tier 2 — Standard** | Happy path + Negative (≥2) + Boundary | >60% acceptance criteria |
| **Tier 3 — Basic** | Happy path + Smoke | >40% acceptance criteria |

### TC format bắt buộc

Mỗi test case phải có đủ các field sau. Thiếu field → QA flag TC không hợp lệ.

| Field | Mô tả | Ví dụ |
|---|---|---|
| **ID** | Format: `[Product]-[Module]-[Số]` | `APP-VOUCHER-001` |
| **Title** | Mô tả behavior đang test | "Mua voucher thành công với số dư đủ" |
| **Preconditions** | Điều kiện cần có trước khi test | "User đã login, có số dư ≥ giá voucher" |
| **Steps** | Từng bước thực hiện | "1. Chọn voucher → 2. Nhấn Mua → 3. ..." |
| **Expected Result** | Kết quả mong đợi — cụ thể, có thể đo được | "Voucher xuất hiện trong Kho, số dư giảm đúng" |
| **Actual Result** | Kết quả thực tế khi chạy | Điền khi execute |
| **Status** | Pass / Fail / Blocked | Không được để trống |
| **Bug link** | Link Redmine (nếu Fail) | `redmine.gotit.vn/issues/XXXX` |

### SA pipeline evidence bắt buộc

| Step | Evidence tối thiểu | Format |
|---|---|---|
| SA0 | Context comment trong ticket | Comment 3–5 dòng: làm gì / thay đổi ở đâu / ảnh hưởng gì |
| SA1 | Clarification đã confirm với BA/PO | Screenshot + "Confirmed với [tên] ngày [date]" |
| SA2 | Risk matrix phân loại High/Medium/Low | Table hoặc list |
| SA3 | Danh sách scenarios + pass/fail result | List có result từng item |
| SA4 | Attack vectors đã thử + kết quả | List vectors + Pass/Fail |
| SA5 | RCA summary | Bắt buộc khi có S1/S2 bug, optional cho S3/S4 |

---

## 4. Definition of Done

### Ticket level — Ticket DONE khi đạt tất cả:

- [ ] Code đã được review và approve bởi ≥1 Dev khác
- [ ] SonarQube: ≥90% coverage, 0 Blocker, 0 Critical, 0 Security Hotspot open
- [ ] SA0–SA4 evidence đính kèm trong ticket (SA5 nếu có S1/S2 bug)
- [ ] Acceptance criteria được đánh pass/fail từng item
- [ ] Không có S1/S2 bug còn open
- [ ] S3/S4 nếu có: đã log Redmine, Dev ack risk bằng comment
- [ ] Đã test trên STG (không chỉ local)

### Release level — Release DONE khi đạt tất cả:

- [ ] Tất cả tickets trong release đạt ticket-level DoD
- [ ] QA Audit Report có mặt (Tier 1) hoặc Spot-check pass (Tier 2)
- [ ] QA decision = GO hoặc GO WITH CONDITIONS (conditions documented)
- [ ] Smoke test trên production pass sau deploy
- [ ] Monitor 24h không có error spike bất thường

---

## 5. Release Gate Standards

> QA là gate duy nhất trước production. Xem chi tiết: [QA Audit Checklist](qa-audit-checklist.md)

### Go/No-Go criteria

**NO-GO — bất kỳ một trong:**
- S1 bug còn open
- SonarQube Blocker hoặc Critical chưa clear
- Security Hotspot open chưa được review
- TC coverage gap >30% acceptance criteria
- QA Smoke test fail trên critical flow

**GO WITH CONDITIONS — tất cả conditions phải documented:**
- S2 bug còn open nhưng có workaround + PM/Dev Lead ack bằng văn bản
- TC gap <20% và covered bởi QA smoke test

**GO — tất cả điều sau đúng:**
- Không có S1/S2 open
- SonarQube standards met
- TC coverage gap <20%
- Smoke test pass

---

## 6. Documentation Standards

| Tài liệu | Lưu ở đâu | Cập nhật khi nào |
|---|---|---|
| Test cases | SharePoint — TC template | Trước khi bắt đầu test |
| SA evidence | Redmine ticket (comment/attachment) | Trong quá trình dev |
| Bug report | Redmine | Khi phát hiện bug |
| Post-sweep report | Gửi Dev Lead + PM trong ngày | Sau khi QC sweep xong |
| QA Audit Report | Gửi Dev Lead + PM + QC Lead | Sau khi QA audit xong |
| Lesson learned | BookStack | Sau mỗi release có incident |

---

## Liên kết

- [QA Role Definition](qa-role-definition.md) — Scope và trách nhiệm QA
- [QA Audit Checklist](qa-audit-checklist.md) — Checklist audit theo 3 layer
- [Dev Test DoR](dev-test-dor.md) — Điều kiện Dev handover
- [QC Classification Matrix](qc-classification-matrix.md) — Tier phân loại sản phẩm
- [QC Sweep Checklist](qc-sweep-checklist.md) — Checklist QC sweep
- [Release Checklist](release-checklist.md) — Checklist deploy production
