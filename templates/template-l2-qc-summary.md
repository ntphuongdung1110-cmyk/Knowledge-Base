# Template — L2 QC Summary (AI-SDLC)

> **Dán vào:** Comment của Redmine ticket khi QC set status "QC Approved"
> **Owner:** QC | **SLA:** Cùng lúc với status change → QC Approved
> **Prerequisite:** L1 Dev Evidence đã được comment trên ticket này

---

## [L2] QC Summary — #[Ticket ID] [Ticket Title]

**Dev:** [Tên] | **QC:** [Tên]
**Date:** [DD/MM/YYYY]
**Status:** QC Approved

---

### Tham chiếu từ L1 (Dev Evidence)

- **Dev TC file (L1):** [SharePoint URL từ comment L1]
- **Dev TC lúc handoff:** Critical: __ Pass / __ Fail · High: __ Pass / __ Fail
- **Known AI Deviations (từ L1):** [Số lượng — QC đã chú ý test kỹ các vùng này]

---

### QC TC — Bổ sung và Thực thi

> QC AI-gen TC từ AC (S2), cross-reference với Dev TC, bổ sung gap và adhoc cases.
> Dev TC và QC TC đặt trong cùng file SharePoint — tab riêng.

**QC TC file:** [SharePoint URL]
**TC QC bổ sung:** __ cases

**Lý do bổ sung chính:**

| # | Case bổ sung | Loại | Lý do Dev TC chưa cover |
|---|---|---|---|
| 1 | | Gap / Adhoc / Regression | |
| 2 | | Gap / Adhoc / Regression | |

---

### Kết quả Thực thi — Pass Rate

| Priority | Tổng TC (Dev + QC) | Pass | Fail | Skip |
|---|---|---|---|---|
| Critical | | | | |
| High | | | | |
| Medium | | | | |
| Low | | | | |
| **TOTAL** | | | | **Pass rate: __%** |

**Exit criteria check:**

- [ ] Critical: 100% Pass _(bắt buộc — còn Fail = không set QC Approved)_
- [ ] High: 100% Pass _(bắt buộc)_
- [ ] Total pass rate ≥ 95% _(còn fail Medium/Low: ghi rõ lý do chấp nhận)_

---

### Bug Summary

| Bug ID | Mô tả ngắn | Severity | Status |
|---|---|---|---|
| #__ | | S1 / S2 / S3 / S4 | Resolved / Open |

_Không có bug → ghi: "Không phát sinh bug trong quá trình test."_

---

### AC Coverage — Final

| AC # | Nội dung AC | TC cover (Dev + QC) | Kết quả |
|---|---|---|---|
| AC-01 | | TC-001, QC-005 | Pass |
| AC-02 | | QC-003 | Fail — Bug #__ |

**TC gap rate:** __% _(= AC items không có TC / tổng AC × 100)_

> Gap rate > 20%: bắt buộc bổ sung TC trước khi set QC Approved.
> Gap rate 10–20%: ghi rõ lý do chấp nhận.

---

### QC Sign-off Checklist

- [ ] Critical và High: 100% Pass
- [ ] Total pass rate ≥ 95%
- [ ] S1/S2 bugs: tất cả Resolved (hoặc không có)
- [ ] TC file đủ fields: ID · Title · Preconditions · Steps · Expected · Actual · Status · Bug link
- [ ] AC coverage gap rate < 20%
- [ ] Exploratory / adhoc testing đã thực hiện ngoài scripted TC
- [ ] Known AI Deviations từ L1 đã được test kỹ

**QC confirms:** Evidence đầy đủ, ticket sẵn sàng cho QA audit.
