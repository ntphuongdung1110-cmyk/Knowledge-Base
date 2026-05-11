# Quy Trình Testing 2026 — QA Gate

**Version:** 1.0 | **Owner:** QA Manager — Dung Nguyễn | **Hiệu lực:** 2026-05-01
**Cập nhật:** 2026-05-11 | **Áp dụng:** Toàn team PD (BA/PO · Dev · QC · QA) · Leadership

---

## Phần 1 — Mục tiêu & Vai trò QA

QA là **independent release gate** — không test lại toàn bộ, mà kiểm tra *evidence* rằng Dev và QC đã làm đúng tiêu chuẩn trước khi deploy.

| | QC — Quality Control | QA — Quality Assurance |
|---|---|---|
| **Làm gì** | Test, viết TC, log bug, sweep | Audit evidence, kiểm tra quy trình |
| **Phạm vi** | Từng ticket / feature | Toàn bộ quy trình + release gate |
| **Output** | Bug report, TC pass/fail | Audit report + Go/No-Go decision |
| **Báo cáo về** | Dev Lead / QC Lead | QA Manager |

**Tại sao cần gate độc lập:**
- Dev và QC có conflict of interest khi tự đánh giá release của mình
- QA đọc evidence trực tiếp từ Redmine — không hỏi, không trust, chỉ verify facts
- Mọi quyết định GO/NO-GO có audit trail, không verbal

---

## Phần 2 — Quy trình tổng thể

### Flow 4 role × 4 phase

| Phase | BA / PO | Dev | QC | QA |
|---|---|---|---|---|
| **🔵 SPRINT** | ① Viết spec + AC gắn Redmine | ② SA0–SA5 pipeline<br>③ SonarQube pass | — | — |
| **🟡 STG TESTING** | Clarify khi QC hỏi | Fix S1/S2 bug (↺ loop với QC) | ④ DoR check<br>⑤ Viết TC<br>⑥ Test STG<br>⑦ Set "QC Approved" | ⑧ Rolling Audit (L1 + L2) |
| **🟠 PRE-RELEASE** | Sign-off nếu GO WITH CONDITIONS | Fix findings từ QA<br>S1/S2 phải fix trước R-day | — | ⑨ T-5 Early Warning<br>⑩ T-1 Compile Report<br>⑪ R-day GO/NO-GO |
| **🔴 PRODUCTION** | — | Hotfix nếu bug escaped (SLA ≤8h) | ⑫ Smoke test production | ⑬ Monitor + RCA nếu bug escaped |

### Điểm handoff — Điều kiện bắt buộc

| Từ | → Đến | Điều kiện — không đủ = không chuyển |
|---|---|---|
| BA/PO | Dev | Spec + AC hoàn chỉnh, gắn Redmine trước kick-off |
| Dev | QC | SA0–SA4 đủ · SonarQube pass · DoR self-check OK |
| QC | QA | Ticket = "QC Approved" · S1/S2 resolved · S3/S4 logged đầy đủ |
| QA (R-day) | Production | Smoke test pass · GO hoặc GO WITH CONDITIONS (có văn bản) |

---

## Phần 3 — GO/NO-GO Gate & Escalation

### R-day Decision

| Quyết định | Điều kiện | Action |
|---|---|---|
| ✅ **GO** | Không có S1/S2 open · SonarQube pass · TC gap <20% · Smoke test pass | Deploy lên Production |
| ⚠️ **GO WITH CONDITIONS** | S2 open nhưng có workaround · PM/BA/Dev Lead ack bằng comment Redmine hoặc email (ghi rõ risk + tên + ngày) | Deploy với acknowledged risks, ghi vào deploy note |
| ❌ **NO-GO** | S1 open · SonarQube Blocker/Critical · TC gap >30% · Smoke fail · Security issue chưa resolve | Block deploy → Dev/QC fix → QA re-smoke |

> **QA không thương lượng về NO-GO.** Mọi override phải đi qua QA Manager.

### Escalation — Khi có tranh luận về quyết định

```
QA ra NO-GO
    │
PM không đồng ý
    │
    ▼
PM gặp QA Manager  (không pressure QA trực tiếp)
    │
    ├─ QA Manager giữ NO-GO
    │       │
    │   PM escalate lên CPO / CTO → CPO/CTO quyết định
    │
    └─ QA Manager đồng ý override
            │
        Phải documented bằng văn bản rõ lý do
        CPO/CTO sign-off chính thức
        QA ghi vào audit log + track outcome sau deploy
```

**Nguyên tắc:** Mọi override NO-GO có văn bản. QA không bao giờ bị pressure trực tiếp từ PM hay Dev.

---

## Phần 4 — QA Audit Rules

### Rolling Audit — Trigger: QC set "QC Approved"

> **QA chỉ đọc từ Redmine. Thiếu bất kỳ item nào = tự động FAIL.**
> **SLA: ≤ 1 ngày làm việc / ticket.**

#### Layer 1 — Dev evidence (11 items, tất cả bắt buộc)

| # | Item | Pass | Fail |
|---|---|---|---|
| 1 | SA0 Context | Comment mô tả change trong ticket | Thiếu |
| 2 | SA1 Clarify | Screenshot confirm với BA/PO | Thiếu |
| 3 | SA2 Risk matrix | Table High/Medium/Low | Thiếu |
| 4 | SA3 Test scenarios | List scenarios + pass/fail từng item | Thiếu hoặc không có kết quả |
| 5 | SA4 Security check | List attack vectors + pass/fail | Thiếu |
| 6 | SA5 RCA | Bắt buộc nếu có S1/S2 bug trong quá trình dev | Có S1/S2 mà thiếu SA5 |
| 7 | SonarQube link | Link đính kèm trong ticket | Thiếu link |
| 8 | Coverage | ≥ 90% (code mới/changed) | < 90% |
| 9 | Blocker + Critical | 0 | Có bất kỳ |
| 10 | Security Hotspot | 0 Open (Resolved hoặc Accepted + lý do) | Còn Open |
| 11 | AC pass/fail | Từng AC item có status rõ ràng | Để trống |

#### Layer 2 — QC work (5 items)

| # | Item | Pass | Kết quả nếu lệch |
|---|---|---|---|
| 1 | TC file link (SharePoint) | Đính kèm trong ticket | ❌ FAIL |
| 2 | TC format | Đủ fields: ID · Title · Preconditions · Steps · Expected · Actual · Status | ❌ FAIL |
| 3 | TC status | Mỗi TC có Pass/Fail rõ ràng, không để trống | ❌ FAIL |
| 4 | Bug link | S1/S2 có link Redmine trong cột Bug link | ⚠️ Warning |
| 5 | TC coverage gap vs AC | < 20% | ⚠️ Warning nếu 20–30% · ❌ FAIL nếu >30% |

### Phân loại findings — Tác động đến release timeline

| Loại | Fix time | Timeline impact |
|---|---|---|
| S1 open | — | ❌ NO-GO tự động |
| Quick fix | < 2 giờ | Release cùng ngày |
| Medium | < 1 ngày | Delay 1 ngày |
| Complex / Structural | > 1 ngày | NO-GO — re-schedule release |

### Communication khi có finding

| Loại finding | Gửi đến | Khi nào |
|---|---|---|
| S1/S2 gap trong rolling audit | Dev Lead + QC Lead (Redmine + direct message) | Cùng ngày phát hiện |
| GO/NO-GO decision | Dev Lead + PM + QC Lead (QA Audit Report) | R-day |
| Pattern gaps (S3/S4) | QC Lead + QC Manager (Weekly Report) | Thứ Sáu hàng tuần |
| Bug escaped to production | Dev Lead + PM + QC Lead (Process RCA doc) | ≤ 1 ngày sau incident |

> **Tone chuẩn:** "Spec mục X không có TC tương ứng" — không "QC làm thiếu". Findings kèm evidence cụ thể, không phán xét chung.

---

## Phần 5 — Metrics & Nhịp hoạt động

### KPIs — QA Manager theo dõi hàng quarter

| Metric | Target | Ý nghĩa |
|---|---|---|
| DoR compliance rate | ≥ 80% | Dev chuẩn bị evidence đúng tỷ lệ bao nhiêu |
| TC coverage gap rate | < 20% | QC viết TC cover đủ spec không |
| SonarQube compliance | 100% | Code đạt chuẩn chất lượng |
| Release block count | Trending ↓ | Số lần QA block release đang giảm |
| Bug escaped to production | Trending ↓ | Bug lọt qua gate đang giảm |
| Audit cycle time | ≤ 1 ngày | QA xử lý đủ nhanh không |

Metrics được pull từ Redmine + SonarQube. Report gửi QA Manager + QC Manager + Dev Leads mỗi quarter.

### QA trong sprint — Ai ở đâu

| Ceremony | QA tham gia? | Mục đích |
|---|---|---|
| Grooming | ✅ Đầy đủ | Flag risk sớm, clarify DoD, estimate audit time |
| Sprint Planning | ✅ Đầu sprint | Biết scope, plan rolling audit calendar |
| Daily Standup | ❌ Không | QA work async — alert trực tiếp khi cần |
| Sprint Review | ✅ Observe | Verify evidence pattern, input cho sprint sau |
| Retrospective | ⚡ Khi có quality issue | Gửi findings summary, không attend thường xuyên |
| Release Planning | ✅ Bắt buộc | Book R-day audit window vào lịch release ngay |

### Quarterly Review (tháng 3 · 6 · 9 · 12)

Pull metrics → Đánh giá 3 trục: **Metrics trend** · **Process gaps** · **Tier classification đúng không** → Quarterly QA Report → Process improvement backlog Q+1.

---

## Liên kết tài liệu

| Tài liệu | Mô tả |
|---|---|
| [QA Process — Nội bộ QA](qa-process.md) | 5 quy trình chi tiết của QA team (rolling audit, release gate, RCA, quarterly) |
| [QA Audit Checklist](qa-audit-checklist.md) | Checklist đầy đủ Layer 1/2/3 |
| [QC Classification Matrix](qc-classification-matrix.md) | Tier 1/2/3 — ticket nào cần audit mức nào |
| [Dev Test DoR](dev-test-dor.md) | Điều kiện Dev phải đạt trước khi handover QC |
| [Dev AI Testing Playbook](dev-ai-testing-playbook.md) | Hướng dẫn SA0–SA5 từng bước |
| [QC Sweep Checklist](qc-sweep-checklist.md) | QC làm gì sau khi nhận ticket |
| [QA Audit Report template](../templates/report-qa-audit.md) | Format report gửi sau R-day decision |
