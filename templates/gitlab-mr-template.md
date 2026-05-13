# GitLab MR Template — AI-SDLC Workflow

**Cách dùng:** Copy toàn bộ nội dung phần "MR Template" bên dưới vào file:
`.gitlab/merge_request_templates/default.md` trong mỗi GitLab project.

---

## MR Template

```markdown
## 📋 Ticket Reference
- Redmine: #[ID]
- Loại thay đổi: Feature / Bug Fix / Refactor / Hotfix

---

## 1. Document Analysis → Spec (SA0 + SA1)

> Bắt buộc hoàn thành trước khi bắt đầu code.

- [ ] Đọc đầy đủ ticket, acceptance criteria, và tài liệu liên quan
- [ ] SA0: Viết context summary vào comment ticket (làm gì / thay đổi ở đâu / ảnh hưởng đến đâu)
- [ ] SA1: Chạy Requirement Clarifier — phát hiện assumption ẩn, mâu thuẫn spec
- [ ] Tất cả điểm chưa rõ đã clarify với BA/PO trước khi code (ghi rõ ai confirm, ngày nào)
- [ ] Hiểu rõ scope: cái gì IN, cái gì OUT của ticket này

**Evidence:** _[Link comment SA0 trong ticket / screenshot SA1 output]_

---

## 2. Technical Design

> Bắt buộc với feature mới hoặc thay đổi architecture. Skip được với bug fix nhỏ.

- [ ] Xác định các component/module bị ảnh hưởng
- [ ] Thiết kế flow/sequence diagram nếu có luồng mới (dùng AI để draft)
- [ ] Xác định API contract (request/response schema) nếu có endpoint mới/thay đổi
- [ ] Review edge cases và error handling ở design stage (không phải lúc code xong)
- [ ] Technical design đã được Tech Lead review (nếu thay đổi lớn)

**Notes:** _[Mô tả ngắn approach hoặc link design doc]_

---

## 3. AI Usage

> Commit message phải có tag `[AI]`, `[HUMAN]`, hoặc `[MIX]`. Thiếu tag = pipeline fail.

**AI tool đã dùng:** Cursor / Claude / ChatGPT / Copilot / _khác:_

| Bước | AI có dùng không? | Tool |
|---|---|---|
| Phân tích spec / SA1 | Yes / No | |
| Technical design | Yes / No | |
| Viết code | Yes / No | |
| Code review (self) | Yes / No | |
| Sinh test cases / SA3 | Yes / No | |

---

## 4. SA Pipeline — Testing Evidence

- [ ] SA2: Risk matrix đã xác định — High risks rõ ràng
- [ ] SA3: Test scenarios đã execute — kết quả pass/fail ghi lại
- [ ] SA4: Ít nhất 3 attack vectors đã thử
- [ ] SA5: RCA đã làm _(chỉ khi có bug S1/S2)_

**Evidence:** _[Link hoặc paste risk matrix / test scenarios]_

---

## 5. Code Quality

- [ ] Unit test ≥90% coverage cho code mới/changed (SonarQube verify)
- [ ] Không có Critical/High issue mới trên SonarQube
- [ ] Không có hardcoded credential, API key, hoặc sensitive data
- [ ] Không có `console.log` / debug statement còn sót

---

## 6. Checklist trước khi Submit

- [ ] Tất cả commit có tag `[AI]` / `[HUMAN]` / `[MIX]`
- [ ] Branch đã rebase/merge latest từ `main`/`develop`
- [ ] Self-review MR diff bằng AI trước khi assign reviewer
- [ ] DoR checklist trong ticket đã điền đủ
- [ ] Đã assign đúng reviewer (Tech Lead / peer)

---

## 7. Notes cho Reviewer

> _Viết ngắn gọn: context cần biết, điểm cần review kỹ, trade-off đã chọn_

```

---

## Hướng dẫn setup trên GitLab

### Bước 1 — Tạo file template trong repo

```bash
mkdir -p .gitlab/merge_request_templates
# Tạo file default.md với nội dung MR Template ở trên
```

### Bước 2 — Commit và push

```bash
git add .gitlab/merge_request_templates/default.md
git commit -m "[HUMAN] Add AI-SDLC MR template for GitLab"
git push
```

### Bước 3 — Verify

Tạo MR mới → GitLab tự động load template vào description field.

---

## Liên kết

- [Dev AI Testing Playbook](../frameworks/dev-ai-testing-playbook.md) — SA0–SA5 chi tiết
- [Dev Test DoR](../frameworks/dev-test-dor.md) — Definition of Ready đầy đủ
