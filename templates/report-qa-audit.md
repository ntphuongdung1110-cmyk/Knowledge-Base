# Template — QA Audit Report

> Gửi: Dev Lead + PM + QC Lead | Sau mỗi audit | Owner: QA
> Tier 1 release: gửi trong ngày audit xong | Tier 2 spot-check: gửi cuối tuần

---

## QA Audit Report — [Tên sản phẩm / Sprint] | [DD/MM/YYYY]

**QA Auditor:**
**Tier:** Critical / Standard
**Trigger:** Pre-release / Spot-check / Post-incident
**Build / Sprint:** ___

---

### 1. Scope audit

| | Chi tiết |
|---|---|
| Số tickets trong scope | |
| Số tickets QA audited | |
| Layer thực hiện | L1 Dev Evidence / L2 QC Work / L3 Smoke Test |
| Thời gian audit | ___ giờ |

---

### 2. Layer 1 — Dev Evidence

| Hạng mục | Status | Gap cụ thể |
|---|---|---|
| SA0 context comment | ✅ Pass / ❌ Fail / ⚠️ Partial | |
| SA1 requirement clarification | ✅ / ❌ / ⚠️ | |
| SA3 test coverage vs spec | ✅ / ❌ / ⚠️ | |
| SonarQube compliance | ✅ / ❌ / ⚠️ | |
| Security baseline | ✅ / ❌ / ⚠️ | |

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
