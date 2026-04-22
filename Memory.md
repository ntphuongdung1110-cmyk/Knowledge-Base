# Memory — Dung Nguyễn

> **Dành cho AI:** Đây là tài liệu onboarding. Đọc file này ở đầu mỗi session để có đầy đủ context trước khi trả lời bất kỳ câu hỏi nào.

**Cập nhật:** 2026-04-21

---

## 1. Tôi là ai

**Tên:** Nguyễn Thị Phương Dung
**Email:** dung.nguyen@gotit.vn
**Công ty:** GotIt — nền tảng e-gift/e-voucher hàng đầu Việt Nam
**Ngày vào:** 18/11/2019

### Hai vai trò song song

| Vai trò | Phạm vi | % thời gian |
|---|---|---|
| **QC Manager** | Quản lý QC team 10 người, định nghĩa quality standards toàn PD, SDET scope | 50% |
| **Technical PM** | Quản lý 30+ đầu mục PD, báo cáo leadership bi-weekly; PM Biệt Đội AI, PM Lumina | 50% |

**Hướng phát triển dài hạn:** SDET (Software Development Engineer in Test)

---

## 2. Công ty — GotIt

**Website:** gotit.vn | **Docs nội bộ:** bookstack.gotit.vn | **Redmine:** redmine.gotit.vn

### Sản phẩm chính
- **Consumer:** GotIt App, Voucher Link (v/g/e), Cover Link, Home Delivery, Miniapp ZaloPay
- **B2B:** Biz Order, Biz CMS, API (Biz/POS/SMS Gateway)
- **Campaign:** Campaign Tool, Campaign CMS, Campaign Loyalty, Scanit AI
- **Internal:** Admin, Internal Tools/HCV

### Scrum teams trong PD (cấu trúc mới — chốt 04/2026)
| Team | Sản phẩm phụ trách |
|---|---|
| **Voucher Service** | Core, CMS, Portal, Biz Order, IPO, Fraud |
| **User Solutions** | VC Link, Gotit App, GI Mini App, GI Web, Home Delivery, Webview, PD Support |
| **Client Solutions** | Scanit, Loyalty, Campaign |

---

## 3. QC Team — Cấu trúc

### Sơ đồ tổ chức (cấu trúc mới — chốt 04/2026)
```
QC Manager: Dung Nguyễn
├── Voucher Service
│   ├── Thùy Nguyễn (Senior, Key Person)
│   └── Hương Nguyễn (Senior)
├── User Solutions
│   └── Phú Đồng (Senior, Key Person)
├── Client Solutions
│   ├── Duyên Nguyễn (Senior TL)
│   ├── Tuyến Thân (QC Engineer)
│   ├── Tuyền Võ (Senior)
│   ├── Khải Lâm (Junior, từ 26/7/2025)
│   └── Trâm Võ (từ 03/11/2025)
└── Automation (Cross-team)
    └── Thắng Trương (Automation Engineer, từ 30/9/2024)
```

### Điểm chú ý quan trọng
- **Internal Tools** không có QC coverage (Tuyền chuyển sang Campaign từ 02/03/2026)
- **QA embedded** (6 người) báo cáo Dev Lead về process; Dung define quality standards
- **Thắng Trương** đang research Applitools Eyes Visual Testing (27/02 → 12/06/2026)

---

## 4. Dự án đang chạy

### [PM] Lumina — GotIt UAT Tracker
- **Vai trò:** Technical PM, UAT từ phía GotIt
- **Vendor:** AIP (bên thứ 3 phát triển)
- **Timeline:** 01/04/2026 → 03/07/2026 (13 tuần) | Tiến độ: ~26.6%
- **⚠️ P1 UAT deadline: 29/04/2026** (5 apps: Knowledge Chatbot, Doc Review, Product Research, Doc Generator, Candidate Evaluation)
- **Notes:** `~/second-brain/projects/lumina/lumina.md`

### [AI] Biệt Đội AI
- **Vai trò:** PM
- **Mô hình:** Full Ownership toàn team PD — Dev + QC + BA đều trong đội
- **Focus:** Áp dụng AI vào product development workflow
- **Tools theo dõi:** Redmine, BookStack

### [AI] QC AI Strategy — 5 Subagents
AI không thay thế QC mà là **subagent** hỗ trợ từng bước:
1. **Requirement Clarifier** — phát hiện assumption, mâu thuẫn trong spec
2. **Risk Scout** — mở rộng risk matrix, edge cases
3. **Test Designer** — sinh test ideas theo risk
4. **Bug Hunter** — gợi ý cách tìm bug khó
5. **RCA Assistant** — phân tích root cause

### [QC] Applitools Visual AI Testing
- **Owner:** Thắng Trương
- **Timeline:** 27/02 → 12/06/2026 (15 tuần)
- **Goal:** Demo + báo cáo cho team

### [PM] IPO — Internal Process Optimization (ERP Odoo)
- **Vai trò:** Technical PM (từ 25/03/2026)
- **Vendor:** AI POWER | **Đầu mối GotIt:** Vy Diệp Thúy, Hiền Bùi Thị
- **Sprint 1** (CRM Sales): Done UAT round 3, chờ API go live
- **Sprint 2–3**: Đang UAT | **Sprint 4–5**: Chưa bắt đầu
- **Notes:** `~/second-brain/projects/ipo/`

### [QC] AI Measurement
- **Trạng thái:** 5a–5d ✅ Done | 5e 🔲 chờ data cuối tháng
- **Mục tiêu:** Đo hiệu quả AI trong QC vs baseline T1–T2/2026
- **Notes:** `~/second-brain/projects/ai-measurement/plan.md`

### [QC] Shift-Left Testing + SonarQube
- **Target:** Code coverage ≥60%, giảm bug QC detect ≥30%
- **Role của Dung:** Xây dựng quy trình đo, báo cáo 2 tuần/lần

### [PM] PM Project PD — 30 đầu mục
- **Vai trò:** Technical PM, báo cáo bi-weekly cho leadership
- **Scope:** 30 projects (Engineering, Security, DevOps, Quality, Data...)
- **PIC của Dung:** #1 AI-assisted SDLC, #4 System Doc Standardization, #14 AI-assisted Test Design
- **Lịch báo cáo:** 20/04 · 04/05 · 18/05 · 01/06 · 15/06 · 29/06...
- **Notes:** `~/second-brain/projects/pd-project/notes.md`

### [QC] QC Transformation 2026
- **Vai trò:** Lead | **Deadline:** 31/08/2026
- **Yêu cầu từ:** Chi Minh + Si (06/03/2026)
- **4 Tracks:** A-Baseline · B-AI vào QC · C-Auto Test · D-Upskilling
- **Notes:** `~/second-brain/projects/qc-transformation/`

### [Tool] Capacity Dashboard
- **URL:** http://localhost:8502 | **Path:** `~/second-brain/tools/capacity-dashboard/`
- **Mục đích:** Capacity + workload toàn PD team từ Redmine

### [QC] Redmine Dashboard
- **Tech:** Python + Streamlit
- **Path:** `~/second-brain/tools/redmine-dashboard/`
- **URL:** http://localhost:8501
- **Mục đích:** Bug analytics, AI adoption tracking, monthly report

---

## 5. OKRs & KPIs 2026

### O1 — Tăng năng suất
| KPI | Target |
|---|---|
| Thử nghiệm AI thành công | ≥2 |
| Giảm thời gian chuẩn bị test data/TC | ≥20% |
| Bug critical giảm | ≥15% so 2025 |
| Bug recurrence giảm | ≥10% |
| Test flow được automation | ≥20% flow trọng tâm |
| Task có checklist & documentation | 100% |

### O2 — Phát triển con người
| KPI | Target |
|---|---|
| Interns đạt milestone học tập | ≥80% |
| Knowledge sharing sessions | ≥10/năm |

---

## 6. Tools & Systems

| Tool | Mục đích |
|---|---|
| **Redmine** (redmine.gotit.vn) | Bug tracking, story management, time log |
| **BookStack** (bookstack.gotit.vn) | Tài liệu nội bộ — 158+ books |
| **Jira** | Task management (một số team) |
| **SharePoint** | Test plan, template TC, training docs |
| **Robot Framework + Applitools** | Automation + Visual testing |
| **SonarQube** | Code coverage (Shift-Left) |
| **second-brain** (`~/second-brain/`) | Knowledge management cá nhân |

---

## 7. Communication Style

- Thích **ngắn gọn, súc tích** — không dài dòng
- Ưu tiên **bullet points** hơn paragraph
- Với leadership: đặt recommendation lên đầu, link với mục tiêu công ty
- Với team: nêu rõ impact đến công việc của họ, đủ context "tại sao"
- **Ngôn ngữ:** Tiếng Việt (mặc định), tiếng Anh khi technical term

---

## 8. Quy tắc routing cho AI

Khi ghi nhận thông tin từ cuộc họp hoặc session:

| Loại thông tin | Lưu vào |
|---|---|
| Action items | `actions/Action Tracker.md` |
| Quyết định quan trọng | File project liên quan trong `roster/` |
| Thông tin về thành viên team | `roster/team-roster.md` |
| Ghi chú họp | `~/second-brain/meetings/YYYY-MM-DD-tên.md` |
| Ghi chú 1:1 | `~/second-brain/people/tên.md` |
| Cập nhật dự án | `roster/project-roster.md` |

---

## 9. Liên kết nhanh

- [Home](Home.md) — Central hub
- [Team Roster](roster/team-roster.md) — QC team members
- [Project Roster](roster/project-roster.md) — Active projects
- [Action Tracker](actions/Action-Tracker.md) — Open tasks
- [Frameworks](frameworks/) — QC process, release checklist, 1:1
- [Templates](templates/) — Meeting notes, 1:1, sprint review
- **Second Brain:** `~/second-brain/` (nguồn đầy đủ, link chéo về đây)
