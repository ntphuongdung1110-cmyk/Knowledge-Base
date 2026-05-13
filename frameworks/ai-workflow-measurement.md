# AI Workflow — Đo lường Năng suất & Hiệu quả

**Owner:** Dung Nguyễn | **Áp dụng cho:** Biệt Đội AI — Dev + QC + BA
**Cập nhật:** 2026-05-13

---

## Mục tiêu

Đo 3 thứ:
1. **Team có thực sự dùng AI không?** (Compliance)
2. **Dùng AI có nhanh hơn không?** (Speed)
3. **Dùng AI có tốt hơn không?** (Quality)

> Nguyên tắc: **Đo outcome, không đo activity.** Tránh self-report — người dùng quên, điền đại, không chính xác.

---

## 1. Compliance — Team có dùng AI đúng quy trình không?

### Cách đo (tự động, không cần self-report)

**Gate 1 — Commit Tag:**
Mọi commit phải có tag ở đầu message:

| Tag | Ý nghĩa |
|---|---|
| `[AI]` | Toàn bộ code viết bằng AI |
| `[HUMAN]` | Viết tay hoàn toàn |
| `[MIX]` | Kết hợp AI + chỉnh sửa tay |

Commit không có tag → GitLab Push Rule tự động reject, không push được.

**Metric từ commit tag:**
```
AI Adoption Rate = số commit [AI] / tổng commit × 100%
```
Pull từ GitLab API theo sprint/tháng → báo cáo trend.

**Gate 2 — SonarQube Quality Gate:**
CI job chạy SonarQube sau mỗi MR. Fail → không merge được.
SonarQube Quality Gate bao gồm: coverage ≥90% + no new critical/blocker issues.

**MR Checklist — Dev tự xác nhận khi tạo MR (< 3 phút):**

- [ ] SA pipeline evidence đã ghi trong ticket comment
- [ ] Self-review code với AI trước khi tạo MR
- [ ] Các luồng liên quan đã test (không chỉ happy path)
- [ ] Không có breaking change chưa thông báo
- [ ] Commit tags đúng `[AI]` / `[HUMAN]` / `[MIX]`

Xem template đầy đủ: [MR Template Feature](../templates/gitlab-mr-template.md) | [MR Template Bug Fix](../templates/gitlab-mr-template-bugfix.md)

**Metric từ MR Checklist:**
```
Checklist Completion Rate = số MR checklist đầy đủ / tổng MR × 100%
```
Verify qua sampling audit — không track tự động từng item.

**Sampling Audit — Lớp kiểm tra thực chất:**
Mỗi sprint, QC Manager chọn ngẫu nhiên **3 MR** và hỏi dev trong sprint retro:
> *"Walk me through MR này — SA1 clarify gì, SA3 tìm ra risk nào?"*

15 phút/sprint. Dev biết có thể bị hỏi → làm thật.

---

## 2. Speed — Dùng AI có nhanh hơn không?

### Metrics & nguồn dữ liệu

| Metric | Công thức | Source |
|---|---|---|
| MR Cycle Time | `merged_at − created_at` (giờ) | GitLab API |
| Story Throughput | Số story Done / sprint | Redmine |
| Story Cycle Time | Từ `In Progress → Done` (ngày) | Redmine journal API |

### Cách lấy dữ liệu

**GitLab API — MR Cycle Time:**
```python
# GET /projects/:id/merge_requests?state=merged
cycle_time = mr["merged_at"] - mr["created_at"]
```

**Redmine — Story Cycle Time:**
```
GET /issues/{id}.json?include=journals
# Tìm journal có prop_key = "status_id", value = closed_id
# Lấy created_on của journal đó
```

### Baseline & target

| Metric | Baseline (T1–T2/2026) | Target sau 3 tháng AI |
|---|---|---|
| MR Cycle Time | Đo trước khi apply AI | Giảm ≥20% |
| Story Throughput | Đo sprint hiện tại | Tăng ≥15% |

> **Bước đầu tiên:** Pull baseline data 2 sprint gần nhất trước khi apply AI workflow. Không có baseline = không đo được improvement.

---

## 3. Quality — Dùng AI có tốt hơn không?

### Metrics & nguồn dữ liệu

| Metric | Công thức | Source |
|---|---|---|
| Bug Rate / Story | Số bug / số story sprint | Redmine |
| Bug Escape Rate | Bug tìm thấy sau QA / tổng bug | Redmine |
| Code Coverage | % line/branch covered | SonarQube |
| Critical Bug Count | Số bug S1/S2 / sprint | Redmine |

### Target

| Metric | Target |
|---|---|
| Bug Rate / Story | Giảm ≥30% so baseline |
| Bug Escape Rate | Giảm ≥20% so baseline |
| Code Coverage | ≥90% (enforce qua SonarQube Gate) |
| Critical Bug (S1/S2) | Giảm ≥50% so 2025 |

---

## 4. Evidence — Lưu ở đâu, gì cần có

Evidence KHÔNG điền vào MR — evidence tạo ra trong lúc làm việc, lưu trong **Redmine ticket comment**.

| Bước SA | Evidence tối thiểu trong ticket comment |
|---|---|
| SA0 | 3–5 dòng: làm gì / thay đổi ở đâu / ảnh hưởng đến đâu |
| SA1 | List điểm đã clarify + tên BA/PO confirm + ngày |
| SA2 | Risk matrix: High / Medium / Low kèm lý do |
| SA3 | List test scenarios + pass/fail |
| SA4 | 3+ attack vectors đã thử + kết quả |
| SA5 | RCA summary _(chỉ khi có bug S1/S2)_ |

**Trong MR description — chỉ cần:**
- Ticket reference (#ID)
- AI self-review result (1 dòng)
- Notes cho reviewer (context, trade-off)

---

## 5. MR Process — 3-Layer Enforcement

```
Layer 1 — AUTOMATED (hệ thống block, không bypass được)
├── Commit tag [AI/HUMAN/MIX] → Push Rule block push
└── SonarQube Quality Gate   → CI block merge

Layer 2 — MR CHECKLIST (dev tự xác nhận, < 3 phút)
├── SA pipeline evidence đã ghi trong ticket
├── Self-review code với AI trước khi tạo MR
├── Luồng liên quan đã test (không chỉ happy path)
├── Không có breaking change chưa thông báo
└── Commit tags đúng

Layer 3 — SAMPLING AUDIT (QC Manager, mỗi sprint)
└── 3 MR ngẫu nhiên → walk-through với dev → 15 phút
```

---

## 6. Dashboard & Báo cáo

### Nguồn dữ liệu → Dashboard

```
GitLab API   → MR cycle time, AI adoption rate (commit tag %)
Redmine API  → Bug rate, story throughput, cycle time
SonarQube    → Code coverage, quality gate history
```

Dùng Redmine Dashboard hiện có (Python + Streamlit tại `localhost:8501`) — thêm GitLab API và SonarQube API vào cùng.

### Báo cáo bi-weekly lên leadership

```
Sprint N (DD/MM – DD/MM):

Speed
├── MR Cycle Time:    X giờ  (↑/↓ Y% vs baseline)
└── Story Throughput: X story (↑/↓ Y% vs baseline)

Quality
├── Bug Rate/Story:   X bug/story (↑/↓ Y%)
├── Bug Escape Rate:  X% (↑/↓ Y%)
└── Code Coverage:    X% (SonarQube)

Compliance
├── AI Adoption Rate: X% commit có [AI] tag
├── SonarQube Gate:   Pass/Fail
└── Sampling Audit:   X/3 MR đạt chuẩn — issues: [...]
```

---

## 7. Triển khai — Thứ tự ưu tiên

### Tuần 1 (setup 1 lần, chạy mãi)
- [ ] GitLab Push Rule: regex `\[(AI|HUMAN|MIX)\]`
- [ ] SonarQube Quality Gate: bật threshold coverage ≥90%, block on new critical
- [ ] GitLab MR Templates: `default.md` (feature) + `bugfix.md`
- [ ] Pull baseline data 2 sprint gần nhất

### Tuần 2
- [ ] Align với Dev Leads về commit convention và evidence format
- [ ] Chạy sprint đầu tiên với full workflow

### Tháng 1
- [ ] Có đủ data để so sánh baseline vs AI workflow
- [ ] First bi-weekly report với trend data

### Tháng 3
- [ ] Review KPI: speed ↓20%, quality ↓30% bug rate, compliance ≥80%
- [ ] Điều chỉnh SonarQube threshold nếu cần

---

## Liên kết

- [Dev AI Testing Playbook](dev-ai-testing-playbook.md) — SA0–SA5 chi tiết + GitLab commit convention
- [Dev Test DoR](dev-test-dor.md) — Definition of Ready trước khi handover QC
- [MR Template Feature](../templates/gitlab-mr-template.md) — Copy vào GitLab
- [MR Template Bug Fix](../templates/gitlab-mr-template-bugfix.md) — Copy vào GitLab
