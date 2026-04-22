# Project Roster — Dự án đang chạy

**Cập nhật:** 2026-04-22

---

## Lumina — GotIt UAT Tracker {#lumina}

**Vai trò:** Technical PM (UAT từ phía GotIt)
**Trạng thái:** 🟡 In Progress — P1 Dev ~74%
**Nhà phát triển:** AIP (bên thứ 3)
**Timeline:** 01/04/2026 → 03/07/2026 (13 tuần)
**Tiến độ:** ~26.6%

### 3 Phases

| Phase | Nội dung | UAT Deadline | Trạng thái |
|---|---|---|---|
| **P1** | Foundation & Core (5 apps) | **29/04/2026** | 🟡 Dev ~74% |
| **P2** | Automation (4 apps) | 29/05/2026 | ⬜ Chưa bắt đầu |
| **P3** | Advanced (6 apps) | 03/07/2026 | ⬜ Dev ~4% |

### P1 Apps (UAT 29/04 — cần chuẩn bị)
- Internal Knowledge Chatbot
- Document Review & Analysis
- Product Research & Catalog Builder
- Document Generator
- Candidate Evaluation

### Quy trình UAT
```
Vendor handover → GotIt UAT → Vendor fix → GotIt confirm
```

### Links
- Notes: `~/second-brain/projects/lumina/lumina.md`
- UAT Log: trong file lumina.md

---

## Biệt Đội AI

**Vai trò:** PM
**Trạng thái:** 🟢 Active
**Báo cáo:** Leadership bi-weekly

### Mô tả
Dự án AI áp dụng vào product development workflow. Full Ownership model — Dev + QC + BA + PM đều trong đội. Mục tiêu leverage AI để tăng năng suất toàn team PD.

### Team
- PM: Dung Nguyễn
- Members: Dev, QC, BA từ nhiều scrum teams

### Quyết định đã chốt
- Full Ownership toàn team PD (không phải chỉ 1 scrum team)
- Tỉ lệ contribution AI được đo và track

### Links
- Meetings: `~/second-brain/meetings/` (tag: biet-doi-ai)
- Notes: `~/second-brain/projects/`

---

## QC AI Strategy — 5 Subagents {#qc-ai-strategy}

**Vai trò:** Lead
**Trạng thái:** 🟢 Active
**Timeline:** Cả năm 2026

### 5 Subagents
1. **Requirement Clarifier** — phát hiện mâu thuẫn trong spec
2. **Risk Scout** — mở rộng risk matrix, edge cases
3. **Test Designer** — sinh test ideas theo risk
4. **Bug Hunter** — tìm bug khó (negative, boundary, security)
5. **RCA Assistant** — phân tích root cause

### KPIs
- ≥2 subagent thử nghiệm thành công
- Giảm effort 20–30%

### BookStack
- https://bookstack.gotit.vn/books/pd-c-qc-team/page/plan-apply-ai

---

## Applitools Visual AI Testing {#applitools-visual-testing}

**Owner:** Thắng Trương (Dung là sponsor)
**Trạng thái:** 🟡 In Progress
**Timeline:** 27/02/2026 → 12/06/2026 (15 tuần)

### Mục tiêu
- Giảm thời gian viết & maintain visual test scripts
- Phát hiện lỗi UI/UX trên nhiều browser, screen size
- Tích hợp với Robot Framework hiện có

### Milestone quan trọng
- Tuần 6 (03/04): Layout mode (quan trọng)
- Tuần 13 (29/05): Script cho luồng nghiệp vụ thật
- Tuần 14 (05/06): Tích hợp Jenkins CI/CD
- Tuần 15 (12/06): **Demo + báo cáo cho team**

### BookStack
- https://bookstack.gotit.vn/books/pd-c-qc-team/page/c-thang-truong-research-2026

---

## Shift-Left Testing + SonarQube {#shift-left-testing}

**Vai trò:** Lead phía QC
**Trạng thái:** 🟡 In Progress
**Start:** Q4 2025

### Targets
- Code coverage ≥60% trên SonarQube cho tất cả repo
- Giảm bug QC detect ≥30%
- Giảm thời gian regression ≥20%

### Workflow
```
Dev viết code → viết unit test → CI/CD trigger Sonar scan
→ Coverage <60% hoặc có blocker → pipeline FAIL
→ Dev fix trước khi merge → QC nhận build đã QA sẵn
```

### Role của Dung
- Xây dựng quy trình đo coverage
- Báo cáo 2 tuần/lần cho leadership

---

## Redmine Dashboard {#redmine-dashboard}

**Vai trò:** Builder & Owner
**Trạng thái:** 🟢 Active
**URL:** http://localhost:8501

### Stack
- Python + Streamlit
- Path: `~/second-brain/tools/redmine-dashboard/`
- Run: `~/second-brain/tools/redmine-dashboard/run.sh`

### 3 Tabs
| Tab | Nội dung |
|---|---|
| Tab 1 — Anomaly | Stories quá due date + stories có nhiều bugs |
| Tab 2 — Monthly Report | Bug summary, story detail, production issues, quality analysis |
| Tab 3 — AI Adoption | % Apply AI theo team Dev/QC, estimated vs actual effort |

---

## IPO — Internal Process Optimization (ERP Odoo) {#ipo}

**Vai trò:** Technical PM (được add vào 25/03/2026)
**Trạng thái:** 🟡 In Progress
**Vendor phát triển:** AI POWER (Dương là Dev Lead phía vendor)
**Đầu mối phía GotIt:** Vy Diệp Thúy (dẫn dắt từ đầu) · Hiền Bùi Thị (Product/API)

### Mô tả
Triển khai ERP Odoo để tập trung hóa toàn bộ quy trình nội bộ — thay thế Excel/email/SharePoint rời rạc. Odoo kết nối với CMS, FAST (kế toán), Biz Order, Gift Port.

### Sprint Status

| Sprint | Nội dung | Trạng thái |
|---|---|---|
| **Sprint 1** | CRM – Sales (Lead, Opportunity, Client, Phân quyền) | ✅ Done UAT round 3, chờ API go live |
| **Sprint 2** | Hợp đồng & Sản phẩm | 🔄 UAT round 2 |
| **Sprint 3** | Báo giá & PO Management | 🔄 UAT round 1 |
| **Sprint 4–5** | Tích hợp sâu CMS/BIZ/FAST | ⬜ Chưa bắt đầu |
| **Merchant Mgmt** | BRD đã khảo sát xong | ⬜ Cần viết BRD |
| **Vendor Mgmt** | BRD đã khảo sát xong | ⬜ Cần viết BRD |

### Vướng mắc đang có
- API Sprint 1 bị delay (start thực tế 20/03 thay vì 09/03)
- Data migration chờ Sales team chuẩn bị data + chốt ngày freeze
- BRD Merchant & Vendor chưa hoàn thiện

### Người liên hệ chính
- **Vy Diệp Thúy** — nắm sâu nhất, hỏi mọi thứ về lịch sử dự án
- **Hiền Bùi Thị** — đầu mối Product/API phía GotIt
- **Dương (AI POWER)** — Dev Lead phía vendor, liên hệ qua Zalo group

### Links
- Notes: `~/second-brain/projects/ipo/`
- Sprint detail: `~/second-brain/projects/ipo/IPO-TONG-HOP-v2.md`

---

## AI Measurement {#ai-measurement}

**Vai trò:** Lead
**Trạng thái:** 🟡 In Progress — Phase 5e đang chờ data

### Mục tiêu
Đo lường hiệu quả thực tế của việc áp dụng AI vào quy trình QC, so sánh với baseline tháng 1–2/2026.

### Tiến độ
| Phase | Nội dung | Trạng thái |
|---|---|---|
| 5a | Convention + Baseline | ✅ Done (174 stories tháng 1, 46 tháng 2) |
| 5b | Time Entry Comment Parser [AI:X] | ✅ Done |
| 5c | Dashboard Tab AI Adoption | ✅ Done |
| 5d | Integration test | ✅ Done |
| **5e** | **Chạy thật cuối tháng** | 🔲 Chờ data thực tế |

### Links
- Plan: `~/second-brain/projects/ai-measurement/plan.md`
- Tab 3 Redmine Dashboard — AI Adoption

---

## PM Project PD — 30 Đầu Mục Công Việc {#pm-pd}

**Vai trò:** Technical PM
**Trạng thái:** 🟢 Active
**Start:** 17/03/2026 | **Báo cáo:** Bi-weekly (bắt đầu 20/04/2026)

### Mô tả
Quản lý 30 đầu mục công việc nội bộ PD (improve/nâng cấp hệ thống). Dung tracking tiến độ và báo cáo leadership mỗi 2 tuần.

### 30 Projects theo nhóm

| Nhóm | Số | PIC liên quan đến Dung |
|---|---|---|
| Engineering | 5 | #1 AI-assisted SDLC, #4 System Doc Standardization |
| Security | 5 | — |
| Reliability/DevOps | 5 | — |
| FinOps/Scalability | 5 | — |
| Quality/Architecture | 5 | #13 Core Flow Automation *(Cancelled → apply AI)*, #14 AI-assisted Test Design |
| Data | 5 | — |

### Lịch báo cáo
Bi-weekly: 20/04 · 04/05 · 18/05 · 01/06 · 15/06 · 29/06 (cuối Q2) → ...

### Links
- File tracking: `PD-Project-Management-2026.xlsx` (do Claude tạo 20/4)
- Notes: `~/second-brain/projects/pd-project/notes.md`

---

## QC Transformation 2026 {#qc-transformation}

**Vai trò:** Lead
**Trạng thái:** 🟡 In Progress
**Deadline:** 31/08/2026
**Nguồn yêu cầu:** Chi Minh + Si (06/03/2026)

### 3 Định hướng từ leadership
1. **Định vị lại scope QC** — tiêu chí bug tối đa cho Dev trước khi handover
2. **Định lượng lại workload** — sau khi apply tiêu chí + AI → xác định structure phù hợp
3. **Retrain skillset** — QC làm được việc ngoài testing trong toàn SDLC

### 4 Tracks

| Track | Nội dung | Timeline |
|---|---|---|
| A — Baseline & Dev Accountability | Đo lường cơ sở + tiêu chí Dev trước handover | T3 |
| B — AI vào QC Process | Tự động phân tích doc, sinh TC, tìm bug | T3–T12 |
| C — Auto Test + Log Bug | Katalon auto-run + Redmine auto-log | T3–T7 |
| D — QC Upskilling | BA, coding basic, support SDLC | T7 |

### Links
- `~/second-brain/projects/qc-transformation/`
- Roadmap: `qc-transformation/roadmap.md`
- Vision: `qc-transformation/vision-longterm.md`

---

## Capacity Dashboard {#capacity-dashboard}

**Vai trò:** Builder & Owner
**Trạng thái:** 🟢 Active
**URL:** http://localhost:8502
**Path:** `~/second-brain/tools/capacity-dashboard/`

### Mô tả
Tool hiển thị capacity + workload toàn PD team (BA + Dev + QC) từ Redmine. Trả lời 5 câu hỏi:
1. Team có đang over capacity không?
2. Với member hiện tại → làm được bao nhiêu task?
3. Avg time per task?
4. Nếu giảm effort (AI) → tăng throughput bao nhiêu?
5. Team nào đang overload nhất?

### Config
- Focus Factor: 0.70 | AI Target: 30% efficiency gain
- Run: `cd ~/second-brain/tools/capacity-dashboard && ./run.sh`

---

## Intern Training Program

**Vai trò:** Lead
**Trạng thái:** 🟡 Planning
**Timeline:** 2026 (cả năm)

### Mục tiêu
- Onboard QC interns: automation basics, test case writing, bug reporting
- ≥80% interns đạt milestone học tập
- 24 tuần chương trình

---

## Liên kết

- [Team Roster](team-roster.md)
- [Action Tracker](../actions/Action-Tracker.md)
- [Memory](../Memory.md)
