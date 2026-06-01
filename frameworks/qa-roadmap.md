# QA Function Roadmap 2026–2027

**Owner:** Dung Nguyễn — QA Manager | **Hiệu lực:** 2026-05-01 | **Cập nhật:** 2026-06-01

---

## 1. Bối cảnh

QA Team được thành lập chính thức từ **2026-05-01** — độc lập với QC (QC nay báo cáo về Dev Lead).

| | Trước T5/2026 | Từ T5/2026 trở đi |
|---|---|---|
| QA/QC cùng một team | ✅ | ❌ |
| QA báo cáo Dung | — | ✅ |
| QC báo cáo Dev Lead | — | ✅ |
| Gate độc lập | ❌ | ✅ |

### Áp lực từ AI-augmented development
AI giúp team làm nhanh hơn nhưng đồng thời tăng risk:
- Dễ hiểu sai requirement
- Thiếu test case / miss edge case
- Dễ phát sinh lỗi production
- Chưa có lớp kiểm soát quality xuyên suốt SDLC
- Leadership chưa có góc nhìn tổng thể về quality & risk

**Hiện trạng QC:** Chủ yếu test feature theo sprint, gắn với Scrum team.
**Khoảng trống:** Không có gate độc lập trước release, không đo lường chất lượng AI-generated code, không có visibility tổng thể.

**Thách thức cốt lõi:** QA function mới 100% — process, tools, templates đang build từ đầu trong khi vẫn phải vận hành release gate hàng ngày.

---

## 2. Tầm nhìn 2027

> QA là **independent release intelligence layer** — không test lại, không thay QC, mà đảm bảo toàn bộ quy trình chất lượng hoạt động đúng trước mỗi release.

### Engagement model
QA **tham gia sau khi Dev bàn giao task lên staging** — vai trò = **independent gate + process intelligence**. Không test lại từ đầu, focus vào evidence audit + smoke test + Go/No-Go decision.

### Kết quả kỳ vọng cuối 2027
- 100% Tier 1 releases có QA audit trail trước khi ship
- DoR compliance rate ≥ 90% (Dev chuẩn bị evidence đúng lần đầu)
- Audit cycle time ≤ 4h/release (QA efficient, không là bottleneck)
- Bug escaped production giảm ≥ 40% so với baseline 2026
- QA team: 1 QA Manager + 2 QA Engineers vận hành độc lập

---

## 3. Mục tiêu QA — 7 trụ cột

1. Giảm lỗi production
2. Tăng chất lượng release
3. Kiểm soát quality xuyên suốt SDLC (từ DoR → release)
4. Chuẩn hoá review & audit
5. Improve testcase / requirement coverage
6. Tăng visibility quality & risk cho leadership
7. Hỗ trợ team adapt workflow có AI

---

## 4. Resource Model & Ratio

| Metric | Số |
|---|---|
| Avg QA effort | ~2h/story |
| Avg Scrum team effort | ~5 mandays/story |
| **QA ratio** | **~1 : 20** (1 QA cho ~20 thành viên scrum) |

Ratio này là baseline để quyết định headcount khi scope mở rộng — vượt 1:20 liên tục 2 tháng = trigger tuyển.

---

## 5. 4 Workstreams — Cấu trúc vận hành xuyên suốt

### WS1 — Process & Standards
- Xây dựng và chuẩn hoá quy trình QA (DoR, DoD, audit flow)
- Chuẩn hoá checklist & release standard cho 3 scrum team
- Improve workflow QA – QC – Dev (interface rõ ràng, không overlap)
- Quarterly process health review

### WS2 — Audit & Release Gate
- Audit chất lượng dự án (Tier 1+2 theo classification matrix)
- 4 layers: L1 Dev evidence → L2 QC work → L3 Coverage intelligence → L4 Smoke test STG
- **Tích hợp smoke test vào CI/CD pipeline mỗi khi Dev đưa lên STG**
- **Audit độ coverage tài liệu (spec/AC/TC) tự động trong pipeline**
- Go/No-Go decision cho Tier 1 release

### WS3 — Quality Intelligence
- Theo dõi: production issue, lỗi lặp lại, release quality
- Weekly Quality Report cho Dev Lead & Leadership
- Build dashboard theo dõi quality & risk (Redmine + SonarQube + pipeline data)
- Quarterly QA Review

### WS4 — AI Quality Governance
- **Đo lường & đảm bảo chất lượng sản phẩm sử dụng AI**
  - AI defect rate vs human-written code
  - Hallucination rate trong test cases / TC do AI sinh
  - Coverage gap khi AI generate TC
- **Theo dõi effort & mức độ sử dụng AI của toàn team**
  - % Apply AI per ticket (CF[78] Redmine)
  - AI saving (effort actual vs estimated)
  - Per-team / per-role AI adoption trend
- Hỗ trợ team adapt workflow có AI (training, playbook)
- AI provenance tracking + risk advisory

---

## 6. Project Audit Scope (T6/2026)

| Scrum Team | Audit chính thức | Pilot (T6–T7, đánh giá T8) |
|---|---|---|
| **Voucher Services** | GiftPort · Partner Integration | — |
| **User Solutions** | P2P ZaloGift · UBND – Gotit App | — |
| **Client Solutions** | — | Campaign Custom · Campaign Tool V3 · Loyalty/RewardHub |

**Rule:** Client Solutions pilot 1–2 tháng → đánh giá effort + value → quyết định audit chính thức từ T8/2026.

---

## 7. Phase Plan — 4 Phases

### Phase 1 — Bootstrap · T5–T7/2026 · _QA Manager solo_

**Mục tiêu:** Đặt nền móng — process live, tools sẵn, bắt đầu tuyển

| Việc cần làm | Workstream | Trạng thái | Deadline |
|---|---|---|---|
| QA Audit process L1/L2/L3/L4 | WS1 | ✅ Done | 2026-05-01 |
| QA-Audit-Checklist.xlsx template | WS1 | ✅ Done | 2026-05-08 |
| SKILL.md `/qa-audit` tự động hoá workflow | WS2 | ✅ Done | 2026-05-12 |
| audit-comment-templates.md | WS1 | ✅ Done | 2026-05-12 |
| process-testing-2026.md — public doc | WS1 | ✅ Done | 2026-05-12 |
| BookStack: QA Gate pages (6736, 6746) | WS1 | ✅ Done | 2026-05-20 |
| Audit 4 project chính thức (Voucher + User Solutions) | WS2 | 🔄 Running | T6–T7 |
| Pilot Client Solutions (3 project) | WS2 | 🔲 Open | T6–T7 |
| Smoke test pipeline PoC trên 1 repo | WS2 | 🔲 Open | T6 |
| Doc coverage audit script PoC | WS2 | 🔲 Open | T7 |
| Baseline metrics: production issue rate, audit cycle time, % Apply AI | WS3, WS4 | 🔲 Open | Cuối T6 |
| Weekly Quality Report bắt đầu chạy đều | WS3 | 🔲 Open | T7 |
| Đăng JD, bắt đầu tuyển QE1 | — | 🔲 Open | T6/2026 |

**Rủi ro chính:** QA Manager = Single Point of Failure — mọi release phụ thuộc vào Dung. Mitigation: ưu tiên tuyển QE1.

**KPIs cuối Phase 1:**
- Audit coverage Tier 1 (4 project chính thức): ≥ 80%
- Audit cycle time: ≤ 1 ngày/ticket (hiện tại ~2–3h)
- Smoke test pipeline live ≥ 1 project
- Baseline metrics đã đo: DoR compliance %, TC gap rate, % Apply AI per team

---

### Phase 2 — First QE · T8–T10/2026 · _Onboard QE1 + Scale audit_

**Mục tiêu:** QE1 onboard, handle Tier 2 độc lập sau 3 tháng ramp-up. Mở rộng audit + pipeline integration.

**Ramp-up plan QE1:**

```
Tháng 1 (T8): Shadow — QE1 xem Dung audit, không tự làm
  └── Đọc: process-testing-2026.md, qa-audit-checklist.md, qa-skills-framework.md
  └── Chạy /qa-audit dưới supervision
  └── Deliverable: Tự audit 1 Tier 2 ticket, Dung review

Tháng 2 (T9): Assisted — QE1 làm, Dung review trước khi post
  └── Handle tất cả Tier 2 với Dung review
  └── Observe Tier 1 audit cùng Dung
  └── Deliverable: Audit accuracy ≥ 85%, cycle time ≤ 1 ngày

Tháng 3 (T10): Independent — QE1 tự handle Tier 2
  └── Tier 2 independent (Dung spot-check 20%)
  └── Assist Tier 1 cùng Dung
  └── Deliverable: 0 false positive, 0 missed critical findings
```

**Song song Phase 2:**
- Đánh giá pilot Client Solutions → quyết định audit chính thức (T8)
- Smoke test pipeline mở rộng sang ≥ 2 project (T8)
- Doc coverage audit auto trên mọi STG push (T9)
- AI quality metrics dashboard v1 live (T8)
- AI defect rate + % Apply AI tracking cross-team (T9–T10)
- Tuyển QE2 (target start T10–T11)

**KPIs cuối Phase 2:**
- QE1 handle Tier 2 độc lập: ≥ 90% accuracy
- Audit coverage Tier 1+2: ≥ 90%
- Smoke test pipeline trên ≥ 2 project
- AI quality dashboard live, báo cáo bi-weekly
- QE2 tuyển thành công và start

---

### Phase 3 — Full Team · T11/2026–Q1/2027 · _Team vận hành_

**Mục tiêu:** QE2 onboard, team vận hành đủ coverage, metrics tracking live

**Phân công mục tiêu:**

| Role | Tier 1 | Tier 2 | Tier 3 | Governance |
|---|---|---|---|---|
| QA Manager (Dung) | Lead + complex cases | Spot-check | N/A | Full |
| QE1 (sau T10) | Assist + shadow | Independent | Spot-check occasional | N/A |
| QE2 (sau T11) | Shadow | Assisted → Independent | — | N/A |

**Ramp-up QE2:** Tương tự QE1 — 3 tháng (T11, T12, T1/2027)

**Process deliverables Phase 3:**
- Quarterly QA Review process hoạt động (lần đầu: T12/2026)
- Weekly Quality Report từ cả team
- Metrics dashboard live (pull từ Redmine + SonarQube + pipeline)
- Tổng kết H2/2026, plan H1/2027

**KPIs cuối Phase 3:**
- Coverage Tier 1+2: 100%
- DoR compliance rate: ≥ 85%
- Bug escaped production: trending down (Q4 vs Q2/Q3)
- Audit cycle time Tier 1: ≤ 4h
- Smoke test pipeline trên 3 team
- Doc coverage audit auto: all STG push

---

### Phase 4 — Mature QA · 2027 · _Process intelligence_

**Mục tiêu:** AI-assisted audit, self-improving process, QA là competitive advantage

**4 hướng phát triển 2027:**

**1. AI-Assisted Audit (H1/2027)**
- Tự động parse L1 evidence từ Redmine → highlight gaps
- AI pre-screen SonarQube metrics, flag anomalies
- QA focus vào judgment + edge cases — không còn data collection thủ công

**2. Predictive Quality (Q2/2027)**
- Trend analysis: ticket patterns nào hay dẫn đến production bug?
- Risk score tự động dựa trên ticket type + dev history
- QA có data để ưu tiên audit depth

**3. Shift-Left QA Involvement (Q3/2027)**
- QA tham gia Grooming để flag spec gaps sớm
- QA define DoD cùng PO trước khi sprint start
- Reduce rework cost — catch gap ở spec phase thay vì release gate

**4. QA Metrics as Company KPI (Q4/2027)**
- DoR compliance, bug escaped, audit cycle time → báo cáo lên CPO/CTO
- QA metrics inform engineering planning (sprint capacity, tech debt priority)

---

## 8. Audit Effort Thực Tế

> Nguồn: audit #70888 (GiftPort · Tier 2) và #70145 (Loyalty AI · Tier 1) — 2026-05-18

| Tier | Layer | Effort thực tế |
|---|---|---|
| Tier 1 (full audit) | L1 → L4 + TC Quality | ~3–4 giờ/story |
| Tier 2 (spot-check) | L1 → L3 | ~1–1.5 giờ/story |
| Tier 3 | Không audit | 0 |

**Ước tính workload mỗi tháng** (3 Scrum teams, 2 sprint/tháng):

| Tier | Tỷ lệ | Số story/tháng | Effort thủ công |
|---|---|---|---|
| Tier 1 | ~30% | ~5–14 stories | 20–56 giờ |
| Tier 2 | ~40% | ~7–19 stories | 11–28 giờ |
| **Tổng** | | | **~30–84 giờ/tháng** |

**Ngưỡng quyết định headcount:**
- Effort ≤ 40h/tháng → tiếp tục solo, focus automation
- Effort > 40h/tháng liên tục 2 tháng → tuyển QE1 ngay
- Volume Tier 1 > 15 stories/tháng → escalate lên leadership
- Vượt ratio 1:20 liên tục 2 tháng → trigger headcount review

---

## 9. Roadmap Manual → Automated Audit

```
T5–T6       T7–T8       T9–T10      T11–T12     Q1/2027
Manual   → Evaluate → Semi-Auto → Full Team → AI-Assisted
            & Auto L1   L1+L2                  Audit
```

**T5–T6 — Manual baseline + Pipeline PoC**
- Audit thủ công 100% Tier 1 (4 project), spot-check Tier 2
- Pilot Client Solutions song song
- Smoke test pipeline PoC trên 1 repo
- Đo volume, effort/story, bottleneck ở layer nào
- Output: data để quyết định headcount T7–T8

**T7–T8 — Evaluate & Tự động hoá L1 + Pipeline scale**
- Đánh giá 2 tháng data → quyết định headcount
- Đánh giá pilot Client Solutions → audit chính thức/drop
- Tự động hoá L1: script pull Redmine evidence, check SonarQube, flag thiếu sót
- Smoke test pipeline ≥ 2 project
- Doc coverage audit script live
- Mục tiêu: giảm Tier 1 từ 3–4h → 2h/story

**T9–T10 — Semi-automated + AI metrics**
- L1 tự động hoàn toàn — QA chỉ review kết quả
- L2 spot-check tự động: format TC, bug count, status
- AI quality dashboard live
- Doc coverage audit auto trên mọi STG push
- Tier 2 giảm từ 1.5h → 30 phút/story

**T11–T12 — Full team vận hành**
- QE1 handle Tier 2 độc lập (nếu đã tuyển)
- Weekly quality report tự động generate
- Quality dashboard live
- Smoke test pipeline 3 team

**Q1/2027 — AI-Assisted Audit**
- AI parse L1 evidence, flag anomaly tự động
- QA review output và ra quyết định — không còn data collection thủ công
- Audit cycle time Tier 1: ≤ 2h

---

## 10. KPI Summary — Theo từng mốc

| Metric | T8/2026 | T10/2026 | T12/2026 | Q4/2027 |
|---|---|---|---|---|
| Audit coverage Tier 1 | ≥ 80% | ≥ 90% | 100% | 100% |
| Audit coverage Tier 2 | Spot-check | ≥ 80% | ≥ 90% | 100% |
| DoR compliance rate | Baseline | ≥ 70% | ≥ 85% | ≥ 90% |
| Audit cycle time (Tier 1) | 3–4h (manual) | ≤ 2h (semi-auto) | ≤ 2h | ≤ 1h |
| Smoke test in pipeline | 1 project | 2 project | 3 project | All |
| Doc coverage audit auto | Script live | All STG push | All STG push | All |
| Bug escaped production | Baseline | Trending ↓ | -20% | -40% |
| Lỗi lặp lại rate | Baseline | <15% | <12% | <10% |
| TC coverage gap rate | Baseline | < 25% | < 20% | < 15% |
| L1 automation | 0% | 100% | 100% | 100% |
| AI defect rate vs human code | Baseline có | Trending ↓ | Parity | Parity sustained |
| % Apply AI tracked (team-level) | All 3 teams | All 3 teams | All 3 teams | All 3 teams |
| AI usage report cadence | Bi-weekly | Bi-weekly | Bi-weekly | Bi-weekly |
| QA team size | 1 (solo) | 1–2 | 2–3 | 3 |

---

## 11. Dependencies & Điều kiện thành công

| Dependency | Từ ai | Tại sao quan trọng |
|---|---|---|
| Dev thực hiện đúng L1 evidence format | Dev Leads (Nghĩa, các TL) | QA audit chỉ có ý nghĩa khi Dev cung cấp đủ evidence |
| QC set "QC Approved" đúng quy trình | QC (báo cáo Dev Lead) | Trigger rolling audit |
| Pipeline access (CI/CD) | DevOps | Cần để integrate smoke test + doc coverage check |
| SonarQube accessible + token live | DevOps | L1 + AI metrics depend on data |
| Approve headcount QE1 + QE2 | Leadership | Phase 2+3 không thể thực hiện không có người |
| Không override NO-GO không có văn bản | PM + Dev Lead | Gate mất giá trị nếu bị bypass |
| Redmine CF[78] % Apply AI được fill đều | All teams | Đo AI adoption depends on data quality |

---

## 12. Rủi ro & Mitigation

| Rủi ro | Khả năng | Impact | Mitigation |
|---|---|---|---|
| QA Manager = Single Point of Failure (solo Phase 1) | Cao | Cao | Ưu tiên tuyển QE1, Tier 2 spot-check only |
| Không tuyển được QE phù hợp | Trung bình | Cao | JD rõ ràng (đã có), phỏng vấn có structured test audit task |
| Dev không adopt L1 evidence format | Cao | Cao | Track compliance, weekly report, escalate qua Dev Lead |
| Audit trở thành bottleneck release | Trung bình | Cao | Audit cycle time KPI, SKILL.md automation giảm thời gian |
| QE1/QE2 không đủ independent judgment | Trung bình | Cao | 3-month ramp-up, shadow trước khi independent |
| Pipeline smoke test chậm release | Trung bình | Trung bình | Time budget ≤ 5 phút, fail fast Critical/High only |
| Doc coverage audit thành bureaucracy | Trung bình | Trung bình | Auto trong pipeline, không phải manual review |
| AI metrics khó đo chính xác | Trung bình | Trung bình | Bắt đầu từ proxy metrics (CF[78], time spent), refine dần |

---

## 13. Mốc Quyết Định

| Mốc | Thời gian | Câu hỏi | Quyết định |
|---|---|---|---|
| **Headcount Gate** | T7–T8/2026 | Effort > 40h/tháng liên tục? Workload > 1:20? | Tuyển QE1 / Tiếp tục solo + automation |
| **Pilot Evaluation** | T8/2026 | Pilot Client Solutions có value? | Audit chính thức / Drop / Tiếp tục pilot |
| **L1 Automation Gate** | T8/2026 | Script Redmine + SonarQube live? | Deploy / Delay |
| **Pipeline Smoke Test Gate** | T9/2026 | Smoke test có giảm production issue? | Scale ra 3 team / Refine |
| **QE1 Ramp Check** | Cuối T10/2026 | QE1 handle Tier 2 được chưa? | Independent / Extend ramp |
| **AI Metrics Gate** | T10/2026 | AI quality metrics có actionable? | Báo cáo leadership / Iterate |
| **Phase 3 Review** | T12/2026 | Team vận hành được chưa? | Metrics-based decision cho 2027 |
| **AI Audit Gate** | Q1/2027 | AI có giúp giảm audit time ≥ 50%? | Invest thêm / Pivot |

---

## 14. Liên kết tài liệu

| Tài liệu | Mô tả |
|---|---|
| [QA Role Definition](qa-role-definition.md) | Scope, ranh giới rõ ràng của QA |
| [QA Process](qa-process.md) | 5 quy trình chi tiết vận hành |
| [QA Job Description](qa-job-description.md) | Dùng cho tuyển QE1, QE2 |
| [QA Skills Framework](qa-skills-framework.md) | Must-have + ramp-up checklist |
| [Process Testing 2026](process-testing-2026.md) | Public — QA Gate cho toàn PD |
| [QA Audit Checklist](qa-audit-checklist.md) | L1/L2/L3/L4 đầy đủ |
| [QC Transformation Roadmap](~/second-brain/projects/qc-transformation/roadmap.md) | Roadmap QC team song song |
| [Plan chi tiết 2026](~/second-brain/projects/qc-transformation/qa-plan-2026.md) | Monthly breakdown |
