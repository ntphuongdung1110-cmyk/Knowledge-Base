# GitLab MR Template — Feature / Refactor

**Cách dùng:** Copy nội dung trong khối markdown bên dưới vào:
`.gitlab/merge_request_templates/default.md`

---

## MR Template

```markdown
## MR Info
- Ticket: #[ID]
- Loại: Feature / Refactor

## Checklist
- [ ] SA pipeline đã chạy đủ — evidence ghi trong ticket comment
- [ ] Self-review code với AI trước khi tạo MR
- [ ] Các luồng liên quan đã test (không chỉ happy path)
- [ ] Không có breaking change chưa thông báo
- [ ] Commit tags đúng [AI] / [HUMAN] / [MIX]

## Notes cho Reviewer
<!-- Điểm cần review kỹ, trade-off đã chọn, context quan trọng -->
```

---

## Automated gates (hệ thống tự enforce — không cần check thủ công)

| Gate | Kết quả nếu fail |
|---|---|
| Commit thiếu `[AI/HUMAN/MIX]` tag | Block push |
| SonarQube Quality Gate fail | Block merge |

> SonarQube Quality Gate đã bao gồm coverage ≥90% và code quality — không cần gate riêng cho từng thứ.

---

## Setup trên GitLab

```bash
mkdir -p .gitlab/merge_request_templates
# Copy nội dung MR Template ở trên vào:
# .gitlab/merge_request_templates/default.md
git add .gitlab/merge_request_templates/default.md
git commit -m "[HUMAN] Add lean MR template for AI-SDLC workflow"
git push
```

---

## Liên kết

- [MR Template Bug Fix](gitlab-mr-template-bugfix.md)
- [Dev AI Testing Playbook](../frameworks/dev-ai-testing-playbook.md) — SA0–SA5 chi tiết
- [Dev Test DoR](../frameworks/dev-test-dor.md) — Definition of Ready
