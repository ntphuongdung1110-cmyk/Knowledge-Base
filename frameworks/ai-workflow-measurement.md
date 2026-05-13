# AI Workflow — Đo lường Năng suất & Hiệu quả

**Owner:** Dung Nguyễn | **Áp dụng cho:** Biệt Đội AI — Dev + QC + BA
**Cập nhật:** 2026-05-13

---

## Mục tiêu

| Câu hỏi | Chiều đo |
|---|---|
| Team có thực sự dùng AI không? | Độ phủ sử dụng AI |
| Dùng AI có nhanh hơn không? | Speed |
| Dùng AI có tốt hơn không? | Quality |

> Nguyên tắc: **Đo outcome, không đo activity.** Tránh self-report — người dùng quên, điền đại, không chính xác.

---

## 1. Độ phủ sử dụng AI

### Cách đo (tự động, không cần self-report)

**Commit Tag — Bắt buộc mọi commit:**

| Tag | Ý nghĩa |
|---|---|
| `[AI]` | AI generate code/logic chính trong commit (dù có chỉnh sửa thêm) |
| `[HUMAN]` | Tự viết toàn bộ, AI không generate logic — kể cả dùng autocomplete |

Commit không có tag → GitLab Push Rule tự động reject, không push được.

**Setup Push Rule (Admin GitLab):**

```
Regex: \[(AI|HUMAN)\]
```

Vào: Project → Settings → Repository → Push Rules → Commit message

**Fallback CI** (nếu không có quyền Push Rules):

```yaml
check-ai-tag:
  stage: .pre
  script:
    - |
      git log origin/$CI_DEFAULT_BRANCH..HEAD --format="%s" | while read msg; do
        if ! echo "$msg" | grep -qE '\[(AI|HUMAN)\]'; then
          echo "❌ Commit thiếu tag: $msg"
          exit 1
        fi
      done
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

**Metric chính:**

```
AI Coverage Rate = số story có ≥1 commit [AI] / tổng story hoàn thành × 100%
```

Đo ở story level — tránh gameable khi dev tạo nhiều commit [AI] nhỏ trên story không dùng AI thực sự.

**Alert:**
- Warning: AI Coverage Rate < 60% / sprint
- Critical: AI Coverage Rate < 40% / sprint

---

## 2. Speed — Dùng AI có nhanh hơn không?

### Metrics & nguồn dữ liệu

| Metric | Công thức | Source | Owner |
|---|---|---|---|
| Dev Cycle Time | `MR merged_at − MR created_at` (giờ) | GitLab API | Dev Lead |
| Full Story Cycle Time | Từ `In Progress → Done` (ngày) | Redmine Journal API | QC Manager |
| Story Throughput | Số story Done / sprint | Redmine | Scrum Master |

> Tách Dev Cycle Time và Full Story Cycle Time để tránh QC backlog ảnh hưởng đến dev metrics.

### Cách lấy dữ liệu

**GitLab API — Dev Cycle Time:**
```python
# GET /projects/:id/merge_requests?state=merged
cycle_time_hours = (mr["merged_at"] - mr["created_at"]).total_seconds() / 3600
```

**Redmine — Full Story Cycle Time:**
```
GET /issues/{id}.json?include=journals
# Tìm journal có prop_key = "status_id", value = closed_id
# Lấy created_on của journal đó
```

### Baseline & target

| Metric | Baseline | Target sau 3 tháng |
|---|---|---|
| Dev Cycle Time | Đo 2 sprint trước khi apply AI | Giảm ≥20% |
| Full Story Cycle Time | Đo 2 sprint trước khi apply AI | Giảm ≥15% |
| Story Throughput | Đo sprint hiện tại | Tăng ≥15% |

> **Bắt buộc:** Pull baseline data 2 sprint gần nhất TRƯỚC khi apply AI workflow. Không có baseline = không đo được improvement.

**Alert:**
- Warning: Story Throughput giảm >10% so baseline 2 sprint liên tiếp

---

## 3. Quality — Dùng AI có tốt hơn không?

### Metrics & nguồn dữ liệu

| Metric | Công thức | Source |
|---|---|---|
| Bug Rate / Story | Số bug / số story sprint | Redmine |
| Bug Escape Rate | Bug tại STG+PROD / (Dev self-found + STG+PROD bugs) × 100% | Redmine |
| Code Coverage | % line/branch covered | SonarQube |
| Critical Bug Count | Số bug S1/S2 / sprint | Redmine |

> **Bug Escape Rate:** Chỉ tính bug phát hiện SAU khi handover QC. Bug dev tự tìm trong quá trình làm không tính là escaped.

### SonarQube Quality Gate

Gate duy nhất cho code quality — đã bao gồm coverage threshold + no new critical issues.

| Sprint | Mode |
|---|---|
| Sprint 1–2 | Warn only (không block merge — legacy codebase cần grace period) |
| Sprint 3+ | Block merge nếu fail |

Cấu hình: coverage ≥90%, no new critical/blocker issues.

### Target

| Metric | Target |
|---|---|
| Bug Rate / Story | Giảm ≥30% so baseline |
| Bug Escape Rate | ≤20% (Warning >20%, Critical >35%) |
| Code Coverage | ≥90% (enforce qua SonarQube Gate) |
| Critical Bug (S1/S2) | Giảm ≥50% so 2025 |

---

## 4. Evidence — Lưu ở đâu, gì cần có

Evidence **KHÔNG** điền vào MR — evidence tạo ra trong lúc làm việc, lưu trong **Redmine ticket comment**.

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
- Notes cho reviewer (context, trade-off, steps to reproduce nếu là bug fix)

---

## 5. Dashboard & Báo cáo

### Nguồn dữ liệu → Dashboard

```
GitLab API   → Dev Cycle Time, AI Coverage Rate (story level)
Redmine API  → Bug Rate, Story Throughput, Full Story Cycle Time
SonarQube    → Code Coverage, Quality Gate history
```

Dùng Redmine Dashboard hiện có (Python + Streamlit tại `localhost:8501`) — thêm GitLab API và SonarQube API vào cùng.

### Báo cáo bi-weekly lên leadership

```
Sprint N (DD/MM – DD/MM):

Độ phủ AI
└── AI Coverage Rate: X% story (↑/↓ Y% vs sprint trước)

Speed
├── Dev Cycle Time:       X giờ  (↑/↓ Y% vs baseline)
├── Full Story Cycle Time: X ngày (↑/↓ Y% vs baseline)
└── Story Throughput:     X story (↑/↓ Y% vs baseline)

Quality
├── Bug Rate/Story:    X bug/story (↑/↓ Y%)
├── Bug Escape Rate:   X%         (↑/↓ Y%)
├── Code Coverage:     X%         (SonarQube)
└── Critical Bug S1/S2: X bug

Automated Gates
├── Push Rule:         X commit blocked (thiếu tag)
└── SonarQube Gate:    Pass / Warn / Block
```

---

## 6. Triển khai

### Tuần 1 — Align & Baseline (trước khi bật gate)
- [ ] Align với Dev Leads về commit convention và evidence format
- [ ] Pull baseline data 2 sprint gần nhất (Dev Cycle Time, Bug Rate, Story Throughput)
- [ ] Demo workflow cho team — giải thích tại sao từng gate cần thiết

### Tuần 2 — Setup Gates (1 lần, chạy mãi)
- [ ] GitLab Push Rule: regex `\[(AI|HUMAN)\]`
- [ ] SonarQube Quality Gate: bật threshold coverage ≥90%, block on new critical
- [ ] GitLab MR Templates: `default.md` (feature) + `bugfix.md`
- [ ] Kết nối GitLab API và SonarQube API vào Dashboard

### Sprint 1–2 — Grace Period
- [ ] SonarQube: warn only, không block merge
- [ ] Theo dõi AI Coverage Rate — chưa enforce target
- [ ] Thu thập feedback từ dev về friction

### Sprint 3 — Full Enforcement
- [ ] SonarQube: chuyển sang block merge nếu fail
- [ ] Bắt đầu so sánh metrics vs baseline

### Tháng 3 — Review KPI
- [ ] Speed: Dev Cycle Time giảm ≥20%, Throughput tăng ≥15%
- [ ] Quality: Bug Rate giảm ≥30%, Bug Escape Rate ≤20%
- [ ] Độ phủ AI: Coverage Rate ≥80%
- [ ] Điều chỉnh SonarQube threshold nếu cần

---

## Liên kết

- [Dev AI Testing Playbook](dev-ai-testing-playbook.md) — SA0–SA5 chi tiết + GitLab commit convention
- [Dev Test DoR](dev-test-dor.md) — Definition of Ready trước khi handover QC
- [MR Template Feature](../templates/gitlab-mr-template.md) — Copy vào GitLab
- [MR Template Bug Fix](../templates/gitlab-mr-template-bugfix.md) — Copy vào GitLab
