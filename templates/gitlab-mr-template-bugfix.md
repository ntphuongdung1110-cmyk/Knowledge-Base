# GitLab MR Template — Bug Fix

**Cách dùng:** Copy nội dung trong khối markdown bên dưới vào:
`.gitlab/merge_request_templates/bugfix.md`

---

## MR Template

```markdown
## Bug Fix Info
- Ticket: #[ID]
- Severity: S1 / S2 / S3 / S4
- Môi trường phát hiện: DEV / STG / PROD

## Root Cause
<!-- 1–2 câu: bug xảy ra ở đâu, tại sao -->

## Checklist
- [ ] Fix đúng root cause, không phải workaround
- [ ] SA5 RCA đã chạy _(bắt buộc với S1/S2)_
- [ ] Steps to reproduce đã verify — bug không còn reproduce được
- [ ] Regression test: các luồng liên quan vẫn pass
- [ ] Commit tags đúng [AI] / [HUMAN]

## Notes cho Reviewer
<!-- Steps to reproduce để reviewer verify, context cần biết -->
```

---

## Automated gates (hệ thống tự enforce — không cần check thủ công)

| Gate | Kết quả nếu fail |
|---|---|
| Commit thiếu `[AI]` hoặc `[HUMAN]` tag | Block push |
| SonarQube Quality Gate fail | Block merge |

> SonarQube Quality Gate đã bao gồm coverage ≥90% và code quality — không cần gate riêng cho từng thứ.

---

## Setup trên GitLab

```bash
# Copy nội dung MR Template ở trên vào:
# .gitlab/merge_request_templates/bugfix.md
# Khi tạo MR → chọn template "bugfix" từ dropdown
```

---

## Liên kết

- [MR Template Feature](gitlab-mr-template.md)
- [Dev AI Testing Playbook](../frameworks/dev-ai-testing-playbook.md) — SA5 RCA
