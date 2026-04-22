# Release Checklist

**Dùng khi:** Chuẩn bị release lên Production
**Cập nhật:** 2026-04-21

---

## Pre-Release (STG)

### Requirement
- [ ] Requirement đã được QC review và confirm không còn mâu thuẫn
- [ ] Acceptance criteria đã được define rõ
- [ ] Edge cases và risk đã được identify (dùng Risk Scout nếu cần)

### Test Execution
- [ ] Test cases đã được viết đầy đủ
- [ ] Test cases đã được peer review
- [ ] Tất cả test cases đã được thực thi trên STG
- [ ] Bug critical/blocker = 0 trên STG
- [ ] Bug major đã được fix hoặc có plan xử lý rõ ràng
- [ ] Regression test đã pass

### Technical
- [ ] Code coverage ≥60% (SonarQube)
- [ ] Không có critical/blocker issue trên SonarQube
- [ ] Unit test pass
- [ ] API test pass (nếu có thay đổi API)

### Documentation
- [ ] Test plan đã cập nhật
- [ ] Bug report đã ghi đầy đủ
- [ ] Lesson learned ghi chú (nếu có bug phức tạp)

---

## Release Day

### Deploy
- [ ] Thông báo team trước khi deploy
- [ ] Deployment window đã được chọn (tránh giờ cao điểm)
- [ ] Rollback plan đã có

### Smoke Test Production
- [ ] Happy path chính đã pass
- [ ] Integration với external service đã kiểm tra
- [ ] Không có error mới trong logs
- [ ] Performance không có dấu hiệu bất thường

---

## Post-Release (24h sau)

- [ ] Monitor error rate trong 24h
- [ ] Kiểm tra bug report từ user/CS
- [ ] Cập nhật test case nếu phát hiện case mới
- [ ] Ghi lesson learned vào BookStack

---

## Risk Levels

| Level | Mô tả | Action |
|---|---|---|
| 🔴 Critical | Crash, data loss, security breach | Block deploy ngay |
| 🟠 Major | Core feature broken, wrong data | Fix trước deploy |
| 🟡 Minor | UI bug, edge case | Có thể deploy, fix trong sprint sau |
| 🟢 Trivial | Cosmetic, typo | Backlog |

---

## Liên kết

- [QC Testing Process](qc-testing-process.md)
- [Templates — Sprint Review](../templates/sprint-review.md)
