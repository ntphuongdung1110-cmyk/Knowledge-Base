# QA Process — Quy trình hoạt động

**Owner:** QA Manager | **Tạo:** 2026-05-08 | **Cập nhật:** 2026-05-08
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
║  │  Dev──[SA0–SA5+DoR]──► QC──[sweep]──► QA ROLLING AUDIT│      ║
║  │                                       (ngay khi ticket │      ║
║  │                                        QC approve)     │      ║
║  │  Grooming → Planning → Daily → Review → Retro          │      ║
║  │  [QA]       [QA 30']            [QA]   [QA khi cần]   │      ║
║  └────────────────────────────────────────────────────────┘      ║
║                              │                                   ║
║  R-DAY GATE                  │                                   ║
║  ┌───────────────────────────▼────────────────────────────┐      ║
║  │  R-day: Smoke test STG → Go/No-Go → Deploy               │      ║
║  │                ┌──────────┼──────────┐                  │      ║
║  │               GO       GO+COND    NO-GO                 │      ║
║  └────────────────┼─────────┼──────────┼───────────────────┘      ║
║                   │         │          │                          ║
║              Deploy    Deploy+doc   Dev/QC fix                   ║
║              Prod       risk        → re-audit                   ║
║                   │                                              ║
║  PRODUCTION       │                                              ║
║  ┌────────────────▼───────────────────────────────────────┐      ║
║  │  QC smoke test production (≤15 phút)                   │      ║
║  │  QA monitor 24h đầu (error rate, bug reports)          │      ║
║  │  Nếu bug → QA Post-Incident RCA                        │      ║
║  └────────────────────────────────────────────────────────┘      ║
║                                                                  ║
║  QUARTERLY (tháng 3 / 6 / 9 / 12)                                ║
║  ┌────────────────────────────────────────────────────────┐      ║
║  │  Metrics review → Tier adjustment → Process update     │      ║
║  └────────────────────────────────────────────────────────┘      ║
╚══════════════════════════════════════════════════════════════════╝
```

---

## Môi trường: STG vs Production

**QA audit và gate chỉ diễn ra trên STG. Production có verification riêng nhưng không phải QA audit.**

```
                    STG                      PRODUCTION
                 ─────────                  ───────────

Layer 1          ✅ Document review          ❌ Không cần
(Dev evidence)   (không cần môi trường)

Layer 2          ✅ TC review                ❌ Không cần
(QC work)        (không cần môi trường)

Layer 3          ✅ QA Smoke test            ❌ QA không test
(Smoke test)     → Go/No-Go decision          ở đây

Post-deploy      ─────────────────────      ✅ QC smoke test
verification                                (≤15 phút, critical flows)
                                            QA nhận kết quả + monitor 24h
```

**Lý do QA không audit trên Production:**
- Data thật → test = tạo transaction thật, ảnh hưởng user thật
- QA gate là pre-deploy — quyết định GO/NO-GO phải xảy ra TRƯỚC deploy
- Nếu phát hiện vấn đề sau deploy chỉ còn rollback

> **Lưu ý:** STG phải mirror Production đủ để audit có giá trị. Cần confirm với Dev: STG dùng real API hay mock? Config (payment, voucher rules) có sync với Production không?

---

## Agile Ceremony — QA tham gia buổi nào?

```
SPRINT CEREMONIES
─────────────────────────────────────────────────────────────────

  Grooming       Planning     Daily      Review      Retro
  ─────────       ────────    ──────    ────────    ──────
  ✅ Attend       ✅ 30'      ❌ Không  ✅ Attend   ⚡ Khi cần
  (full)          đầu

  Release Planning (ngoài sprint)
  ─────────────────────────────
  ✅ Bắt buộc — đặt audit window vào lịch release
```

### Chi tiết từng ceremony

**Grooming** ✅ — QA tham gia đầy đủ

Đây là buổi quan trọng nhất để QA tham gia sớm. QA làm gì:
- Flag risk sớm: "ticket này có financial flow → cần Tier 1 audit"
- Clarify DoD: acceptance criteria cần cụ thể để QA audit được
- Xác nhận evidence nào cần chuẩn bị (SA pipeline, SonarQube)
- Input vào estimate: ticket phức tạp → QA estimate thêm 30–60 phút audit

**Sprint Planning** ✅ — QA tham gia 30 phút đầu

Mục đích: biết scope sprint để plan rolling audit calendar.
- Xem danh sách tickets được commit vào sprint
- Estimate tổng QA audit time cho sprint (xem bảng bên dưới)
- Flag nếu sprint có quá nhiều Tier 1 items → cần điều chỉnh

**Daily Standup** ❌ — QA không tham gia

QA work async, không cần báo cáo daily. Nếu có finding cần alert → nhắn trực tiếp cho Dev Lead/QC Lead.

**Sprint Review** ✅ — QA tham gia đầy đủ (observe)

- Xem những gì đã ship trong sprint
- Verify qua evidence đang được làm đúng không (không phán xét trước mặt team, ghi nhận riêng)
- Input cho planning audit sprint tiếp theo

**Retrospective** ⚡ — QA tham gia khi có quality issue

- Không attend thường xuyên
- Được mời khi sprint có nhiều bug/DoR fail/release bị block
- Hoặc QA gửi findings summary cho PM/Scrum Master → SM đưa vào retro

**Release Planning** ✅ — Bắt buộc

- Đây là lúc QA đặt audit window vào lịch release
- Confirm release date → QA book R-day smoke test window
- Nếu release date không có buffer cho QA → phải re-negotiate ngay, không để đến hôm trước

---

## Sprint Calendar — QA trong 2 tuần sprint

```
SPRINT 2 TUẦN — CÁI NHÌN TỔNG THỂ

          Mon      Tue      Wed      Thu      Fri
Week 1  ┌────────┬────────┬────────┬────────┬────────┐
        │        │        │        │        │        │
        │Dev code│Dev code│Dev code│QC test │QC test │
        │SA pipe-│SA pipe-│SA pipe-│        │        │
        │line    │line    │line    │  QA    │  QA    │
        │        │        │        │rolling │rolling │
        │        │        │        │audit   │audit   │
        │        │        │Grooming│(tickets│(tickets│
        │        │        │[QA ✅] │Closed) │Closed) │
        └────────┴────────┴────────┴────────┴────────┘

          Mon      Tue      Wed      Thu      Fri
Week 2  ┌────────┬────────┬────────┬────────┬────────┐
        │        │        │        │        │        │
        │Dev fix │Dev fix │QC re-  │        │Sprint  │
        │bugs    │bugs    │test    │        │Review  │
        │        │        │        │        │[QA ✅] │
        │        │  QA    │  QA    │        │        │
        │        │rolling │rolling │        │Weekly  │
        │        │audit   │audit   │        │Report  │
        │Planning│        │        │        │        │
        │[QA30'] │        │        │        │        │
        └────────┴────────┴────────┴────────┴────────┘

Release day (ví dụ: Mon tuần 3)
        ┌──────────────────────────────────────────┐
        │ QA Smoke Test STG → Go/No-Go Decision    │
        │ → Gửi QA Audit Report                    │
        │ → Deploy Production                      │
        │ → QC smoke test Production (≤15 phút)    │
        │ → QA monitor kết quả + 24h watch         │
        └──────────────────────────────────────────┘
```

---

## Time Estimation — QA

### Per ticket (rolling audit trong sprint)

| Tier | QA audit time | Gồm |
|---|---|---|
| Tier 1 | 45–60 phút | Layer 1 (Dev evidence) + Layer 2 (TC review) |
| Tier 2 | 20–30 phút | Layer 1 chủ yếu + Layer 2 sample |
| Tier 3 | Không audit | — |

### Per sprint (tổng QA workload)

Ví dụ sprint có 10 Tier 1 + 5 Tier 2 tickets:

| Hoạt động | Estimate |
|---|---|
| Rolling audit Tier 1 (10 × 50 phút) | 8 giờ |
| Rolling audit Tier 2 (5 × 25 phút) | 2 giờ |
| Release day smoke test + decision | 1–2 giờ |
| Weekly Quality Report | 30 phút |
| **Tổng** | **~12 giờ/sprint** |

> QA cần estimate thêm vào Sprint Planning để timeline không bị surprise.

### Phân loại findings theo fixability

Khi QA phát hiện issue, phân loại ngay để team biết impact:

| Loại | Fix time | Impact đến timeline |
|---|---|---|
| **Quick fix** | <2 giờ | Không delay release — fix trong ngày |
| **Medium** | <1 ngày làm việc | Delay release 1 ngày — chấp nhận được |
| **Complex / Structural** | >1 ngày | NO-GO — cần re-schedule release |

---

## Quy trình 1 — Rolling Audit trong Sprint

### Sơ đồ

```
                   Ticket lifecycle trong sprint
                   ─────────────────────────────

Dev code          QC test          QA audit
─────────         ────────         ────────
Ticket Open
    │
Dev complete
+ SA pipeline
+ DoR done
    │
    ▼
QC receives ──► QC sweep/test
                    │
              QC approve?
                    │
              ┌─────┴──────┐
              ▼             ▼
           Pass          Fail → Dev fix
              │
              │  ← TRIGGER QA ROLLING AUDIT NGAY
              ▼
         QA audit Layer 1
         (Dev evidence, 15–30 phút)
              │
         QA audit Layer 2
         (QC work, 15–30 phút)
              │
         ┌────┴────┐
         ▼          ▼
       Pass        Gap?
         │           │
    Ghi nhận    Loại gap?
    weekly      ├─ S1/S2 → Alert ngay Dev Lead + QC Lead
    report      └─ S3/S4 → Ghi vào weekly report
```

### Mô tả các bước

**Bước 1 — Trigger (ngay khi QC approve ticket)**

QC approve ticket → QA nhận tín hiệu (Redmine status change) → bắt đầu audit trong cùng ngày hoặc ngày làm việc tiếp theo. Không đợi đến T-2.

**Bước 2 — Audit Layer 1 (15–30 phút)**

Kiểm tra Dev evidence — không hỏi Dev, evidence phải tự nói lên được:
- SA0–SA5 evidence đính kèm đúng format
- SonarQube link đính kèm, đạt chuẩn (≥90%, 0 Blocker, 0 Critical)
- Acceptance criteria từng item có pass/fail rõ ràng
- QA scan spec → liệt kê case thiếu trong SA3 (nếu có)

**Bước 3 — Audit Layer 2 (15–30 phút)**

Kiểm tra QC work:
- TC format đúng template chuẩn trên SharePoint
- Mọi TC có Status — không để trống
- Scan spec → TC list → tính TC gap rate (số case thiếu / tổng spec cases)

**Bước 4 — Xử lý kết quả**

| Kết quả | Hành động |
|---|---|
| Pass | Ghi ticket vào "audit done" list → tổng hợp weekly report |
| S3/S4 gap | Ghi vào report, theo dõi pattern |
| S1/S2 gap | Alert ngay Dev Lead + QC Lead trong ngày |

---

## Quy trình 2 — Release Gate (Tier 1)

> Nhờ rolling audit trong sprint, phần lớn tickets đã được audit xong.
> Release gate chỉ còn: Smoke test STG → R-day Go/No-Go Decision.

### Sơ đồ

```
         PM / Dev Lead                    QA                     Outcome
         ─────────────               ──────────               ──────────

[Trong sprint]:
                                  Rolling audit từng ticket
                                  sau khi QC approve
                                  (phần lớn tickets đã done)

Release day:
  (Sáng)                         Layer 3: Smoke test STG
                                  Critical flows (1–2 giờ)
                                       │
  (Trưa)                          Go/No-Go Decision
                                  + Gửi QA Audit Report
                                       │
                         ┌─────────────┼────────────┐
                         ▼             ▼             ▼
                      🟢 GO       🟡 GO+COND     🔴 NO-GO
                         │             │             │
                      Deploy        PM ack        Phân loại
                     Production    + Deploy        findings
                         │         + doc risk   (Quick/Medium/
                         │             │          Complex)
                   QC smoke test  QC smoke test      │
                   production      production    Dev/QC fix
                         │             │         (SLA theo loại)
                   QA monitor     QA monitor         │
                      24h            24h         QA re-verify
                                                    (≤4h)
```

### Mô tả các bước

**Release day sáng: Smoke Test STG (1–2 giờ)**

QA tự thực hiện trên STG — chỉ critical flows liên quan đến release:
- Consumer flows nếu có change: browse → mua → dùng voucher
- Campaign/Loyalty nếu có change: tham gia → nhận thưởng → verify điểm
- API nếu có change: auth → core request → idempotency
- Sanity: không có 500 error, load time ổn

**Release day trưa: Go/No-Go Decision**

Hoàn thiện report và ra quyết định:

| Quyết định | Điều kiện |
|---|---|
| 🟢 GO | Tất cả criteria pass |
| 🟡 GO WITH CONDITIONS | S2 bug có workaround + PM ack; TC gap <20% và smoke test cover |
| 🔴 NO-GO | S1 open / SonarQube Blocker / TC gap >30% / smoke test fail |

**Nếu NO-GO — phân loại findings ngay:**

| Loại | Fix time ước tính | Timeline impact |
|---|---|---|
| Quick fix | <2 giờ | Release cùng ngày |
| Medium | <1 ngày | Delay 1 ngày |
| Complex | >1 ngày | Re-schedule release |

Gửi QA Audit Report đến: Dev Lead + PM + QC Lead.

**Post-deploy: Production verification**

- QC thực hiện smoke test production (≤15 phút, critical flows)
- QC báo kết quả cho QA
- QA monitor 24h đầu: error rate, bug reports từ CS/user

---

## Quy trình 3 — Release Gate (Tier 2)

### Sơ đồ

```
Tier 2 release
      │
      ▼
   Override trigger?
   (financial change, migration,
    post-incident, nhiều Tier 2
    đổi cùng release, risk signal)
      │
   ┌──┴──┐
   ▼     ▼
  Có    Không
   │       │
QA audit   PM/Dev request QA? ──Có──► QA audit (tự nguyện)
(như Tier 1  │
 lighter)    Không
             │
             ▼
       Không có QA gate
       Dev/QC tự chịu trách nhiệm
```

---

## Quy trình 4 — Post-Incident RCA

### Sơ đồ

```
Bug phát hiện trên Production
           │
           ▼
  QC log bug Redmine + tag severity
           │
    ┌──────┴──────┐
    ▼             ▼
  S1/S2         S3/S4
  Alert QA      QA nhận
  cùng ngày     cuối tuần
           │
           ▼
  QA Process RCA (≤1 ngày làm việc)
  ──────────────────────────────────
  Câu hỏi 1: Bug lẽ ra catch ở đâu?
    ├─ Layer 1? → SA evidence thiếu
    ├─ Layer 2? → TC coverage gap
    └─ Layer 3? → Smoke test miss

  Câu hỏi 2: QA đã check chưa?
    ├─ Đã check → tại sao miss?
    │             (checklist mù / evidence fake / QA oversight)
    └─ Chưa check → tại sao?
                   (không có trong checklist / out of scope)

  Câu hỏi 3: Fix gì để không lặp lại?
           │
    ┌──────┼──────────┐
    ▼      ▼          ▼
  Update  Feedback  Update
  audit   QC/Dev    report
  check-  (process) template
  list
```

### Mô tả các bước

**Bước 1 — Nhận thông báo:** S1/S2 → alert QA cùng ngày. S3/S4 → QA review cuối tuần qua Redmine.

**Bước 2 — Process RCA (≤1 ngày):** Trả lời 3 câu hỏi trên, ghi kết quả ra rõ ràng.

**Bước 3 — Action items:**

| Phát hiện | Hành động |
|---|---|
| Checklist thiếu item | Update QA audit checklist ngay |
| Evidence của QC/Dev thiếu | Feedback cho QC Lead/Dev Lead (process, không blame cá nhân) |
| QA oversight | Self-improvement — ghi chú vào QA guide |

---

## Quy trình 5 — Quarterly Review

### Sơ đồ

```
Cuối quarter (tháng 3 / 6 / 9 / 12)
             │
             ▼
  Pull metrics từ Redmine + SonarQube
  ┌────────────────────────────────────────┐
  │ DoR compliance rate      target ≥80%   │
  │ TC coverage gap rate     target <20%   │
  │ SonarQube compliance     target 100%   │
  │ Release blocked count    trending ↓    │
  │ Bug escaped to prod      trending ↓    │
  │ Audit cycle time         target ≤4h    │
  └────────────────┬───────────────────────┘
                   │
          ┌────────┼────────┐
          ▼        ▼        ▼
       Metrics   Process  Tier
       on track? gaps?   correct?
          │        │        │
       ✓ OK   Fix DoR/  Nâng/hạ
              playbook  tier nếu
                        cần
                   │
             Quarterly QA Report
             → QA Manager + QC Manager + Dev Leads
             → Process improvement backlog Q+1
```

---

## Escalation Path

### Kịch bản 1 — PM muốn override NO-GO

```
QA: NO-GO
     │
     ▼
PM không đồng ý
     │
     ▼
PM phải gặp QA Manager (không trực tiếp pressure QA)
     │
     ┌──────────────────────┐
     ▼                      ▼
QA Manager đồng ý      QA Manager muốn override
với QA                      │
     │                 Phải documented rõ lý do
Giữ NO-GO             bằng văn bản
     │                      │
PM escalate            CPO/CTO override
lên CPO/CTO            (documented chính thức)
     │                      │
CPO/CTO quyết định    QA ghi vào audit log
                       → Track outcome sau
```

### Kịch bản 2 — QC/Dev không đồng ý với QA finding

```
QA finding gửi đến QC/Dev
     │
QC/Dev disagree
     │
QC/Dev trình bày context bổ sung (≤24 giờ)
     │
QA review lại
     │
  ┌──┴──┐
  ▼      ▼
QA sai  QA đúng
  │       │
Acknowledge  Giữ finding
+ update     + explain
checklist    rõ hơn
               │
         Vẫn không đồng ý?
               │
         Escalate: QA Manager + QC Manager
```

---

## SLAs

| Hoạt động | SLA | Owner |
|---|---|---|
| QA audit ticket sau QC approve (rolling) | ≤1 ngày làm việc | QA |
| Release day smoke test + decision | ≤2 giờ | QA |
| QA Audit Report gửi sau decision | Trong ngày | QA |
| Dev/QC fix Quick findings | <2 giờ | Dev/QC |
| Dev/QC fix Medium findings | <1 ngày làm việc | Dev/QC |
| QA verify fix sau NO-GO | ≤4 giờ làm việc | QA |
| Post-incident Process RCA (S1/S2) | ≤1 ngày làm việc | QA |
| Weekly Quality Report | Thứ Sáu cuối tuần | QA |
| Quarterly QA Report | Tuần đầu tháng kế tiếp | QA |

---

## Communication Protocol

| Loại finding | Gửi đến | Format | Khi nào |
|---|---|---|---|
| S1/S2 gap trong rolling audit | Dev Lead + QC Lead | Redmine comment + direct message ngay | Cùng ngày phát hiện |
| Go/No-Go decision | Dev Lead + PM + QC Lead | QA Audit Report | Release day trưa |
| Spot-check patterns | QC Lead + QC Manager | Weekly Quality Report | Thứ Sáu |
| Post-incident RCA | Dev Lead + PM + QC Lead | Process RCA doc | ≤1 ngày sau incident |
| Process improvement | QA Manager + QC Manager + Dev Leads | Quarterly Report | Đầu quarter tiếp |

**Nguyên tắc:**
- Findings kèm **evidence cụ thể**, không phán xét chung chung
- Tone: "Spec mục X không có TC tương ứng" — không "QC làm thiếu"
- QA không blame cá nhân — focus vào process gap

---

## Liên kết

- [QA Role Definition](qa-role-definition.md) — Scope và trách nhiệm
- [QA Audit Checklist](qa-audit-checklist.md) — Checklist chi tiết Layer 1/2/3
- [QA Quality Standards](qa-quality-standards.md) — Tiêu chuẩn Go/No-Go
- [QC Classification Matrix](qc-classification-matrix.md) — Tier nào cần audit mức nào
- [QA Audit Report template](../templates/report-qa-audit.md)
- [Dev Test DoR](dev-test-dor.md) — Evidence QA kiểm tra ở Layer 1
