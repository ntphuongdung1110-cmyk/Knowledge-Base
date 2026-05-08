# Job Description — QA Engineer (Quality Assurance)

**Tạo:** 2026-05-08 | **Owner:** QA Manager / HR
**Phòng ban:** Product Development — Quality
**Báo cáo:** QA Manager

---

## ⚠️ Đọc kỹ trước khi apply

**Vị trí này KHÔNG phải Senior QC.** Nếu bạn muốn một công việc tập trung vào viết test case và tìm bug, đây không phải vị trí phù hợp.

**QA Engineer tại GotIt** là người:
- Kiểm tra **quy trình và evidence**, không phải test toàn bộ sản phẩm
- Ra quyết định **Go/No-Go** trước mỗi release — chịu trách nhiệm trực tiếp
- Định nghĩa **tiêu chuẩn chất lượng** cho cả tổ chức
- Làm việc **độc lập, không bị ảnh hưởng** bởi pressure ship nhanh

---

## Mục tiêu vị trí

QA Engineer là **process guardian** và **release gate** của GotIt PD.

Mục tiêu: Đảm bảo mọi release lên production đã đạt chuẩn chất lượng — không phải bằng cách test lại toàn bộ, mà bằng cách kiểm tra xem Dev và QC đã làm đúng quy trình chưa, và tự thực hiện smoke test độc lập trước khi ra quyết định.

---

## Trách nhiệm chính

### 1. Xây dựng & duy trì chuẩn chất lượng
- Định nghĩa và cập nhật Quality Standards: bug severity, code coverage threshold, TC format, Definition of Done
- Xây dựng và duy trì: DoR checklist, test templates, QA audit checklist
- Review và update frameworks khi audit phát hiện gap

### 2. Audit độc lập — 3 Layer
Không phải kiểm tra từng feature, mà kiểm tra **evidence và process**:

**Layer 1 — Dev evidence:**
- SA0–SA5 evidence có đủ và đúng format không
- SonarQube: coverage ≥90%, 0 Blocker, 0 Critical, security hotspots reviewed
- Requirement coverage: Dev hiểu đúng spec của PO chưa
- TC của Dev có cover hết risk matrix không

**Layer 2 — QC work:**
- TC format đúng template chuẩn không
- Pass/Fail ghi rõ từng TC không
- TC set cover hết acceptance criteria trong spec không
- QA tự scan spec → liệt kê case còn thiếu

**Layer 3 — Smoke test (QA tự thực hiện):**
- Critical flows trên STG trước khi deploy
- Không phải full regression — chỉ những gì liên quan đến release

### 3. Go/No-Go — Release Gate
- **Quyết định độc quyền** trước mỗi Tier 1 release
- Không bị override bởi PM hay Dev Lead (chỉ CTO/CPO với documented exception)
- Chịu trách nhiệm với quyết định của mình — track xem decision có đúng không

### 4. Spot-check ngẫu nhiên
- Tier 1: 30–50% stories/sprint (không báo trước)
- Tier 2: 15–20% stories/sprint (không báo trước)
- Phát hiện pattern → đưa vào Weekly Quality Report

### 5. Phân tích & cải tiến quy trình
- Khi có production bug: thực hiện Process RCA (bug lẽ ra bị catch ở bước nào)
- Tracking pattern lặp lại → đề xuất update DoR / checklist / playbook
- Quarterly process health review → báo cáo lên QA Manager

### 6. Báo cáo định kỳ
- **QA Audit Report** — sau mỗi Tier 1 release audit
- **Weekly Quality Report** — thứ Sáu hàng tuần (spot-check results, patterns)
- **Quarterly QA Report** — metrics trends, process improvements

---

## Yêu cầu bắt buộc

### Kinh nghiệm
- ≥3 năm kinh nghiệm trong QA/QC/Software Testing
- Đã từng **define** test process hoặc quality standards (không chỉ follow)
- Có kinh nghiệm với **risk-based testing** và coverage analysis
- Có kinh nghiệm đọc và review TC của người khác

### Kỹ năng kỹ thuật
- Đọc hiểu **SonarQube** report: coverage, Blocker/Critical, Security Hotspots
- Hiểu **API testing** cơ bản: request/response, status codes, schema validation (Postman hoặc tương đương)
- Hiểu **security basics**: input validation, SQL injection, XSS, auth bypass (không cần exploit, cần nhận biết risk)
- Đọc hiểu **test case**: biết TC nào good, TC nào thiếu coverage
- Làm việc với **bug tracking system** (Redmine, Jira, hoặc tương đương)

### Kỹ năng mềm
- **Judgment độc lập:** Có thể ra quyết định Go/No-Go dưới áp lực ship — và defend quyết định đó
- **Communication rõ ràng:** Viết audit report cụ thể, không mơ hồ; feedback gap mà không blame cá nhân
- **Systems thinking:** Nhìn pattern, không chỉ nhìn từng case đơn lẻ
- **Integrity:** Không bị ảnh hưởng bởi pressure timeline khi evidence chưa đủ

### Ngôn ngữ
- **Tiếng Việt:** Giao tiếp và viết báo cáo nội bộ
- **Tiếng Anh:** Đọc technical documentation, tools, SonarQube reports

---

## Yêu cầu ưu tiên (nice-to-have)

- Có kinh nghiệm trong **e-commerce, fintech, hoặc payment platform**
- Hiểu CI/CD pipeline cơ bản (không cần setup, cần biết nó hoạt động thế nào)
- Có kinh nghiệm với **audit** hoặc **process review** (ISO, CMMI, internal audit)
- Đã từng **mentoring hoặc coaching** QC team về process
- Biết sử dụng **SQL cơ bản** để query database verify data integrity

---

## QA KHÔNG làm gì — Phải đọc

Đây là ranh giới cực kỳ quan trọng. QA Engineer tại GotIt **không** chịu trách nhiệm:

| QA KHÔNG làm | Ai làm thay |
|---|---|
| Test lại toàn bộ sản phẩm | QC Engineer |
| Viết test case cho Dev hoặc QC | Dev (SA pipeline) + QC |
| Review từng ticket trong sprint | QC Engineer |
| Manage QC team hàng ngày | QC Manager |
| Chịu trách nhiệm fix bug production | Dev team |
| Thực hiện full regression test | QC Engineer (automation) |
| Là bottleneck cho từng ticket nhỏ | QA chỉ gate ở release level |

---

## KPIs — Đo lường thành công

| Metric | Target Q3/2026 | Ý nghĩa |
|---|---|---|
| DoR compliance rate | ≥80% tickets pass lần đầu | QA audit có đang giúp Dev làm đúng không |
| TC coverage gap rate | <20% spec cases thiếu TC | QA audit có hiệu quả không |
| SonarQube compliance | 100% tickets đạt threshold tại release | Code quality gate có hoạt động không |
| Audit cycle time (Tier 1) | ≤4 giờ/release | QA có đang efficient không |
| Bug escaped to production | Trending down | QA gate có thực sự catch được vấn đề không |
| False NO-GO (ship sau khi QA unblock) | Trending down | QA judgment có accurate không |

---

## Môi trường làm việc

**Team:** GotIt Product Development — 3 Scrum Teams
- **Voucher Service** — Core revenue, B2B API, Biz Order, Fraud
- **User Solutions** — GotIt App, Voucher Link, GI Mini App, Home Delivery
- **Client Solutions** — Campaign, Loyalty, Scanit AI

**QC team:** ~8–10 QC Engineers (QA sẽ audit work của team này)

**Products:** E-gift/e-voucher platform, fintech domain. Financial transactions là core — bug có thể có hậu quả tiền thật.

**Working style:**
- QA hoạt động **độc lập** — không embedded trong sprint team
- Audit **asynchronous** — không block daily dev workflow
- **Remote-friendly** cho audit work; cần có mặt khi smoke test release

---

## Tools

| Tool | Mục đích |
|---|---|
| **SonarQube** | Kiểm tra code coverage, Blocker/Critical, Security Hotspots |
| **Redmine** | Đọc ticket evidence, SA pipeline, bug history |
| **SharePoint** | Đọc TC files, verify template compliance |
| **BookStack** | Đọc spec, requirements |
| **STG environment** | Smoke test Layer 3 |
| **Postman** | API spot-check khi cần |

---

## Tại sao vị trí này thú vị

1. **Quyền lực thực sự** — Bạn có quyền block release. Không ai có thể override bạn ngoại trừ C-level. Quyền này đi kèm trách nhiệm thực sự.

2. **Impact rõ ràng** — Khi process gap rate giảm từ 30% xuống 10%, đó là kết quả của bạn. Đo được, visible.

3. **Học rộng** — Bạn phải hiểu Dev pipeline (SonarQube, SA, security), QC process (TC design, risk-based), và business domain (voucher, loyalty, B2B API). Không bị stuck ở một góc hẹp.

4. **Đúng thời điểm** — GotIt đang build AI-assisted testing, shift-left, automation. QA là người thiết kế quy trình cho giai đoạn chuyển đổi này.

---

## Liên kết nội bộ — Tài liệu liên quan

- [QA Role Definition](qa-role-definition.md)
- [QA Process](qa-process.md)
- [QA Quality Standards](qa-quality-standards.md)
- [QA Audit Checklist](qa-audit-checklist.md)
