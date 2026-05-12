# Template — L1 Dev Evidence

> **Dán vào:** Comment của Redmine ticket khi Dev hoàn thành coding và sẵn sàng bàn giao QC
> **Owner:** Dev | **SLA:** Trước khi QC bắt đầu test

---

## [L1] Dev Evidence — #[Ticket ID] [Ticket Title]

**Dev:** [Tên]
**Date:** [DD/MM/YYYY]
**Status:** Ready for QC

---

### SA0 — Context

<!-- Mô tả thay đổi: file/function nào thay đổi, logic nào bị ảnh hưởng -->

### SA1 — Requirement Clarification

<!-- Screenshot hoặc link comment confirm với BA/PO về yêu cầu không rõ -->

_Không có điểm mơ hồ → ghi: "Spec rõ, không cần clarify"_

### SA2 — Risk Matrix

| Risk | Mức độ | Mitigation |
|---|---|---|
| | High / Medium / Low | |

### SA3 — Test Scenarios (Dev self-test)

| # | Scenario | Kết quả |
|---|---|---|
| 1 | | Pass / Fail |
| 2 | | Pass / Fail |

### SA4 — Security Check

| Attack Vector | Kiểm tra | Kết quả |
|---|---|---|
| SQL Injection | Input user → query DB không? | Pass / N/A |
| XSS | Content user render không encode? | Pass / N/A |
| Auth bypass | Endpoint mới có kiểm tra quyền? | Pass / N/A |
| Hardcoded credentials | API key / password bị hard-code? | Pass / N/A |

### SA5 — Root Cause Analysis

_Chỉ điền nếu có S1/S2 bug phát sinh trong quá trình dev. Nếu không → bỏ qua section này._

- **Bug:** #[ID]
- **Root cause:**
- **Fix approach:**
- **Prevention:**

---

### SonarQube

- **Link:** [URL]
- **Coverage (code mới/changed):** __% _(threshold: ≥ 90%)_
- **Blocker:** __ _(threshold: 0)_
- **Critical:** __ _(threshold: 0)_
- **Security Hotspot Open:** __ _(threshold: 0 — Resolved hoặc Accepted + lý do)_

---

### AC Checklist

| # | Acceptance Criteria | Status |
|---|---|---|
| 1 | | Pass / Fail |
| 2 | | Pass / Fail |
