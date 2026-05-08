# QA Skills Framework — Must-have & Nice-to-have

**Owner:** QA Manager (Dung) | **Tạo:** 2026-05-08
**Áp dụng:** Đào tạo QC → QA, tuyển dụng QA Engineer
**BookStack:** https://bookstack.gotit.vn/books/pd-c-qa-team/page/ky-nang-qa-engineer-must-have-nice-to-have

---

## Nguyên tắc cốt lõi

QA và QC là hai vai trò khác nhau về tư duy:

| | QC | QA |
|---|---|---|
| Làm gì | Test, tìm bug, sweep | Audit evidence, kiểm tra quy trình |
| Đầu vào | Code + Spec | Evidence từ Dev + QC |
| Output | Bug report | Audit report + Go/No-Go |
| Phạm vi | Từng ticket | Toàn bộ release pipeline |

---

## MUST-HAVE

### Nhóm 1: Tư duy nền tảng (prerequisite — không đào tạo được nhanh)

#### Analytical Thinking
- **Mục đích:** QA đọc evidence gián tiếp và rút ra kết luận — không test trực tiếp
- **Mục tiêu:** Từ SA comments, TC list, SonarQube → tự xác định đủ/thiếu/rủi ro
- **Giải thích:** Chia nhỏ vấn đề, so với tiêu chuẩn, kết luận bằng logic — không cảm tính

#### Critical Thinking
- **Mục đích:** Bảo vệ Go/No-Go khỏi áp lực và thông tin không đáng tin
- **Mục tiêu:** Luôn đặt câu hỏi: "evidence này đủ không?", "tôi đang bị pressure không?"
- **Giải thích:** Tách biệt áp lực khỏi bằng chứng. Dev nói "đã test kỹ" → QA hỏi "evidence ở đâu?"

#### Systems Thinking
- **Mục đích:** Phát hiện vấn đề gốc rễ trong quy trình, không chỉ xử lý từng triệu chứng
- **Mục tiêu:** Nhìn pattern across tickets/sprints → xác định lỗi cá nhân hay lỗi hệ thống
- **Giải thích:** Không hỏi "bug này do ai?" mà hỏi "tại sao bug này tồn tại đến production?"

---

### Nhóm 2: Kỹ năng kỹ thuật

#### Evidence Auditing
- **Mục đích:** Công việc chính của QA — kiểm tra người khác làm đúng quy trình chưa
- **Mục tiêu:** Đọc SA0–SA5, TC list, SonarQube → kết luận evidence đủ chưa hay còn gap
- **Giải thích:** Như kiểm toán viên — không làm lại sổ sách, đọc và xác nhận đúng chuẩn

#### TC Gap Analysis
- **Mục đích:** Đảm bảo TC cover đủ spec, không miss case quan trọng
- **Mục tiêu:** Đọc spec → tự liệt kê scenarios → so TC list → tính gap rate → flag nếu >20%
- **Giải thích:** TC đúng format vẫn có thể miss 30% scenarios thực tế

#### SonarQube Literacy
- **Mục đích:** Kiểm tra code quality layer mà QC không có access
- **Mục tiêu:** Trong 5 phút: coverage đạt chưa, Blocker/Critical có không, Hotspots xử lý chưa
- **Giải thích:** Blocker = không release · Critical = cần xem xét · Hotspot Open = chưa xử lý

#### API Testing Cơ bản
- **Mục đích:** GotIt là API-driven — nhiều luồng không có UI
- **Mục tiêu:** Dùng Postman verify critical flows: response schema, status code, business logic
- **Giải thích:** Cần biết: HTTP methods, status codes (200/400/500), request/response schema

#### Security Risk Recognition
- **Mục đích:** GotIt fintech — security bug có thể gây mất tiền thật
- **Mục tiêu:** Nhận biết rủi ro khi audit ticket có thay đổi input/auth/DB/payment
- **Giải thích:** Đây là *nhận biết*, không phải *khai thác*. Cần nhận diện:
  - SQL Injection: input user đưa trực tiếp vào query DB?
  - XSS: nội dung user render mà không encode?
  - Auth bypass: endpoint mới có kiểm tra quyền không?
  - Hardcoded credentials: API key, password bị hard-code?

---

### Nhóm 3: Kỹ năng mềm

#### Independent Judgment
- **Mục đích:** Go/No-Go là quyết định độc quyền của QA — không độc lập thì không có giá trị
- **Mục tiêu:** Ra quyết định dựa trên evidence dù bị pressure từ deadline, Dev Lead, PM
- **Giải thích:** Phân biệt "tôi đang bị pressure" vs "evidence thực sự đủ rồi"

#### Process RCA
- **Mục đích:** Production bug = tín hiệu một bước pipeline đã fail → cần fix quy trình
- **Mục tiêu:** Trace ngược: SA2 identify risk chưa? TC cover chưa? QA audit check chưa?
- **Giải thích:** RCA của QA hỏi "quy trình quality miss ở đâu?" — không phải "code bị sao?"

#### Technical Report Writing
- **Mục đích:** Audit Report là output chính — mơ hồ thì Dev/PM không biết làm gì
- **Mục tiêu:** Mỗi finding có: fact cụ thể + severity + action required
- **Giải thích:**
  - ❌ "TC chưa đầy đủ, cần bổ sung thêm"
  - ✅ "TC thiếu 4 negative cases cho payment timeout — gap rate 35%. Risk: double-charge. QC bổ sung trước release."

#### Integrity Under Pressure
- **Mục đích:** Quality gate chỉ có giá trị nếu không bị bỏ qua khi bất tiện
- **Mục tiêu:** Khi bị pressure ship, không lower standard — hoặc PM phải documented risk acceptance
- **Giải thích:** Bẻ cong tiêu chuẩn một lần → tiêu chuẩn mất giá trị vĩnh viễn

---

## NICE-TO-HAVE

### Automation Testing

| Kỹ năng | Mục đích | Ghi chú |
|---|---|---|
| CI/CD Literacy | Biết automated tests chạy ở bước nào, đọc gate status | Không cần setup |
| Đọc Automation Report | Audit kết quả automation như Layer 1 evidence | Phân biệt flaky test vs real failure |
| Viết Script Cơ bản | Tự automate smoke test Layer 3 | Playwright/Robot Framework — login → flow → verify |
| Performance Testing | Đọc JMeter/K6 report: p95 latency, error rate | Không cần tự chạy |
| Visual Regression | Đọc visual diff report | Phân biệt design change vs regression |

> QA không cần build framework — đó là việc của Automation Engineer. Chỉ cần đủ để *đánh giá chất lượng* automation.

### Kỹ thuật nâng cao

| Kỹ năng | Giá trị |
|---|---|
| SQL cơ bản | Verify data integrity độc lập, không nhờ Dev |
| Đọc Code Diff | Hiểu scope thay đổi PR → audit đúng trọng tâm |
| Domain Fintech | Nhận ra high-risk business logic (idempotency, reconciliation) |
| Mentoring QC | Coaching → QC tự catch gap trước khi QA flag |

---

## Ma trận ưu tiên đào tạo

| Kỹ năng | Độ khó | Thời gian | Ưu tiên |
|---|---|---|---|
| Evidence Auditing | Cao | 4–6 tuần | **P0** |
| Independent Judgment | Rất cao | 3–6 tháng | **P0** |
| SonarQube Literacy | Thấp | 1–2 tuần | **P1** |
| Security Recognition | Trung bình | 3–4 tuần | **P1** |
| Systems Thinking | Cao | Dài hạn | **P1** |
| Technical Report Writing | Trung bình | 2–3 tuần | **P2** |
| Automation Literacy | Thấp–Trung | 2–4 tuần | **P3** |

**Top 3 kỹ năng khi tuyển:** Critical Thinking → Evidence Auditing → Integrity Under Pressure

---

## Liên kết

- [QA Role Definition](qa-role-definition.md) — QA làm gì, không làm gì
- [QA Job Description](qa-job-description.md) — Yêu cầu tuyển dụng đầy đủ
- [QA Process](qa-process.md) — Quy trình hoạt động 4 nhịp
- [QA Audit Checklist](qa-audit-checklist.md) — Checklist Layer 1/2/3
