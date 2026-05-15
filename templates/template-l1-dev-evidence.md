# Template — L1 Dev Evidence (AI-SDLC)

> **Dán vào:** Comment của Redmine ticket khi Dev hoàn thành S4 và sẵn sàng bàn giao QC
> **Owner:** Dev | **Prerequisite:** Gate S3 đã có BA + Dev + QC sign-off

---

## [L1] Dev Evidence — #[Ticket ID] [Ticket Title]

**Dev:** [Tên]
**Date:** [DD/MM/YYYY]
**Status:** Ready for QC

---

### S3 — Design Artifacts

| # | Artifact | Link | Status |
|---|---|---|---|
| 1 | Technical Design | [BookStack / SharePoint URL] | ✅ / ❌ |
| 2 | API Spec / OpenAPI | [Link] / N/A — không có API mới | ✅ / N/A |
| 3 | Cross-check Checklist | [Link hoặc paste bên dưới] | ✅ / ❌ |
| 4 | Gate S3 Sign-off | BA: [Tên] · Dev: [Tên] · QC: [Tên] — [ngày] | ✅ / ❌ |

**Cross-check summary** _(Technical Design vs BRD + Functional Spec)_:
- Conflict/gap phát hiện: [mô tả hoặc "Không có"]
- Xử lý: [đã resolve / loop lại S3]

---

### S4 — Coding Artifacts

| # | Artifact | Link / Detail | Status |
|---|---|---|---|
| 5 | Code Review | MR/PR link: [URL] · Reviewer: [Tên] | ✅ / ❌ |
| 6 | SonarQube | Link: [URL] | ✅ / ❌ |
| 7 | Unit Test | Pass rate: __/__ · Coverage: __% | ✅ / ❌ |

**SonarQube detail:**
- Coverage (code mới/changed): __% _(threshold: ≥ 90%)_
- Blocker: __ _(threshold: 0)_
- Critical: __ _(threshold: 0)_
- Security Hotspot Open: __ _(threshold: 0 — Resolved hoặc Accepted + lý do)_

---

### AI-Assisted Testing Summary (SA0–SA4)

> Output của từng SA step — QC dùng để không phải chạy lại từ đầu.

**SA0 — Scope Context:**
- Ticket hiểu như thế nào: [1–2 câu mô tả scope]
- Impact estimate: [module / API / DB bị ảnh hưởng]

**SA1 — Requirement Clarifications:**

| Ambiguity found | Resolution | Confirmed by |
|---|---|---|
| [mô tả mâu thuẫn / assumption ẩn] | [đã resolve thế nào] | [BA/PO tên] — [ngày] |

_Không có → ghi: "Spec rõ ràng, không cần clarify."_

**SA2 — Risk Matrix:**

| Risk | Severity | TC cover |
|---|---|---|
| [mô tả risk] | High / Med / Low | [TC ID hoặc "Manual probe"] |

_Copy từ SA2 Risk Scout output — note rõ High risk nào đã test._

**SA4 — Bug Hunter Vectors Tried:**
- [Boundary condition / Negative path / Edge case]: [kết quả]

_Không chạy SA4 → ghi: "SA4 skip — ticket low risk theo SA2."_

---

### Dev Self-Test

**TC File:** [SharePoint URL]

| Priority | Tổng | Pass | Fail | Ghi chú |
|---|---|---|---|---|
| Critical | | | | Tất cả phải Pass trước khi handoff |
| High | | | | Tất cả phải Pass trước khi handoff |
| Medium | | | | Fail ghi rõ lý do |
| Low | | | | |

> Critical hoặc High còn Fail → **không handoff QC**, fix trước.

**AC Traceability** _(mỗi AC item → TC ID cover)_:

| AC # | Nội dung AC | TC ID | Priority |
|---|---|---|---|
| AC-01 | | TC-001, TC-002 | Critical |
| AC-02 | | TC-003 | High |

**Domain link (STG):** [URL tính năng trên staging]

---

### Known AI Deviations

> Những chỗ Dev phải override hoặc sửa output AI — đây là vùng rủi ro cao, QC cần chú ý khi test.

| # | Vị trí (file/function) | AI gen gì | Dev override thành gì | Lý do |
|---|---|---|---|---|
| 1 | | | | |

_Không có deviation → ghi: "AI output được dùng trực tiếp, không có override."_

---

### Dev Sign-off

> Tôi đã review toàn bộ AI output, xác nhận artifacts đầy đủ, và chịu trách nhiệm về code này.

**Dev:** [Tên] — [DD/MM/YYYY]
