# Template — Post-Sweep Findings Report

> Gửi: Dev Lead + PM của product | Gửi ngay sau khi sweep xong (same day) | Owner: QC thực hiện sweep

---

## Post-Sweep Report — [Tên sản phẩm] | [DD/MM/YYYY]

**QC thực hiện:** Thắng Trương / Khải Lâm
**Tier:** Critical / Standard
**Trigger:** Routine release / Override (lý do: ___)

---

### 1. Scope sweep

| Hạng mục | Chi tiết |
|---|---|
| Release version / build | |
| Luồng swept | |
| Test cases thực hiện | |
| Môi trường | STG / Production |
| Thời gian sweep | _giờ_ |

---

### 2. Kết quả — Bug Summary

| Severity | Số lượng | Status |
|---|---|---|
| S1 — Critical | | Open / Fixed |
| S2 — Major | | Open / Fixed |
| S3 — Minor | | Open / Fixed |
| S4 — Trivial | | Open / Fixed |
| **Tổng** | | |

---

### 3. Verdict

**Release Decision:** `🟢 GO` / `🔴 NO-GO` / `🟡 GO WITH CONDITIONS`

**Lý do:**

**Conditions (nếu GO WITH CONDITIONS):**
- [ ]
- [ ]

---

### 4. Bug detail (S1 và S2 bắt buộc, S3/S4 optional)

| # | Ticket | Severity | Mô tả ngắn | Repro | Dev assigned |
|---|---|---|---|---|---|
| 1 | | S1 | | | |
| 2 | | S2 | | | |

---

### 5. Observation — Không phải bug, nhưng cần lưu ý

> Những điểm kỹ thuật, UX, hoặc business logic có vẻ không ổn nhưng chưa đủ evidence để log bug.

-
-

---

### 6. Dev DoR gap (nếu có)

> Bugs nào lẽ ra đã bị catch ở Dev testing? Step SA nào bị miss?

| Bug | Step bị miss | Ghi chú |
|---|---|---|
| | | |

→ Nếu pattern lặp lại ≥2 lần: trigger upskilling session với team

---

### 7. Follow-up

| Action | Owner | Deadline |
|---|---|---|
| Fix S1/S2 bugs | Dev | |
| Re-verify sau fix | QC | |
| Update DoR/Playbook nếu pattern mới | Dung | |
