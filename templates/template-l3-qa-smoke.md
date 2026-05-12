# Template — L3 QA Smoke Test (AI-SDLC)

> **Dán vào:** Comment của Redmine ticket sau khi QA hoàn thành smoke test trên STG
> **Owner:** QA | **SLA:** ≤ 1 ngày làm việc sau L2 pass | **Env:** STG

---

## [L3] QA Smoke Test — #[Ticket ID] [Ticket Title]

**QA:** [Tên]
**Date:** [DD/MM/YYYY]
**Env:** STG | **Build / Commit:** ___

---

### Pre-check — L1 + L2

| # | Check | Status |
|---|---|---|
| 1 | L1: Technical Design + API Spec attached | ✅ / ❌ |
| 2 | L1: Gate S3 sign-off confirmed (BA + Dev + QC) | ✅ / ❌ |
| 3 | L1: SonarQube pass (Coverage ≥90%, Blocker/Critical = 0) | ✅ / ❌ |
| 4 | L1: Code Review evidence present | ✅ / ❌ |
| 5 | L2: Status "QC Approved" đã set | ✅ / ❌ |
| 6 | L2: Critical + High — 100% Pass | ✅ / ❌ |
| 7 | L2: Total pass rate ≥ 95% | ✅ / ❌ |
| 8 | L2: AC coverage gap rate < 20% | ✅ / ❌ |

> Bất kỳ item nào ❌ → **KHÔNG thực hiện L3**, notify Dev/QC fix trước.

---

### Scope — Flows cần test

> Source: Domain link (L1) + Technical Design (S3) để xác định affected flows

**Story flows (core):**

| # | Flow | Mô tả |
|---|---|---|
| 1 | | |
| 2 | | |

**Affected flows** _(xác định từ Technical Design S3 — modules/API thay đổi)_:

| # | Flow | Module/API bị thay đổi | Lý do ảnh hưởng |
|---|---|---|---|
| 1 | | | |

---

### Kết quả Smoke Test

| Flow | Loại | Kết quả | Ghi chú |
|---|---|---|---|
| | Story / Affected | PASS / FAIL | |
| | Story / Affected | PASS / FAIL | |

**Sanity check:**

- [ ] Không có 500 error / broken screen
- [ ] Error state xử lý đúng (input lỗi → message rõ ràng)
- [ ] Performance bình thường (không chậm bất thường)
- [ ] Known AI Deviations (từ L1) — đã verify trực tiếp trên STG

---

### GO / NO-GO

| Điều kiện | Check |
|---|---|
| Không có story flow FAIL | ✅ / ❌ |
| Không có affected flow FAIL critical | ✅ / ❌ |
| Sanity check pass | ✅ / ❌ |

**Quyết định:** `🟢 GO` / `🔴 NO-GO` / `🟡 GO WITH CONDITIONS`

**Lý do:**

**Conditions (nếu GO WITH CONDITIONS):**

- Condition:
- Risk acknowledged bởi: [Tên] — [ngày] — [link comment Redmine / email]

---

_QA Audit Report đầy đủ (L1 + L2 + L3 summary) sẽ gửi riêng tới Dev Lead + PM + QC Lead._
