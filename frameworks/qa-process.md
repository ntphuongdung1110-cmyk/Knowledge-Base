# QA Process — Quy trình hoạt động

**Owner:** QA Manager | **Tạo:** 2026-05-08
**Áp dụng:** QA team + stakeholders (Dev Lead, QC Manager, PM)

---

## Tổng quan — QA trong pipeline

```
                     Sprint
┌────────────────────────────────────────────────────────┐
│                                                        │
│  Dev → [SA0-SA5 + DoR] → QC sweep/sample → QA audit  │
│                                  ↑                    │
│              QA spot-check ngẫu nhiên bất kỳ bước     │
└────────────────────────────────────────────────────────┘
                          │
                          ▼
                   QA Go/No-Go Gate
                          │
              ┌───────────┼───────────┐
              ▼           ▼           ▼
             GO     GO WITH        NO-GO
              │     CONDITIONS        │
              ▼           │           ▼
          Production      │      Dev/QC fix
                          ▼      → back to QC
                     Deploy +
                  documented risk
```

---

## 1. Sprint Cycle Integration

QA **không** là thành viên sprint. QA hoạt động độc lập, không block daily workflow.

### QA tham gia những gì trong sprint:

| Ceremony | QA tham gia? | Mục đích |
|---|---|---|
| Sprint Planning | Không | Dev/QC tự plan |
| Daily Standup | Không | Không relevant |
| Sprint Review | ✅ Có (observe, không vote) | Hiểu scope shipped để lên kế hoạch audit |
| Sprint Retrospective | Không | QC team nội bộ |
| Release Planning | ✅ Có | Lên lịch audit theo release schedule |

### Spot-check trong sprint:

QA thực hiện spot-check **ngẫu nhiên, không báo trước**:

```
Tier 1 products: spot-check 30–50% stories/sprint
Tier 2 products: spot-check 15–20% stories/sprint
Tier 3 products: chỉ khi có production bug
```

**Thời gian ngân sách cho spot-check:**
- Tier 1 story: 30–60 phút/story (Layer 1 + 2, không smoke test)
- Tier 2 story: 15–30 phút/story (Layer 1 chủ yếu)

**Output:** Ghi nhận vào Weekly Quality Report — không gửi từng finding riêng lẻ (trừ S1/S2 gap).

---

## 2. Release Gate Process — Tier 1

> Áp dụng cho mọi release có code change trên Tier 1 products.
> Xem danh sách Tier 1: [QC Classification Matrix](qc-classification-matrix.md)

### Timeline chuẩn:

```
T-2 ngày trước release    QA nhận thông báo scope release từ PM/Dev Lead
        │
        ▼
T-1 ngày                  QA bắt đầu audit Layer 1 + 2 (evidence review)
        │
        ▼
Release day (sáng)        QA thực hiện Layer 3 Smoke Test trên STG
        │
        ▼
Release day (trưa)        QA ra Go/No-Go decision
        │
   ┌────┴────────────────┐
   ▼                     ▼
  GO / GO WITH         NO-GO
  CONDITIONS             │
   │                     ▼
   ▼             Dev/QC nhận findings
Deploy           Fix → Re-submit → QA verify
Production       (nếu urgent: same day verify)
```

### Bước chi tiết:

**Bước 1 — Chuẩn bị (T-2 ngày)**
- PM/Dev Lead gửi release scope: danh sách tickets, build version, changelog
- QA xác nhận nhận được và lên kế hoạch audit
- Xác định tickets nào có override trigger (financial, migration, incident) → audit kỹ hơn

**Bước 2 — Audit Layer 1 + 2 (T-1 ngày, ≤3 giờ)**
- Layer 1: Dev evidence (SA pipeline, SonarQube, security)
- Layer 2: QC work (TC format, coverage vs spec, completeness)
- Ghi nhận gap vào QA Audit Report draft

**Bước 3 — Smoke Test Layer 3 (Release day sáng, ≤1 giờ)**
- Chỉ critical flows liên quan đến thay đổi trong release
- Test trên đúng STG build sẽ deploy

**Bước 4 — Go/No-Go Decision (Release day trưa)**
- Hoàn thiện QA Audit Report
- Ra quyết định: GO / GO WITH CONDITIONS / NO-GO
- Gửi report đến Dev Lead + PM + QC Lead

**Bước 5a — Nếu GO:**
- PM/Dev Lead tiến hành deploy
- QC thực hiện smoke test production sau deploy
- QA monitor kết quả 24h

**Bước 5b — Nếu GO WITH CONDITIONS:**
- PM ký acknowledge conditions bằng văn bản (email hoặc comment Redmine)
- Deploy với documented risks
- Conditions phải có resolution timeline cụ thể

**Bước 5c — Nếu NO-GO:**
- QA gửi findings rõ ràng: lý do block + what needs to be fixed
- Dev/QC fix theo findings
- QA verify fixes trong **4 giờ làm việc**
- Nếu fix đủ → QA ra GO
- Nếu cần thêm time → re-schedule release

---

## 3. Release Gate Process — Tier 2

> Không có mandatory QA gate per release. QA audit theo spot-check cadence.

**Khi nào QA tham gia Tier 2 release:**
- Override trigger xảy ra (xem [Classification Matrix](qc-classification-matrix.md))
- PM/Dev Lead request QA audit (optional)
- QA chủ động khi thấy risk signal từ spot-check trong sprint

---

## 4. Spot-Check Process

QA thực hiện ngẫu nhiên — không theo lịch cố định để tránh team "chuẩn bị trước".

### Cách chọn stories để spot-check:

```
Cuối mỗi tuần:
1. Lấy danh sách tickets Closed trong tuần từ Redmine
2. Random chọn theo tỷ lệ (Tier 1: 30-50%, Tier 2: 15-20%)
3. Ưu tiên tickets có: financial logic, new feature, dev mới
```

### Scope của spot-check (khác với full audit):

| | Full Audit (Tier 1 release) | Spot-check |
|---|---|---|
| Layer 1 — Dev evidence | ✅ Đầy đủ | ✅ Đầy đủ |
| Layer 2 — QC work | ✅ Đầy đủ | ✅ Sample (50% TCs) |
| Layer 3 — Smoke test | ✅ Critical flows | ❌ Không (trừ khi có signal) |
| Output | QA Audit Report | Note trong Weekly Quality Report |

### Output của spot-check:

- **Pass:** Ghi nhận trong Weekly Quality Report (không cần action)
- **Minor gap:** Ghi vào report, theo dõi pattern
- **Critical gap (S1/S2 evidence thiếu):** Thông báo ngay cho Dev Lead + QC Lead

---

## 5. Post-Incident Audit

Khi có production bug thoát qua QA gate:

```
Bug reported on production
        │
        ▼
QC verify → log Redmine → tag severity
        │
        ▼
QA nhận thông báo (cùng ngày với S1/S2)
        │
        ▼
QA thực hiện Process RCA (≤1 ngày làm việc):
  - Bug này lẽ ra bị catch ở bước nào?
  - Layer 1/2/3 đã check item liên quan chưa?
  - Nếu check → tại sao miss? (checklist thiếu / evidence fake / QA oversight)
  - Nếu không check → cần thêm item vào checklist không?
        │
        ▼
Output: Process gap analysis + action items
  → Update QA checklist nếu cần
  → Feedback cho QC/Dev nếu evidence gap
  → Update QA Audit Report template nếu blind spot
```

---

## 6. Quarterly Process Health Review

Thực hiện cuối mỗi quarter (tháng 3, 6, 9, 12):

### Nội dung review:

**Metrics dashboard:**
- DoR compliance rate trend (target: ≥80%)
- TC coverage gap rate trend (target: <20%)
- SonarQube compliance rate (target: 100%)
- Release blocked count (trending down?)
- Bug escaped to production count (trending down?)
- Audit cycle time (target: ≤4 giờ/Tier 1 release)

**Process review:**
- QA checklist có item nào không còn relevant → remove
- Có gap nào lặp lại ≥3 lần → cần fix ở DoR/playbook
- Tier classification có cần điều chỉnh không (sản phẩm nào nên lên/xuống tier)

**Output:**
- Quarterly QA Report (gửi QA Manager + QC Manager + Dev Leads)
- Updated QA checklist (nếu có thay đổi)
- Process improvement backlog cho quarter tiếp theo

---

## 7. Escalation Path

### Khi QA ra NO-GO nhưng có pressure ship:

```
QA NO-GO decision
        │
        ▼
PM muốn override → PM phải nói chuyện với QA Manager
        │
        ▼
QA Manager xem xét:
  - Nếu đồng ý với QA → giữ NO-GO, PM escalate lên CPO/CTO
  - Nếu QA Manager muốn override → phải documented rõ lý do
        │
        ▼
CPO/CTO là người duy nhất có quyền override QA gate
(documented bằng email/message chính thức)
        │
        ▼
Nếu override xảy ra → QA ghi nhận vào audit log
→ Track xem decision có đúng không (nếu bug → học bài)
```

**Nguyên tắc:** QA không bị áp lực ngầm. Override phải explicit và documented. Nếu override đúng thường xuyên → re-examine QA criteria. Nếu override dẫn đến bug → học bài từ incident.

### Khi QC/Dev không đồng ý với QA finding:

```
QC/Dev disagree với gap QA phát hiện
        │
        ▼
QC/Dev trình bày context bổ sung cho QA (≤24h)
        │
        ▼
QA review lại với context mới:
  - Nếu QA sai → acknowledge, update checklist để không repeat
  - Nếu QA đúng → giữ finding, explain rõ hơn
        │
        ▼
Nếu vẫn không đồng ý → escalate lên QA Manager + QC Manager
```

---

## 8. SLAs

| Hoạt động | SLA | Owner |
|---|---|---|
| QA nhận scope release → bắt đầu audit | ≤4 giờ làm việc | QA |
| Full Tier 1 audit (Layer 1+2+3) | ≤4 giờ | QA |
| Spot-check (per story) | ≤1 giờ | QA |
| QA Audit Report gửi sau decision | Trong ngày | QA |
| Dev/QC fix NO-GO findings (urgent) | ≤1 ngày làm việc | Dev/QC |
| QA verify fix sau NO-GO | ≤4 giờ làm việc | QA |
| Post-incident process RCA | ≤1 ngày làm việc | QA |
| Weekly Quality Report gửi | Thứ Sáu cuối tuần | QA |

---

## 9. Communication Protocol

**Ai nhận QA findings:**

| Loại finding | Gửi đến | Format |
|---|---|---|
| Full audit result (Go/No-Go) | Dev Lead + PM + QC Lead | QA Audit Report |
| Spot-check patterns | QC Lead + Dung | Weekly Quality Report |
| Critical gap (S1/S2 evidence thiếu) | Dev Lead + QC Lead | Redmine comment + slack ngay |
| Process improvement | QA Manager + QC Manager | Quarterly Report |
| Post-incident RCA | Dev Lead + PM + QC Lead + QA Manager | Process RCA doc |

**Nguyên tắc communication:**
- QA không blame cá nhân — focus vào process gap
- Findings phải kèm **evidence cụ thể**, không phán xét chung chung
- Tone: "Spec mục X không có TC tương ứng" (không "QC làm thiếu")

---

## Liên kết

- [QA Role Definition](qa-role-definition.md) — Scope và trách nhiệm
- [QA Audit Checklist](qa-audit-checklist.md) — Checklist chi tiết 3 layer
- [QA Quality Standards](qa-quality-standards.md) — Tiêu chuẩn Go/No-Go
- [QC Classification Matrix](qc-classification-matrix.md) — Tier nào cần audit mức nào
- [QA Audit Report template](../templates/report-qa-audit.md)
- [Dev Test DoR](dev-test-dor.md) — Evidence QA kiểm tra
