# QA Process — Quy trình hoạt động

**Owner:** QA Manager | **Tạo:** 2026-05-08
**Áp dụng:** QA team + stakeholders (Dev Lead, QC Manager, PM)

---

## Sơ đồ tổng quan

QA có **4 quy trình chính**, chạy theo các nhịp khác nhau:

```
╔══════════════════════════════════════════════════════════════════╗
║                    QA PROCESS — BỨC TRANH TỔNG THỂ              ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║  SPRINT (2 tuần — lặp liên tục)                                  ║
║  ┌────────────────────────────────────────────────────────┐      ║
║  │                                                        │      ║
║  │  Dev ──[SA0–SA5+DoR]──► QC ──[sweep/sample]──►  (??) │      ║
║  │                                                   ↑   │      ║
║  │                                    QA SPOT-CHECK ─┘   │      ║
║  │                               (ngẫu nhiên, bất kỳ lúc)│      ║
║  └────────────────────────────────────────────────────────┘      ║
║                              │                                   ║
║                    Tích lũy findings                             ║
║                              │                                   ║
║  TRƯỚC MỖI TIER 1 RELEASE    ▼                                   ║
║  ┌────────────────────────────────────────────────────────┐      ║
║  │                                                        │      ║
║  │  L1: Dev evidence audit ──┐                           │      ║
║  │  L2: QC work audit ───────┼──► QA REPORT              │      ║
║  │  L3: Smoke test STG ──────┘         │                 │      ║
║  │                                     ▼                 │      ║
║  │                          ┌──── GO / NO-GO ────┐       │      ║
║  │                          ▼          ▼          ▼       │      ║
║  │                         GO      GO+COND     NO-GO      │      ║
║  │                          │          │          │        │      ║
║  │                        Deploy   Deploy     Dev/QC fix   │      ║
║  │                                  +doc    → re-audit     │      ║
║  └────────────────────────────────────────────────────────┘      ║
║                              │                                   ║
║                     Production (24h monitor)                     ║
║                              │                                   ║
║  KHI CÓ BUG PRODUCTION       ▼                                   ║
║  ┌────────────────────────────────────────────────────────┐      ║
║  │  QA Process RCA → Tìm bước bị miss → Update checklist │      ║
║  └────────────────────────────────────────────────────────┘      ║
║                                                                  ║
║  QUARTERLY (tháng 3 / 6 / 9 / 12)                                ║
║  ┌────────────────────────────────────────────────────────┐      ║
║  │  Metrics review → Tier adjustment → Process update     │      ║
║  └────────────────────────────────────────────────────────┘      ║
╚══════════════════════════════════════════════════════════════════╝
```

---

## Quy trình 1 — Spot-check trong Sprint

### Sơ đồ

```
SPRINT WEEK 1                         SPRINT WEEK 2
──────────────────────────────────────────────────────
                                       
 Dev/QC làm việc bình thường            Tương tự Week 1
       │                                      │
       │  QA lấy danh sách tickets            │
       │  đã Closed trong tuần                │
       │  từ Redmine                          │
       │         │                            │
       │    Random chọn                       │
       │  ┌──────┴──────┐                     │
       │  │             │                     │
       │ Tier 1       Tier 2                  │
       │ 30–50%       15–20%                  │
       │  stories      stories                │
       │         │                            │
       │   Audit từng story:                  │
       │   ✔ Layer 1 (Dev evidence)           │
       │   ✔ Layer 2 sample (50% TC)          │
       │   ✘ Không smoke test                 │
       │         │                            │
       │    ┌────┴────┐                       │
       │   Pass     Gap?                      │
       │    │         │                       │
       │    │    S1/S2 gap?                   │
       │    │    ├── Có  → Alert ngay         │
       │    │    └── Không → Ghi vào report   │
       │         │                            │
       └─────────┘                            │
                                              │
                              ┌───────────────┘
                              │
                        THỨ SÁU CUỐI SPRINT
                              │
                        Gửi Weekly Quality Report
                        (tổng hợp findings cả sprint)
```

### Mô tả các bước

**Bước 1 — Chọn stories để spot-check (15 phút, cuối mỗi tuần)**

Vào cuối mỗi tuần, QA vào Redmine lấy danh sách tickets đã Closed trong tuần đó.

Ưu tiên chọn tickets có:
- Financial logic (tiền, voucher, điểm)
- Feature mới (lần đầu làm)
- Dev mới hoặc chưa quen domain

Chọn ngẫu nhiên theo tỷ lệ:

| Tier | Tỷ lệ spot-check | Thời gian/story |
|---|---|---|
| Tier 1 | 30–50% stories/tuần | 30–60 phút |
| Tier 2 | 15–20% stories/tuần | 15–30 phút |
| Tier 3 | Không (trừ khi có bug) | — |

**Bước 2 — Thực hiện spot-check (theo thời gian ngân sách)**

Chạy Layer 1 + Layer 2 (sample), không chạy Layer 3:

- **Layer 1 — Dev evidence:** SA0–SA5 có đủ không, SonarQube đạt chuẩn không, security baseline ổn không
- **Layer 2 — QC work (50% TCs):** TC format đúng template không, pass/fail ghi rõ không, coverage so spec có gap không

**Bước 3 — Xử lý kết quả**

| Kết quả | Hành động |
|---|---|
| Pass | Ghi nhận vào Weekly Quality Report, không cần thêm |
| Gap nhỏ (S3/S4 level) | Ghi vào report, theo dõi pattern |
| Gap nghiêm trọng (S1/S2 evidence thiếu) | Alert ngay Dev Lead + QC Lead trong ngày |

**Bước 4 — Weekly Quality Report (thứ Sáu)**

Tổng hợp tất cả findings trong sprint → gửi cho QC Lead + Dung.
Template: [report-weekly-quality.md](../templates/report-weekly-quality.md)

---

## Quy trình 2 — Release Gate (Tier 1)

> Bắt buộc trước **mọi** release có code change trên Tier 1 products.
> Danh sách Tier 1: xem [QC Classification Matrix](qc-classification-matrix.md)

### Sơ đồ

```
         PM / Dev Lead                    QA                     Outcome
         ─────────────               ──────────               ──────────

T-2 ngày:
  Gửi release scope ──────────────► Nhận scope
  • Danh sách tickets                Lên kế hoạch audit
  • Build version                    Xác định tickets
  • Changelog                        có override trigger
  • Deploy target time               (financial/migration/incident)

T-1 ngày:                            ┌─────────────────────┐
                                     │   LAYER 1 (≤2 giờ)  │
                                     │ • SA pipeline ok?    │
                                     │ • SonarQube đạt?     │
                                     │ • Security clear?    │
                                     │ • Req coverage?      │
                                     └──────────┬──────────┘
                                                │
                                     ┌──────────▼──────────┐
                                     │   LAYER 2 (≤1 giờ)  │
                                     │ • TC format ok?      │
                                     │ • Pass/Fail đủ?      │
                                     │ • Coverage vs spec?  │
                                     │ • Gap scan           │
                                     └──────────┬──────────┘
                                                │
                                         Draft Audit Report

Release day:                         ┌──────────▼──────────┐
  (Sáng)                             │   LAYER 3 (≤1 giờ)  │
                                     │ Smoke test STG:      │
                                     │ • Critical flows     │
                                     │ • Sanity check       │
                                     └──────────┬──────────┘
                                                │
  (Trưa)                             ┌──────────▼──────────┐
                                     │  FINALIZE REPORT     │
                                     │  + Decision          │
                                     └──────────┬──────────┘
                                                │
                              ┌─────────────────┼────────────────┐
                              ▼                 ▼                ▼
                          🟢 GO           🟡 GO+COND         🔴 NO-GO
                              │                 │                │
                           Deploy           PM ack          Gửi findings
                          Production        + Deploy         rõ ràng cho
                              │            + doc risk         Dev/QC
                              │                 │                │
                        QC smoke test     QC smoke test      Dev/QC fix
                        production         production             │
                              │                 │         QA verify (≤4h)
                        Monitor 24h       Monitor 24h             │
                                                              ┌───┴───┐
                                                              ▼       ▼
                                                             GO    Cần thêm
                                                              │     time?
                                                          Deploy   Re-schedule
```

### Mô tả các bước

**Bước 1 — Nhận scope và chuẩn bị (T-2 ngày)**

PM hoặc Dev Lead gửi:
- Danh sách tickets trong release
- Build version / branch name
- Changelog mô tả ngắn
- Thời gian deploy dự kiến

QA thực hiện:
- Xác nhận đã nhận scope
- Scan qua danh sách tickets, đánh dấu tickets ưu tiên audit kỹ (có override trigger: financial, migration, post-incident, new 3rd-party API)
- Lên kế hoạch: Layer 1+2 vào T-1, Layer 3 vào release day sáng

**Bước 2 — Audit Layer 1 (T-1 ngày, ≤2 giờ)**

Kiểm tra Dev evidence — không hỏi Dev, evidence phải tự nói lên được:

- SA0–SA5 evidence đính kèm đúng format và đủ nội dung
- SonarQube: link đính kèm, coverage ≥90%, 0 Blocker, 0 Critical, security hotspots reviewed
- Acceptance criteria: từng item có pass/fail rõ ràng
- QA tự scan spec → liệt kê case thiếu trong SA3 (nếu có)

**Bước 3 — Audit Layer 2 (T-1 ngày, ≤1 giờ)**

Kiểm tra QC work — không test lại, chỉ đọc evidence:

- TC format đúng template chuẩn trên SharePoint
- Mọi TC có Status (Pass/Fail/Blocked) — không để trống
- QA đọc spec → đọc TC list → liệt kê case trong spec không có TC
- Tính TC gap rate = số case thiếu / tổng case trong spec

**Bước 4 — Smoke Test Layer 3 (Release day sáng, ≤1 giờ)**

QA tự thực hiện trên STG — chỉ critical flows liên quan đến release này:

- Chọn flows dựa trên changelog (cái gì thay đổi thì test cái đó)
- Consumer flows nếu có change: browse → mua → dùng voucher
- Campaign/Loyalty nếu có change: tham gia → nhận thưởng → verify điểm
- API nếu có change: auth → core request → idempotency
- Sanity: không có 500 error, load time ổn, error state đúng

**Bước 5 — Go/No-Go Decision (Release day trưa)**

Hoàn thiện QA Audit Report và ra quyết định:

| Quyết định | Điều kiện | Hành động tiếp theo |
|---|---|---|
| 🟢 **GO** | Tất cả criteria pass | Gửi report → Deploy |
| 🟡 **GO WITH CONDITIONS** | S2 bug có workaround + PM ack; TC gap <20% và smoke test cover | PM ký acknowledge → Deploy + document |
| 🔴 **NO-GO** | Bất kỳ: S1 open / SonarQube Blocker / TC gap >30% / smoke test fail | Gửi findings → Dev/QC fix |

Gửi QA Audit Report đến: Dev Lead + PM + QC Lead

**Bước 6a — Nếu GO/GO WITH CONDITIONS**

- PM tiến hành deploy
- QC thực hiện smoke test production sau deploy
- QA monitor kết quả 24h đầu

**Bước 6b — Nếu NO-GO**

- Findings gửi kèm: lý do cụ thể + cần fix gì + priority
- Dev/QC fix và re-submit (SLA: ≤1 ngày làm việc)
- QA verify fixes trong 4 giờ làm việc
- Nếu fix đủ → GO → Deploy
- Nếu cần thêm time → PM re-schedule deploy window

---

## Quy trình 3 — Release Gate (Tier 2)

### Sơ đồ

```
Tier 2 release
      │
      ▼
   Có override trigger?
   (financial, migration,
    post-incident, risk signal)
      │
   ┌──┴──┐
   ▼     ▼
  Có    Không
   │       │
QA audit   PM/Dev request QA? ──Có──► QA audit (tự nguyện)
(như Tier 1  │
 nhưng       Không
 lighter)    │
             ▼
       Không có QA gate
       Dev/QC tự chịu trách nhiệm
```

Không có mandatory QA gate cho Tier 2. QA chỉ tham gia khi:
- Override trigger xảy ra (xem [Classification Matrix](qc-classification-matrix.md))
- PM/Dev Lead chủ động request
- QA thấy risk signal từ spot-check trong sprint

---

## Quy trình 4 — Post-Incident RCA

### Sơ đồ

```
Bug phát hiện trên Production
           │
           ▼
  QC log bug vào Redmine
  + tag severity (S1/S2/S3/S4)
           │
    ┌──────┴──────┐
    ▼             ▼
  S1/S2         S3/S4
  (alert QA      (QA nhận
  cùng ngày)     cuối tuần)
           │
           ▼
  QA thực hiện Process RCA (≤1 ngày)
  ┌─────────────────────────────────┐
  │ Bug này lẽ ra catch ở đâu?     │
  │                                 │
  │ Layer 1? ──► SA evidence thiếu  │
  │ Layer 2? ──► TC coverage gap    │
  │ Layer 3? ──► Smoke test miss    │
  │ Không check? ──► Checklist gap  │
  └──────────────┬──────────────────┘
                 │
        ┌────────┼──────────┐
        ▼        ▼          ▼
  Update      Feedback    Update
  QA audit    cho QC/Dev  report
  checklist   (process,   template
               không blame)
```

### Mô tả các bước

**Bước 1 — Nhận thông báo bug production**
- S1/S2: QC alert QA ngay cùng ngày phát hiện
- S3/S4: QA nhận qua Redmine dashboard, review cuối tuần

**Bước 2 — Phân tích Process RCA (≤1 ngày làm việc)**

Trả lời tuần tự 3 câu hỏi:

1. *Bug này lẽ ra bị catch ở bước nào?* (Layer 1, 2, hay 3)
2. *QA đã check item liên quan chưa?*
   - Nếu đã check → tại sao vẫn miss? (evidence fake / checklist mù / QA oversight)
   - Nếu chưa check → tại sao? (không có trong checklist / out of scope?)
3. *Cần thêm/sửa gì để không bị lại?*

**Bước 3 — Action items**

| Phát hiện | Hành động |
|---|---|
| Checklist thiếu mục → thêm vào QA audit checklist | Update ngay |
| Evidence của QC/Dev thiếu mục → feedback theo process | Gửi QC Lead/Dev Lead |
| QA oversight (có check nhưng miss) → retrain/update guide | Self-improvement |

---

## Quy trình 5 — Quarterly Review

### Sơ đồ

```
Cuối quarter (tháng 3 / 6 / 9 / 12)
             │
             ▼
  ┌─────────────────────────────────────────┐
  │           PULL METRICS                  │
  │                                         │
  │  DoR compliance rate     → target ≥80%  │
  │  TC coverage gap rate    → target <20%  │
  │  SonarQube compliance    → target 100%  │
  │  Release blocked count   → trending ↓  │
  │  Bug escaped count       → trending ↓  │
  │  Audit cycle time        → target ≤4h  │
  └──────────────────┬──────────────────────┘
                     │
             ┌───────┼───────────┐
             ▼       ▼           ▼
         Metrics   Process    Tier
         on track? gaps?    correct?
             │       │           │
        ✓ OK   Fix DoR/   Nâng/hạ tier
               playbook    nếu cần
                     │
                     ▼
         Quarterly QA Report
         → QA Manager + QC Manager + Dev Leads
         → Process improvement backlog Q+1
```

---

## Escalation Path

### Sơ đồ

```
KỊCH BẢN 1: PM muốn override NO-GO
──────────────────────────────────────────────────────

QA: NO-GO
     │
     ▼
PM không đồng ý → Phải gặp QA Manager
                        │
                ┌───────┴───────┐
                ▼               ▼
          QA Manager        QA Manager
          đồng ý với QA      muốn override
                │               │
           Giữ NO-GO        Phải documented
                │            rõ lý do bằng văn bản
                │               │
           PM escalate          ▼
           lên CPO/CTO    CPO/CTO override
                │         (documented chính thức)
                │               │
           CPO/CTO               ▼
           quyết định     QA ghi vào audit log
                          → Track outcome sau


KỊCH BẢN 2: QC/Dev không đồng ý với QA finding
──────────────────────────────────────────────────────

QA finding gửi đến QC/Dev
          │
          ▼
   QC/Dev disagree
          │
          ▼
   QC/Dev trình bày context bổ sung
   (≤24 giờ làm việc)
          │
          ▼
   QA review lại:
   ┌──────┴──────┐
   ▼             ▼
  QA sai      QA đúng
   │             │
Acknowledge   Giữ finding
+ update      + explain
checklist     rõ hơn
                │
          Vẫn không
          đồng ý?
                │
                ▼
       Escalate lên
     QA Manager + QC Manager
```

---

## SLAs

| Hoạt động | SLA | Owner |
|---|---|---|
| PM gửi scope → QA confirm nhận | ≤4 giờ làm việc | PM + QA |
| QA hoàn thành Layer 1+2 | ≤3 giờ (T-1 ngày) | QA |
| QA hoàn thành Layer 3 + decision | ≤2 giờ (release day sáng) | QA |
| QA Audit Report gửi sau decision | Trong ngày | QA |
| Dev/QC fix NO-GO findings (urgent) | ≤1 ngày làm việc | Dev/QC |
| QA verify fix sau NO-GO | ≤4 giờ làm việc | QA |
| Spot-check per story | ≤1 giờ | QA |
| Post-incident Process RCA | ≤1 ngày làm việc (S1/S2) | QA |
| Weekly Quality Report | Thứ Sáu cuối tuần | QA |
| Quarterly QA Report | Tuần đầu tháng kế tiếp | QA |

---

## Liên kết

- [QA Role Definition](qa-role-definition.md) — Scope và trách nhiệm
- [QA Audit Checklist](qa-audit-checklist.md) — Checklist chi tiết Layer 1/2/3
- [QA Quality Standards](qa-quality-standards.md) — Tiêu chuẩn Go/No-Go
- [QC Classification Matrix](qc-classification-matrix.md) — Tier nào cần audit mức nào
- [QA Audit Report template](../templates/report-qa-audit.md)
- [Dev Test DoR](dev-test-dor.md) — Evidence QA kiểm tra ở Layer 1
