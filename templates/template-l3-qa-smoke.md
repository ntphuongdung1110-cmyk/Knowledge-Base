# Template — L3 QA Smoke Test

> **Dán vào:** Comment của Redmine ticket sau khi QA hoàn thành smoke test trên STG
> **Owner:** QA | **SLA:** ≤ 1 ngày làm việc sau L2 pass | **Env:** STG

---

## [L3] QA Smoke Test — #[Ticket ID] [Ticket Title]

**QA:** [Tên]
**Date:** [DD/MM/YYYY]
**Env:** STG | **Build / Commit:** ___

---

### L1 + L2 Pre-check

- [ ] L1 Dev evidence: tất cả 11 items PASS
- [ ] L2 QC summary: "QC Approved" đã set + TC gap rate < 20%

_Nếu có items FAIL ở L1/L2 → KHÔNG thực hiện L3, notify Dev/QC fix trước._

---

### Scope — Flows cần test

**Story flows (core):**

| # | Flow | Mô tả |
|---|---|---|
| 1 | | |
| 2 | | |

**Affected flows (manual identification qua code diff + SA2 risk matrix):**

| # | Flow | Lý do ảnh hưởng |
|---|---|---|
| 1 | | [Module / function bị thay đổi] |

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
- [ ] Rollback plan xác nhận (nếu cần)

---

### GO / NO-GO

**Quyết định:** `🟢 GO` / `🔴 NO-GO` / `🟡 GO WITH CONDITIONS`

**Lý do:**

**Conditions (nếu GO WITH CONDITIONS):**

- Condition:
- Owner acknowledged: [Tên] — [ngày] — [link comment Redmine / email]

---

_QA Audit Report đầy đủ (L1 + L2 + L3 summary) sẽ gửi riêng tới Dev Lead + PM + QC Lead._
