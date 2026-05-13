# AI Workflow — Đo lường Năng suất & Hiệu quả

**Owner:** Dung Nguyễn | **Áp dụng cho:** Biệt Đội AI — Dev + QC + BA
**Cập nhật:** 2026-05-13

---

## Mục tiêu

Trả lời 3 câu hỏi thực chất:

| # | Câu hỏi | Chiều đo |
|---|---|---|
| 1 | Team có thực sự dùng AI không? | **Độ phủ sử dụng AI** |
| 2 | Dùng AI có nhanh hơn không? | **Speed** |
| 3 | Dùng AI có chất lượng hơn không? | **Quality** |

> **Nguyên tắc:** Đo outcome, không đo activity. Ưu tiên automated signal — tránh self-report vì người dùng quên hoặc điền đại.

---

## Thành viên cần làm gì?

### 👨‍💻 Developer

**Việc 1 — Tag commit message (bắt buộc, cả hai tag):**

```
[AI]    feat: generate authentication service
[HUMAN] fix: correct typo in error message
```

`[AI]` nếu AI generate code/logic chính. `[HUMAN]` nếu tự viết toàn bộ. **Thiếu tag → Push Rule reject, không push được.**

**Việc 2 — Điền 1 trường khi close issue trên Redmine:**

| Trường | Giá trị |
|---|---|
| `AI Used` | Yes / No |

---

### 📋 BA / PO

**Điền 2 trường khi close issue:**

| Trường | Giá trị |
|---|---|
| `AI Used` | Yes / No |
| `Cần clarify sau khi viết` | Yes / No |

---

### 🧪 QC

**Điền 2 trường khi close bug:**

| Trường | Giá trị |
|---|---|
| `AI Used` | Yes / No |
| `Bug Source` | Dev / Requirement / Test miss |

---

### 🔒 Security

**Điền 1 trường khi close issue:**

| Trường | Giá trị |
|---|---|
| `AI Used` | Yes / No |

---

### 🔍 QA

Không điền thêm trường. Tổng hợp Sprint AI Report cuối mỗi sprint (15–20 phút).

---

## 1. Độ phủ sử dụng AI

Mục tiêu: biết được bao nhiêu % công việc thực sự có AI tham gia.

### Cách đo — Commit Tag (tự động, Dev)

Mọi commit **bắt buộc** có tag ở đầu message. GitLab Push Rule tự reject nếu thiếu.

| Tag | Khi nào dùng |
|---|---|
| `[AI]` | AI generate code/logic chính trong commit (dù có chỉnh sửa thêm) |
| `[HUMAN]` | Tự viết toàn bộ — AI không generate logic, kể cả dùng autocomplete |

**Metric chính:**

```
AI Coverage Rate = số story có ít nhất 1 commit [AI] / tổng story × 100%
```

> Đo ở cấp **story**, không cấp commit — tránh inflate bằng nhiều commit nhỏ.

**HUMAN Rate** = phần còn lại → dùng để phát hiện ai chưa adopt AI.

### Cách đo — AI Task Coverage (Redmine, tất cả roles)

```
AI Task Coverage = issue có AI Used = Yes / tổng issue đã close × 100%
```

Bổ sung cho AI Coverage Rate — capture được BA, QC, Security dùng AI nhưng không commit code.

### Setup Push Rule trên GitLab

Project → Settings → Repository → Push Rules → Commit message:

```
\[(AI|HUMAN)\]
```

**Fallback CI** nếu không có quyền Push Rules — thêm vào `.gitlab-ci.yml`:

```yaml
check-ai-tag:
  stage: .pre
  script:
    - |
      git log origin/$CI_DEFAULT_BRANCH..HEAD --format="%s" | while read msg; do
        if ! echo "$msg" | grep -qE '\[(AI|HUMAN)\]'; then
          echo "❌ Commit thiếu tag: $msg — thêm [AI] hoặc [HUMAN]"
          exit 1
        fi
      done
  rules:
    - if: '$CI_PIPELINE_SOURCE == "merge_request_event"'
```

### Alert threshold

| Metric | Warning | Critical |
|---|---|---|
| AI Coverage Rate | < 60% | < 40% |
| AI Task Coverage | < 50% | < 30% |

---

## 2. Speed — Dùng AI có nhanh hơn không?

### Metrics

| Metric | Định nghĩa | Source |
|---|---|---|
| MR Cycle Time | `merged_at − created_at` (giờ) | GitLab API |
| Dev Cycle Time | `In Progress → Ready for QC` (ngày) | Redmine journal |
| Full Story Cycle Time | `In Progress → Done` (ngày) | Redmine journal |
| Story Throughput | Số story Done / sprint | Redmine |

> **Dev Cycle Time** đo tốc độ dev thuần túy.
> **Full Story Cycle Time** bao gồm cả thời gian QC — tách riêng để tránh metric dev bị ảnh hưởng bởi QC backlog.

### Cách lấy dữ liệu

**GitLab API — MR Cycle Time:**

```python
# GET /projects/:id/merge_requests?state=merged
cycle_time_hours = (mr["merged_at"] - mr["created_at"]).total_seconds() / 3600
```

**Redmine Journal API — Dev Cycle Time:**

```
GET /issues/{id}.json?include=journals
→ Tìm prop_key = "status_id"
→ Start: chuyển sang "In Progress"
→ End:   chuyển sang "Ready for QC"
```

### Baseline & Target

> **Bắt buộc:** Pull baseline 2 sprint gần nhất trước khi triển khai. Không có baseline = không đo được cải thiện.

| Metric | Target (sau 3 tháng) | Owner |
|---|---|---|
| MR Cycle Time | Giảm ≥ 20% vs baseline | Dev Lead |
| Dev Cycle Time | Giảm ≥ 15% vs baseline | Dev Lead |
| Story Throughput | Tăng ≥ 15% vs baseline | QC Manager |

### Alert threshold

| Metric | Warning | Critical |
|---|---|---|
| MR Cycle Time | Tăng > 30% vs baseline | Tăng > 50% vs baseline |

---

## 3. Quality — Dùng AI có chất lượng hơn không?

### Metrics

| Metric | Định nghĩa | Source | Owner |
|---|---|---|---|
| Bug Rate / Story | Số bug / số story / sprint | Redmine | QC Manager |
| Bug Escape Rate | Bug ở STG+PROD / (bug Dev tự tìm + bug STG+PROD) × 100% | Redmine | QC Manager |
| Bug Source: Dev | Bug do Dev code sai / tổng bug × 100% | Redmine — QC field | QC Manager |
| Bug Source: Requirement | Bug do spec chưa rõ / tổng bug × 100% | Redmine — QC field | QC Manager |
| Bug Source: Test Miss | Bug QC bỏ sót / tổng bug × 100% | Redmine — QC field | QC Manager |
| Code Coverage | % line/branch covered | SonarQube | Dev Lead |
| Critical Bug Count | Số bug S1/S2 / sprint | Redmine | QC Manager |

> **Bug Escape Rate:** Bug Dev tự tìm trước QC handover **không tính** vào escape. Chỉ tính bug phát hiện tại STG hoặc PROD sau khi đã handover QC.

> **Bug Source:** QC điền khi close bug — phân tích gốc rễ để biết AI đang giúp hay gây vấn đề ở giai đoạn nào (code / spec / test).

### Automated Quality Gate — SonarQube

CI job chạy SonarQube sau mỗi MR. **Fail → không merge được.**

Cấu hình Quality Gate: **coverage ≥ 90%** + **no new critical/blocker issues**

> SonarQube là single source of truth cho code quality. Không cần gate riêng cho coverage.

**Grace period:** Sprint 1–2: warn only → Sprint 3 trở đi: block merge.

### Target

| Metric | Target | So sánh với |
|---|---|---|
| Bug Rate / Story | Giảm ≥ 30% | Baseline T1–T2/2026 |
| Bug Escape Rate | Giảm ≥ 20% | Baseline T1–T2/2026 |
| Code Coverage | ≥ 90% | Enforce tự động |
| Critical Bug S1/S2 | Giảm ≥ 50% | Năm 2025 |

### Alert threshold

| Metric | Warning | Critical |
|---|---|---|
| Bug Escape Rate | > 20% | > 35% |
| Critical Bug / sprint | > 2 | > 4 |
| Bug Source: Dev | > 60% liên tục 2 sprint | — |

---

## 4. Evidence — Lưu ở đâu, cần có gì

Evidence **không điền vào MR** — được tạo ra trong lúc làm việc, lưu trong **Redmine ticket comment**.

| Bước | Evidence tối thiểu trong ticket comment |
|---|---|
| SA0 | 3–5 dòng: làm gì / thay đổi ở đâu / ảnh hưởng đến đâu |
| SA1 | Điểm đã clarify + tên BA/PO confirm + ngày |
| SA2 | Risk matrix: High / Medium / Low + lý do ngắn |
| SA3 | List test scenarios + pass/fail |
| SA4 | 2+ attack vectors phù hợp risk profile + kết quả |
| SA5 | RCA summary *(chỉ khi bug S1/S2)* |

**Trong MR description — chỉ cần 3 thứ:**

1. Ticket reference (#ID)
2. AI self-review result (1 dòng)
3. Notes cho reviewer (context, trade-off, điểm cần review kỹ)

---

## 5. Dashboard & Báo cáo

### Nguồn dữ liệu

| Source | Metrics |
|---|---|
| GitLab API | MR cycle time, AI Coverage Rate |
| Redmine API | Bug rate, throughput, Dev/Full cycle time, AI Task Coverage, Bug Source |
| SonarQube API | Code coverage, quality gate history |

Tích hợp vào Redmine Dashboard hiện có (Python + Streamlit, `localhost:8501`).

### Sprint AI Report — Template cho QA tổng hợp

```
Sprint AI Report — [Tên Sprint]  |  [dd/mm/yyyy]
Người tổng hợp: [name]

⚡ TỐC ĐỘ
  Dev Cycle Time (avg):         __ ngày   (sprint trước: __ ngày)
  MR Cycle Time (avg):          __h       (sprint trước: __h)
  Story Throughput:             __        (sprint trước: __)

🎯 CHẤT LƯỢNG
  Bug Rate/Story:               __        (sprint trước: __)
  Bug Escape Rate:              __%       (target ≤ 20%)
  Bug Source — Dev:             __%       (cờ đỏ nếu > 60%)
  Bug Source — Requirement:     __%
  Bug Source — Test miss:       __%
  Critical Bug S1/S2:           __        (target = 0)
  Code Coverage:                __%       (SonarQube)

📊 ĐỘ PHỦ AI
  AI Coverage Rate (story):     __%       (sprint trước: _%)
  AI Task Coverage (all roles): __%       (sprint trước: _%)

NHẬN XÉT
  AI đang giúp ích rõ nhất ở:
  Chưa thấy cải thiện ở:
  Action sprint tới:
```

> **Đọc kết quả:** Không cần tính điểm. Nhìn vào xu hướng **3 sprint liên tiếp** của từng dimension.

### Cờ đỏ — cần thảo luận ngay trong Retro

- **Bug Escape Rate tăng** trong khi AI Coverage tăng → AI đang ảnh hưởng tiêu cực đến chất lượng
- **Bug Source: Dev > 60%** liên tục 2 sprint → cần review lại cách dev dùng AI để generate code
- **AI Coverage Rate không tăng** sau sprint 3 → rào cản adoption cần được xử lý
- **Dev Cycle Time tăng** nhưng Story Throughput giảm → team đang bị overhead bởi quy trình mới

### Format báo cáo bi-weekly lên leadership

```
Sprint N (DD/MM – DD/MM)

ĐỘ PHỦ AI
├── AI Coverage Rate:    X% story có [AI] commit  (↑/↓ vs sprint trước)
└── AI Task Coverage:    X% task toàn team         (↑/↓ vs sprint trước)

SPEED
├── MR Cycle Time:       X giờ  (↑/↓ Y% vs baseline)
├── Dev Cycle Time:      X ngày (↑/↓ Y% vs baseline)
└── Story Throughput:    X story (↑/↓ Y% vs baseline)

QUALITY
├── Bug Rate/Story:      X bug/story  (↑/↓ Y%)
├── Bug Escape Rate:     X%           (↑/↓ Y%)
├── Critical Bug S1/S2:  X bugs
├── Code Coverage:       X%           (SonarQube)
└── SonarQube Gate:      Pass / Warn / Fail

ACTION ITEMS
└── [Các điểm cần xử lý dựa trên alert threshold]
```

---

## 6. Triển khai

### Tuần 1 — Chuẩn bị (trước khi setup bất cứ thứ gì)

- [ ] Pull baseline data 2 sprint gần nhất (GitLab + Redmine + SonarQube)
- [ ] Align với Dev Leads: commit convention, evidence format, MR template
- [ ] Demo quy trình cho team

### Tuần 2 — Setup automated gates

- [ ] GitLab Push Rule: `\[(AI|HUMAN)\]`
- [ ] SonarQube Quality Gate: coverage ≥ 90%, **warn only** (chưa block)
- [ ] GitLab MR Templates: `default.md` (feature) + `bugfix.md`
- [ ] Thêm `AI Used` và `Bug Source` custom fields vào Redmine

### Sprint 1–2 — Giai đoạn làm quen

- [ ] SonarQube warn only, chưa block merge
- [ ] Sampling audit lần đầu (xem mục QA)
- [ ] Thu thập feedback, điều chỉnh nếu cần

### Sprint 3 trở đi — Full enforcement

- [ ] SonarQube chuyển sang block merge
- [ ] First bi-weekly report với trend vs baseline

### Tháng 3 — Review KPI

- [ ] AI Coverage Rate ≥ 80%
- [ ] Speed: MR cycle time ↓ 20%, throughput ↑ 15%
- [ ] Quality: Bug rate ↓ 30%, escape rate ↓ 20%
- [ ] Điều chỉnh threshold nếu cần

---

## Tài liệu liên quan

- **Dev AI Testing Playbook** — SA0–SA5, commit convention, push rule setup
- **Dev Test DoR** — Definition of Ready trước khi handover QC
- **MR Template Feature** — `.gitlab/merge_request_templates/default.md`
- **MR Template Bug Fix** — `.gitlab/merge_request_templates/bugfix.md`
