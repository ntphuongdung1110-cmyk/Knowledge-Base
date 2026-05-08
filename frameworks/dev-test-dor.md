# Dev Test DoR — Definition of Ready for QC Handover

**Owner:** Dung Nguyễn | **Tạo:** 2026-05-04
**Áp dụng cho:** Tất cả Dev trong 3 Scrum Teams
**Review với Dev Leads:** Cần align trước 12/05/2026

---

## Mục đích

DoR này định nghĩa **Dev phải làm gì trước khi handover ticket cho QC**. Nếu không đạt → QC từ chối nhận, Dev fix và re-submit. Không có exception.

> Nguồn: [qc-scope-v2.md](~/second-brain/projects/qc-transformation/qc-scope-v2.md)

> **Lưu ý cho Dev:** QA sẽ audit ngẫu nhiên evidence trong DoR này. Evidence phải đủ để QA kiểm tra độc lập — không cần hỏi thêm Dev. Thiếu evidence = fail audit, dù ticket đã được QC chấp nhận.

---

## 3 Điều kiện bắt buộc

### Điều kiện 1 — SA Pipeline

Dev bắt buộc chạy **SA0–SA5** và đính kèm evidence:

| Step | Subagent | Output cần attach |
|---|---|---|
| SA0 | Context Setup | Comment trong ticket: làm gì / thay đổi ở đâu / ảnh hưởng đến đâu (3–5 dòng) |
| SA1 | Requirement Clarifier | Screenshot kết quả + câu hỏi đã clarify với BA/PO |
| SA2 | Risk Scout | Risk matrix đã review — đánh dấu High/Medium/Low |
| SA3 | Test Designer | Danh sách test scenarios đã thực hiện |
| SA4 | Bug Hunter | Các attack vectors đã thử — kết quả pass/fail |
| SA5 | RCA Assistant | Chỉ bắt buộc khi phát hiện bug severity 1–2 trong quá trình dev |

**Evidence format:** Comment trong Redmine ticket hoặc attachment screenshot

---

### Điều kiện 2 — Unit Test Coverage

| Yêu cầu | Tool kiểm tra | Pass threshold |
|---|---|---|
| Unit test coverage | SonarQube | **≥90%** cho code mới/changed |
| Không có Blocker issue | SonarQube | **0 Blocker** |
| Không có Critical issue | SonarQube | **0 Critical** |

**Evidence:** Link SonarQube scan result, hoặc screenshot dashboard

---

### Điều kiện 3 — Self-QA Checklist

Dev tự kiểm tra trước khi tạo PR/handover:

#### A. Functional Check

- [ ] Đã test happy path toàn bộ theo acceptance criteria
- [ ] Đã test ít nhất 3 negative scenarios (input sai, thiếu, boundary)
- [ ] Đã test trên môi trường STG (không chỉ local)
- [ ] Không có bug Severity 1 hoặc 2 còn open
- [ ] Nếu có bug Severity 3–4: đã log vào Redmine, chấp nhận risk

#### B. Technical Check

- [ ] Code review đã được approve bởi ít nhất 1 Dev khác
- [ ] Không có console.log / debug statement còn trong code
- [ ] Không có hardcoded credentials hoặc sensitive data
- [ ] Migration (nếu có) đã chạy thành công trên STG DB

#### C. Integration Check

- [ ] Đã test các luồng liên quan đến module khác (nếu có impact)
- [ ] API contract không thay đổi breaking (hoặc đã thông báo bên liên quan)
- [ ] Đã check ảnh hưởng đến luồng đang live (regression quick check)

#### D. Documentation

- [ ] Acceptance criteria trong ticket được đánh dấu pass/fail rõ ràng
- [ ] Nếu behavior thay đổi: đã update comment/doc trong ticket
- [ ] Đã estimate thời gian QC cần (để QC plan sprint)

---

## Quy trình khi DoR không đạt

```
Dev tạo PR / request handover
        │
        ▼
QC kiểm tra 3 điều kiện (≤30 phút)
        │
   ┌────┴────┐
 Pass       Fail
   │           │
   ▼           ▼
QC nhận    QC comment rõ lý do fail
           + tag Dev + PM trong ticket
           Dev fix → Re-submit
```

**SLA:**
- QC verify DoR: trong **1 ngày làm việc** sau khi Dev request
- Dev re-submit sau fix: trong **1 ngày làm việc**
- Nếu fail lần 2: escalate lên Dev Lead + Dung

---

## Bug severity mà Dev được phép handover

| Severity | Được handover | Điều kiện |
|---|---|---|
| S1 — Critical (system down, data loss) | ❌ Không | Phải fix trước |
| S2 — Major (luồng chính broken) | ❌ Không | Phải fix trước |
| S3 — Minor (luồng phụ bị ảnh hưởng) | ✅ Có | Đã log Redmine, Dev ack risk |
| S4 — Trivial (UI/typo) | ✅ Có | Đã log Redmine |

---

## Metrics theo dõi DoR compliance

Tracked trên Redmine Dashboard (Tab 2 — Monthly Report):

| Metric | Mục đích | Target |
|---|---|---|
| % tickets pass DoR lần đầu | Đo dev self-quality | ≥80% (từ Q3/2026) |
| Average re-submit count | Đo mức độ gap | <1.5 lần |
| % SA pipeline evidence attached | Đo AI adoption | ≥70% (từ Q3/2026) |

---

## Liên kết

- [Quality Standards](qa-quality-standards.md) — Tiêu chuẩn đầy đủ: severity, coverage, TC format
- [QC Classification Matrix](qc-classification-matrix.md) — Tier nào cần sweep sau khi DoR pass
- [Dev AI Testing Playbook](dev-ai-testing-playbook.md) — Hướng dẫn SA0–SA5 từng bước
- [QC Sweep Checklist](qc-sweep-checklist.md) — QC làm gì sau khi nhận ticket
