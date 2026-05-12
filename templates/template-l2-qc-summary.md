# Template — L2 QC Summary

> **Dán vào:** Comment của Redmine ticket khi QC set status "QC Approved"
> **Owner:** QC | **SLA:** Cùng lúc với status change → QC Approved

---

## [L2] QC Summary — #[Ticket ID] [Ticket Title]

**QC:** [Tên]
**Date:** [DD/MM/YYYY]
**Status:** QC Approved

---

### TC File

- **SharePoint link:** [URL]
- **Tổng số TC:** __
- **Pass:** __ | **Fail:** __ | **Skip:** __

### Bug Summary

| Bug ID | Mô tả ngắn | Severity | Status |
|---|---|---|---|
| #__ | | S1 / S2 / S3 / S4 | Resolved / Open |

_Không có bug → ghi: "Không phát sinh bug trong quá trình test"_

### TC Coverage (tự đánh giá)

- **Tổng số spec items / AC:** __
- **TC đã cover:** __
- **TC gap rate:** __% _(= uncovered / total × 100)_

> Gap rate > 20%: phải bổ sung TC hoặc ghi rõ lý do chấp nhận gap

### QC Sign-off Checklist

- [ ] Tất cả S1/S2 đã Resolved (hoặc không có S1/S2)
- [ ] TC file có đủ fields: ID · Title · Preconditions · Steps · Expected · Actual · Status · Bug link
- [ ] Mỗi TC có Pass/Fail rõ ràng — không để trống
- [ ] Bug S1/S2 có Redmine link trong cột Bug link
- [ ] Tất cả AC đã pass

**QC confirms:** Evidence đầy đủ, ticket sẵn sàng cho QA audit.
