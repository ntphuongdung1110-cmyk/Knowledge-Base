# QA Role Definition — Quality Assurance

**Owner:** QA Manager | **Tạo:** 2026-05-07
**Báo cáo:** QA Manager → CTO/CPO
**Phạm vi:** Toàn bộ sản phẩm GotIt PD

---

## QA là gì — và không phải là gì

**QA (Quality Assurance)** là team độc lập, không tham gia trực tiếp vào quá trình test. QA là **process guardian** và **release gate**.

| | QC (Quality Control) | QA (Quality Assurance) |
|---|---|---|
| Làm gì | Test, tìm bug, sweep | Audit evidence, kiểm tra quy trình |
| Đầu vào | Code + spec | Evidence từ Dev + QC |
| Output | Bug report, sweep result | Audit report + Go/No-Go decision |
| Phạm vi | Từng ticket/feature | Toàn bộ quy trình + release gate |
| Báo cáo về | QC Manager | QA Manager |
| Quan hệ với sản phẩm | Test trực tiếp | Kiểm tra evidence — không test lại toàn bộ |

**QA KHÔNG làm:**
- Test lại toàn bộ sản phẩm (đó là QC)
- Viết test case cho Dev (đó là Dev + QC)
- Là bottleneck cho từng ticket — chỉ gate ở release level

---

## Trách nhiệm chính

### 1. Định nghĩa & duy trì chuẩn chất lượng
- Xây dựng và update: test templates, DoR checklist, quality standards
- Định nghĩa Definition of Done và acceptance criteria chuẩn
- Quyết định tiêu chí Go/No-Go cho từng loại release
- Review và update frameworks khi phát hiện gap qua audit

### 2. Audit độc lập
QA kiểm tra evidence — không phải test lại. Audit gồm 3 layer:

**Layer 1 — Dev evidence audit:**
- SA0–SA5 evidence có đính kèm đúng format không?
- Requirement coverage: Dev hiểu đúng spec của PO chưa?
- TC của Dev có cover hết risk matrix không?
- SonarQube: coverage ≥90%, 0 Blocker, 0 Critical?
- Security baseline: không hardcode credentials, input validation có không?

**Layer 2 — QC work audit (khi vẫn còn QC trong process):**
- TC format đúng template chuẩn chưa?
- Pass/Fail được đánh rõ ràng cho từng TC chưa?
- TC set cover hết acceptance criteria trong spec chưa?
- QA tự scan spec → liệt kê case còn thiếu trong TC

**Layer 3 — Smoke test trên staging (QA tự làm):**
- Chỉ critical flows — không phải full regression
- Thực hiện TRƯỚC khi Go/No-Go decision

### 3. Go/No-Go — Release Gate
- **Quyền và trách nhiệm độc quyền của QA**
- Không ai override được (chỉ CTO/CPO với documented exception)
- Phải có QA Audit Report trước mỗi Tier 1 release
- Tham khảo: [QA Audit Checklist](qa-audit-checklist.md)

### 4. Spot-check ngẫu nhiên
- Không theo sprint cố định — random sampling để tránh gaming
- Tier 1: spot-check 30–50% stories mỗi sprint
- Tier 2: spot-check 15–20% stories mỗi sprint
- Tier 3: spot-check khi có production bug

### 5. Phân tích & cải tiến quy trình
- Tracking bug lặp lại → root cause: quy trình nào bị miss?
- Đề xuất update DoR, playbook, template
- Quarterly process health review
- Kết quả audit → input cho upskilling của QC/Dev

---

## Khi nào QA thực hiện audit

| Trigger | Hành động | Output |
|---|---|---|
| Trước mỗi Tier 1 release | Audit Layer 1+2+3 + Go/No-Go | QA Audit Report |
| Sprint review (Tier 2) | Spot-check 15–20% stories | Weekly Quality Report |
| Sau production bug | RCA audit: process gap nào đã miss? | Process improvement action |
| Quarterly | Process health review toàn bộ | Quarterly QA Report |
| Random spot-check | Bất kỳ lúc nào — không báo trước | Brief audit note |

---

## Tiêu chí Go/No-Go

**GO** khi:
- Layer 1 + 2 + 3 pass (hoặc gap được documented và PM accept risk)
- Không có S1/S2 bug còn open
- SonarQube standards met (≥90%, 0 Blocker, 0 Critical)

**NO-GO** khi:
- Bất kỳ S1 bug còn open
- SonarQube Blocker hoặc Critical chưa clear
- TC coverage gap lớn (>30% spec cases không có TC)
- Security issue chưa resolve

**GO WITH CONDITIONS** khi:
- S2 bug còn open nhưng workaround exists — PM/Dev Lead ack bằng văn bản
- TC gap nhỏ (<20% spec) và covered bởi QA smoke test
- Conditions được documented rõ trong QA Audit Report

---

## Metrics QA theo dõi

| Metric | Target | Cách đo |
|---|---|---|
| DoR compliance rate | ≥80% (Q3/2026) | % tickets pass DoR lần đầu |
| TC coverage gap rate | <20% spec cases bị QA phát hiện thiếu | QA gap scan mỗi audit |
| SonarQube compliance | 100% tickets đạt threshold | SonarQube dashboard |
| Release blocked by QA | Trending down | Monthly count |
| Bug escaped to production | Trending down | Production bug count |
| Audit cycle time (Tier 1) | ≤4 giờ/release | Thực đo |

---

## Tools QA cần access

| Tool | Mục đích |
|---|---|
| **SonarQube** | Kiểm tra code coverage, Blocker/Critical, Security Hotspots |
| **Redmine** | Đọc ticket evidence, SA pipeline comments, bug history |
| **SharePoint** | Đọc TC files, verify đúng template |
| **BookStack** | Đọc spec, requirement documents |
| **STG environment** | Thực hiện smoke test Layer 3 |

---

## Relationship model

```
Dev → [SA0–SA5 + DoR evidence] → QC sweep → [QC result] → QA audit → Go/No-Go → Release
                                                                              ↑
                                                              QA có thể spot-check bất kỳ bước nào
```

**Nguyên tắc làm việc:**
- QA không blame cá nhân — focus vào process gap
- QA feedback đến Dev Lead / QC Lead (không chỉ cá nhân)
- Kết quả audit → shared với team để học, không phải để phán xét

---

## Liên kết

- [QA Audit Checklist](qa-audit-checklist.md) — Checklist cụ thể cho từng audit
- [Dev Test DoR](dev-test-dor.md) — Evidence Dev phải cung cấp cho QA audit
- [QC Classification Matrix](qc-classification-matrix.md) — Tier nào audit ở mức nào
- [QA Audit Report template](../templates/report-qa-audit.md)
