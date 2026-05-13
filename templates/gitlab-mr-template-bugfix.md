# GitLab MR Template — Bug Fix

**Cách dùng:** Copy nội dung phần "MR Template" vào file:
`.gitlab/merge_request_templates/bugfix.md` trong GitLab project.

---

## MR Template

```markdown
## 🐛 Bug Fix
- Redmine bug: #[ID]
- Severity: S1 / S2 / S3 / S4
- Môi trường phát hiện: DEV / STG / PROD

---

## 1. Root Cause

> Mô tả ngắn gọn: bug xảy ra ở đâu, tại sao.

**Root cause:** _[1–2 câu]_

**SA5 RCA đã chạy:** Yes / No _(bắt buộc với S1/S2)_

---

## 2. Fix Approach

- [ ] Fix đúng root cause, không phải workaround
- [ ] Xác định scope ảnh hưởng — fix này có impact gì ngoài điểm bug không?
- [ ] Không introduce regression (review các module liên quan)

**Approach:** _[Mô tả cách fix ngắn gọn]_

---

## 3. AI Usage

**AI tool đã dùng:** Cursor / Claude / ChatGPT / Copilot / Không dùng

- [ ] Tất cả commit có tag `[AI]` / `[HUMAN]` / `[MIX]`

---

## 4. Testing

- [ ] Steps to reproduce đã verify → bug không còn reproduce được
- [ ] SA4: Ít nhất 2 related attack vectors đã thử (tránh tương tự bug này)
- [ ] Regression test: các luồng liên quan vẫn pass
- [ ] Unit test cho case này đã thêm (tránh bug tái phát)

**Steps to reproduce (để reviewer verify):**
1.
2.
3.

---

## 5. Checklist Submit

- [ ] Tất cả commit có tag `[AI]` / `[HUMAN]` / `[MIX]`
- [ ] Branch đã rebase/merge latest
- [ ] Self-review diff — không có unrelated change
- [ ] Comment rõ trong ticket: root cause + fix approach

```

---

## Setup trên GitLab

```bash
# Tạo file trong repo
.gitlab/merge_request_templates/bugfix.md

# Khi tạo MR → chọn template "bugfix" từ dropdown
```

---

## Liên kết

- [MR Template Feature](gitlab-mr-template.md) — Template đầy đủ cho feature
- [Dev AI Testing Playbook](../frameworks/dev-ai-testing-playbook.md) — SA0–SA5
