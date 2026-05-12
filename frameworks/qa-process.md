# QA Process — Quy trình hoạt động

**Owner:** QA Manager | **Tạo:** 2026-05-08 | **Cập nhật:** 2026-05-08
**Áp dụng:** QA team + stakeholders (Dev Lead, QC Manager, PM)

---

## Sơ đồ tổng quan

QA có **3 quy trình chính**, chạy theo các nhịp khác nhau:

```
╔══════════════════════════════════════════════════════════════════╗
║                    QA PROCESS — BỨC TRANH TỔNG THỂ              ║
╠══════════════════════════════════════════════════════════════════╣
║                                                                  ║
║  SPRINT + STG (2 tuần — lặp liên tục)                            ║
║  ┌────────────────────────────────────────────────────────┐      ║
║  │                                                        │      ║
║  │  QC viết TC (song song Dev coding)                     │      ║
║  │  Dev──[SA0–SA5]──► QC──[DoR+Execute TC]──► QA L1+L2   │      ║
║  │                                       (rolling audit   │      ║
║  │                                        ngay khi ticket │      ║
║  │                                        QC approve)     │      ║
║  │                        ▼                               │      ║
║  │                 QA Smoke test L3 (STG)                 │      ║
║  │                 → GO/NO-GO → Deploy                    │      ║
║  └────────────────────────────────────────────────────────┘      ║
║                   │         │          │                         ║
║                  GO      GO+COND    NO-GO                        ║
║              Deploy     Deploy+doc  Dev/QC fix                   ║
║              Prod        risk       → re-audit                   ║
║                   │                                              ║
║  PRODUCTION       │                                              ║
║  ┌────────────────▼───────────────────────────────────────┐      ║
║  │  QC smoke test production                               │      ║
║  │  QA monitor sau deploy (error rate, bug reports)        │      ║
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
  ✅ Attend       ✅          ❌ Không  ✅ Attend   ⚡ Khi cần
  (full)          (đầu sprint)

  Release Planning (ngoài sprint)
  ─────────────────────────────
  ✅ Bắt buộc — đặt Smoke test L3 window vào lịch release
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
- Confirm release date → QA book Smoke test L3 window
- Nếu release date không có buffer cho QA → phải re-negotiate ngay, không để đến hôm trước

---

## Sprint Calendar — QA trong 2 tuần sprint

```
SPRINT 2 TUẦN — CÁI NHÌN TỔNG THỂ

          Mon      Tue      Wed      Thu      Fri
Week 1  ┌────────┬────────┬────────┬────────┬────────┐
        │        │        │        │        │        │
        │Dev code│Dev code│Dev code│QC DoR  │QC exec │
        │SA pipe-│SA pipe-│SA pipe-│check + │TC trên │
        │line    │line    │line    │exec TC │STG     │
        │        │        │        │        │        │
        │QC phân │QC viết │QC viết │  QA    │  QA    │
        │tích    │TC      │TC      │rolling │rolling │
        │spec    │        │        │audit   │audit   │
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
        │[QA ✅] │        │        │        │        │
        └────────┴────────┴────────┴────────┴────────┘

STG Gate — cuối sprint (ví dụ: Mon tuần 3)
        ┌──────────────────────────────────────────┐
        │ QA Smoke Test L3 (STG) → GO/NO-GO        │
        │ → Gửi QA Audit Report                    │
        │ → Deploy Production                      │
        │ → QC smoke test Production               │
        │ → QA monitor kết quả sau deploy          │
        └──────────────────────────────────────────┘
```

---

## Time Estimation — QA

### Per ticket (rolling audit)

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
| STG Gate: Smoke test L3 + GO/NO-GO | 1–2 giờ |
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

## Quy trình 1 — STG Gate (Per-story)

Mỗi story/feature đi qua **1 gate** trên STG trước khi được phép deploy:

```
Dev done → push STG → QC bắt đầu test
    │
    ├─ QA: L1 audit (song song, không chờ QC done)
    │      SA0–SA5 · SonarQube · AC pass/fail
    │      Kết quả → flag ngay nếu có gap (Dev fix song song, QC không bị block)
    │
QC Approved
    │
    ├─ QA: L2 check (15–30 phút)
    │      TC execution results · Coverage vs spec
    │
    ├─ QA: L3 Smoke test (15–30 phút, Tier 1)
    │      Story flows + manually identified affected flows
    │
    └─ Pass → Story PASSED → GO/NO-GO → sẵn sàng deploy bất cứ lúc nào
```

### L1 — Audit Dev evidence (song song QC testing)

**Start:** QC bắt đầu test — không chờ QC Approved.
**Lý do:** Dev evidence (SA0–SA5, SonarQube) không thay đổi trong lúc QC test. Audit sớm → Dev fix sớm, không delay QC.

| # | Bước | Nội dung | Output |
|---|---|---|---|
| 1 | **Start trigger** | QC bắt đầu test story trên STG | — |
| 2 | **Audit L1** · 15–30 phút | SA0–SA5 đính kèm đúng format · SonarQube: ≥90% coverage · 0 Blocker · 0 Critical · AC từng item có pass/fail rõ | L1 result |
| 3 | **Xử lý kết quả** | Pass → ghi nhận. S1/S2 gap → alert Dev Lead + QC Lead ngay (Redmine + direct message). Dev fix song song, QC tiếp tục test bình thường | Alert hoặc Pass |

### L2 — Audit QC work (sau QC Approved)

**Start:** QC set ticket = "QC Approved".
**Lý do:** TC execution results chỉ có giá trị khi QC đã finish — không audit giữa chừng.

| # | Bước | Nội dung | Output |
|---|---|---|---|
| 1 | **Audit L2** · 15–30 phút | TC format đúng template chuẩn · Mọi TC có Status (không để trống) · Tính TC gap rate vs spec | L2 result + gap rate |
| 2 | **Xử lý kết quả** | Pass → sang L3. S3/S4 gap → ghi report. S1/S2 gap → alert ngay, hold L3 cho đến khi QC bổ sung | Pass hoặc Alert |

### L3 — Smoke test (sau L2 pass, Tier 1)

**Start:** L2 pass + QC Approved.
**Scope:** Story flows + manually identified affected flows.

**Cách xác định affected flows:**
- Đọc code diff / SA2 risk matrix của story → module nào bị touch?
- Map module → flows trong hệ thống (domain knowledge QA)
- Ví dụ: story thay đổi `voucher-redemption-service` → test thêm: checkout, wallet balance, transaction history

| # | Bước | Nội dung | Output |
|---|---|---|---|
| 1 | **Identify scope** | Story flows + affected flows từ code diff / SA2 | Smoke test checklist |
| 2 | **Smoke test** · 15–30 phút | Chạy critical flows trong scope trên STG | Smoke result |
| 3 | **GO/NO-GO** | Pass → Story PASSED. Fail → phân loại finding (Quick/Medium/Complex) | GO/NO-GO |
| 4 | **Gửi report** | QA Audit Report → Dev Lead + PO + QC Lead | Report |

**GO/NO-GO conditions:**

| Quyết định | Điều kiện |
|---|---|
| ✅ GO | L1+L2+L3 pass · Không có S1/S2 open |
| ⚠️ GO WITH CONDITIONS | S2 open + workaround + PO ack văn bản |
| ❌ NO-GO | S1 open · SonarQube Blocker/Critical · TC gap >30% · Smoke fail |

### Tier 2 — Audit nhẹ hơn (khi có override trigger)

| Trigger | Lý do |
|---|---|
| Change động đến financial flow (pricing, voucher, transaction) | Risk cao dù change nhỏ |
| Data migration / schema change | Không thể rollback dễ |
| Post-incident (liên quan đến Tier 2 flow) | Cần chứng minh không tái phát |
| Nhiều Tier 2 tickets cùng một release | Risk cộng dồn |
| PM/Dev Lead request | Discretionary — QA phán xét |

**Khi audit Tier 2 "lighter":**
- L1: Kiểm tra SonarQube + AC — bỏ qua SA evidence chi tiết nếu không phải financial
- L2: Sample 30–50% TCs thay vì toàn bộ
- L3: Không bắt buộc — rely on QC sweep (trừ khi có override trigger cao)
- Không có formal QA Audit Report — ghi nhận vào weekly report là đủ

**Khi không có QA gate:** Dev Lead và QC Lead tự sign off. Nếu bug escaped → QA sẽ RCA tại sao không có gate.

---

## Quy trình 2 — Post-Incident RCA

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
    │             │
    └──────┬──────┘
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

## Quy trình 3 — Quarterly Review

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

### Mô tả các bước

**Bước 1 — Pull metrics từ Redmine + SonarQube**

Lấy data cho cả quarter, không chỉ sprint cuối:
- Redmine: đếm DoR fail, bug severity, release block count
- SonarQube: compliance rate trung bình theo project
- QA log: audit cycle time trung bình, findings pattern

**Bước 2 — Đánh giá theo 3 trục**

| Trục | Câu hỏi | Hành động nếu lệch |
|---|---|---|
| Metrics | Có trend nào xấu đi không? | Xác định root cause, đưa vào backlog |
| Process | Có bước nào team hay skip hoặc làm sai không? | Update playbook / DoR / checklist |
| Tier | Ticket nào bị audit sai tier (quá nặng hoặc quá nhẹ)? | Adjust classification matrix |

**Bước 3 — Quarterly QA Report**

Gửi đến QA Manager + QC Manager + Dev Leads, bao gồm:
- Metrics dashboard (vs target + trend)
- Top 3 process gaps phát hiện trong quarter
- Tier adjustment đề xuất (nếu có)
- Process improvement backlog cho Q+1

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
| STG Gate: Smoke test L3 + GO/NO-GO | ≤2 giờ | QA |
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
| GO/NO-GO decision | Dev Lead + PM + QC Lead | QA Audit Report | Sau Smoke test L3 |
| Spot-check patterns | QC Lead + QC Manager | Weekly Quality Report | Thứ Sáu |
| Post-incident RCA | Dev Lead + PM + QC Lead | Process RCA doc | ≤1 ngày sau incident |
| Process improvement | QA Manager + QC Manager + Dev Leads | Quarterly Report | Đầu quarter tiếp |

**Nguyên tắc:**
- Findings kèm **evidence cụ thể**, không phán xét chung chung
- Tone: "Spec mục X không có TC tương ứng" — không "QC làm thiếu"
- QA không blame cá nhân — focus vào process gap

---

## Roadmap — Process Evolution

### Phase 1 — Hiện tại (manual impact identification)

Đang áp dụng. L3 scope xác định thủ công: QA đọc code diff + SA2 risk matrix → tự map ra affected flows.

**Điều kiện để chuyển Phase 2:** automation coverage ≥70% + CI/CD pipeline stable + có owner maintain tag map.

---

### Phase 2 — Tag-based Test Impact Analysis (khi automation mature)

**Nguyên lý:**
- Dev gắn tag cho mỗi module/service bị thay đổi trong story
- CI/CD tự động trigger automation test cases tagged với các module đó
- QA L3: validate tag accuracy + manual spot-check high-risk flows (không re-run toàn bộ)

**Flow Phase 2:**
```
Story thay đổi module X
    │
    ├─ Dev tag: ["module_x", "flow_a", "flow_b"]
    │
    ├─ CI/CD: trigger automation tests tagged với những flows đó
    │          Kết quả: auto pass/fail
    │
    └─ QA L3: verify tag đúng chưa + manual smoke flow có risk cao nhất
```

**Lợi ích so với Phase 1:**
- Loại bỏ rủi ro QA miss affected flow do không biết dependency
- Giảm manual L3 time — QA tập trung vào validation thay vì execution
- Coverage tăng mà không tăng QA workload

**Rủi ro cần quản lý:**
- Tag stale: Dev thêm dependency mới nhưng không update tag → automation pass nhưng thực ra miss flows
- Mitigation: QA review tag accuracy là một phần của L1 audit khi Phase 2 active

---

## Liên kết

- [QA Role Definition](qa-role-definition.md) — Scope và trách nhiệm
- [QA Audit Checklist](qa-audit-checklist.md) — Checklist chi tiết Layer 1/2/3
- [QA Quality Standards](qa-quality-standards.md) — Tiêu chuẩn Go/No-Go
- [QC Classification Matrix](qc-classification-matrix.md) — Tier nào cần audit mức nào
- [QA Audit Report template](../templates/report-qa-audit.md)
- [Dev Test DoR](dev-test-dor.md) — Evidence QA kiểm tra ở Layer 1
