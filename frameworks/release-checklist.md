# Release Checklist

**Owner:** QA + QC | **Cập nhật:** 2026-05-07
**Dùng khi:** Chuẩn bị release lên Production

---

## STG Testing

### Requirement
- [ ] Acceptance criteria đã được define rõ, không còn mâu thuẫn
- [ ] Edge cases và risk đã được identify (SA2 Risk Scout)
- [ ] Tất cả tickets trong release đạt DoD ticket-level

### Test Execution
- [ ] Test cases đã được viết đủ, có review chéo
- [ ] Tất cả test cases đã được thực thi trên STG
- [ ] Không có S1/S2 bug còn open trên STG
- [ ] S3/S4 nếu còn open: đã log Redmine, PM ack risk
- [ ] Retest pass sau khi Dev fix

### Code Quality (SonarQube)
- [ ] Unit test coverage ≥90% cho code mới/changed
- [ ] 0 Blocker issue
- [ ] 0 Critical issue
- [ ] Security Hotspots: tất cả đã Resolved hoặc Accepted

### QA Gate ← **Bước bắt buộc trước khi deploy**
- [ ] QA Audit Report đã có (Tier 1) hoặc Spot-check pass (Tier 2)
- [ ] QA decision = **GO** hoặc **GO WITH CONDITIONS** (conditions documented)
- [ ] Nếu GO WITH CONDITIONS: PM đã ack bằng văn bản

---

## Release Day

### Deploy
- [ ] Thông báo team trước khi deploy
- [ ] Deployment window đã được chọn (tránh giờ cao điểm)
- [ ] Rollback plan đã có và đã test

### Smoke Test Production (QC thực hiện)
- [ ] Happy path chính đã pass
- [ ] Integration với external service đã kiểm tra
- [ ] Không có error mới trong logs
- [ ] Performance không có dấu hiệu bất thường

---

## Post-Release (24h sau)

- [ ] Monitor error rate trong 24h
- [ ] Kiểm tra bug report từ user/CS
- [ ] Cập nhật test case nếu phát hiện case mới
- [ ] Ghi lesson learned vào BookStack nếu có incident

---

## Bug Severity — Release Decision

| Severity | Tên | Release action |
|---|---|---|
| **S1** | Critical | ❌ Block — QA NO-GO bắt buộc |
| **S2** | Major | ❌ Block — QA NO-GO (hoặc GO WITH CONDITIONS nếu PM ack) |
| **S3** | Minor | ✅ Release được — fix sprint sau |
| **S4** | Trivial | ✅ Release được — backlog |

> Định nghĩa đầy đủ: [Quality Standards](qa-quality-standards.md)

---

## Liên kết

- [QA Quality Standards](qa-quality-standards.md) — Tiêu chuẩn chất lượng đầy đủ
- [QA Audit Checklist](qa-audit-checklist.md) — QA gate criteria
- [QC Testing Process](qc-testing-process.md)
- [QC Classification Matrix](qc-classification-matrix.md) — Tier của sản phẩm
