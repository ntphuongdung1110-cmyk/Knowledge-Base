# Template — QA Audit Report

> Gửi: Dev Lead + PM + QC Lead | Sau mỗi GO/NO-GO decision | Owner: QA
> Tier 1: gửi trong ngày audit xong | Tier 2: gửi cuối tuần (spot-check batch)

---

## QA Audit Report — [Tên sản phẩm / Sprint] | [DD/MM/YYYY]

**QA Auditor:**
**Tier:** Tier 1 / Tier 2
**Trigger:** Pre-release / Spot-check / Post-incident
**Build / Sprint:** ___

---

### 1. Scope audit

| | Chi tiết |
|---|---|
| Ticket | #[ID] — [Title] |
| Layer thực hiện | L1 + L2 + L3 (Tier 1) / L1 + L2 (Tier 2) |
| Thời gian audit | ___ giờ |
| Link L1 comment | [Redmine comment URL] |
| Link L2 comment | [Redmine comment URL] |
| Link L3 comment | [Redmine comment URL] |

---

### 2. Layer 1 — Dev Evidence

| # | Hạng mục | Status | Gap cụ thể |
|---|---|---|---|
| 1 | SA0 — Context comment | ✅ Pass / ❌ Fail / ⚠️ Partial | |
| 2 | SA1 — Requirement clarification | ✅ / ❌ / ⚠️ | |
| 3 | SA2 — Risk matrix | ✅ / ❌ / ⚠️ | |
| 4 | SA3 — Test scenarios + kết quả | ✅ / ❌ / ⚠️ | |
| 5 | SA4 — Security check | ✅ / ❌ / ⚠️ | |
| 6 | SA5 — RCA (khi có S1/S2) | ✅ / ❌ / N/A | |
| 7 | SonarQube link | ✅ / ❌ | |
| 8 | Coverage ≥ 90% | ✅ / ❌ | |
| 9 | Blocker + Critical = 0 | ✅ / ❌ | |
| 10 | Security Hotspot = 0 Open | ✅ / ❌ | |
| 11 | AC pass/fail rõ ràng | ✅ / ❌ / ⚠️ | |

**Coverage gap phát hiện (nếu có):**

| # | Case còn thiếu trong SA3 | Ticket | Risk level |
|---|---|---|---|
| | | | |

**SonarQube detail (nếu fail):**
- Coverage: __% (threshold: ≥90%)
- Blocker: __ (threshold: 0)
- Critical: __ (threshold: 0)
- Security Hotspots open: __

---

### 3. Layer 2 — QC Work Audit

> Bỏ qua nếu không có QC trong process (Dev self-certify tiers)

| Hạng mục | Status | Gap cụ thể |
|---|---|---|
| TC format compliance | ✅ / ❌ / ⚠️ | |
| TC coverage vs spec | ✅ / ❌ / ⚠️ | |
| Pass/Fail completeness | ✅ / ❌ / ⚠️ | |
| QC process compliance | ✅ / ❌ / ⚠️ | |

**TC coverage gap (nếu có):**

| # | Spec case không có TC | Rủi ro | Đề xuất |
|---|---|---|---|
| | | | |

**TC gap rate:** __% (gap cases / total spec cases)
→ Threshold: >20% = cần bổ sung trước release

---

### 4. Layer 3 — QA Smoke Test

> Chỉ dành cho Tier 1 release

| Critical Flow | Kết quả | Ghi chú |
|---|---|---|
| | PASS / FAIL | |
| | PASS / FAIL | |
| | PASS / FAIL | |

**Sanity check:**
- [ ] Không có 500 error / broken screen
- [ ] Error state xử lý đúng
- [ ] Load time ổn

---

### 5. Go/No-Go Decision

**Quyết định:** `🟢 GO` / `🔴 NO-GO` / `🟡 GO WITH CONDITIONS`

**Lý do:**

**Conditions (nếu GO WITH CONDITIONS):**
- [ ] Condition 1:
- [ ] Owner xác nhận bằng văn bản: [Tên] — [ngày]

---

### 6. Process improvement actions

> Pattern nào cần fix ở DoR / playbook / template / upskilling?

| # | Gap phát hiện | Loại | Đề xuất cải tiến | Owner | Deadline |
|---|---|---|---|---|---|
| 1 | | DoR / TC / Process / Security | | | |
| 2 | | | | | |

---

### 7. Tracking re-check

**Next checkpoint:** [DD/MM/YYYY]
**Check lại:** Gap items trong section 6 đã được fix chưa?
**QA theo dõi:** ___
