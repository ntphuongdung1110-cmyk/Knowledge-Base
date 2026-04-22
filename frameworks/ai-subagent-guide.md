# AI Subagent Guide — QC Team

**Dùng khi:** Áp dụng AI subagents vào quy trình QC
**Cập nhật:** 2026-04-21

---

## Tổng quan

AI đóng vai **subagent** — không thay thế QC mà hỗ trợ từng bước cụ thể trong quy trình 7 bước.

```
[Nhận req] → [Phân tích] → [Viết TC] → [Chuẩn bị] → [Thực thi] → [Log bug] → [Retest]
      ↓              ↓              ↓                                    ↓
Req Clarifier    Risk Scout    Test Designer                       RCA Assistant
                                + Bug Hunter
```

---

## Subagent 1 — Requirement Clarifier

**Khi nào dùng:** Bước 2 — Nhận requirement mới

**Input:**
```
Context: [Mô tả feature/hệ thống liên quan]
Task: Phân tích tài liệu đặc tả dưới đây và:
  1. Liệt kê tất cả assumption ẩn
  2. Phát hiện mâu thuẫn logic
  3. Điểm mờ cần làm rõ với BA/PO
  4. Câu hỏi cần hỏi trước khi test
Constraints: Trả về dạng danh sách theo mức độ ưu tiên
Document: [paste tài liệu]
```

**Output mong đợi:** Danh sách câu hỏi, mâu thuẫn, assumption theo priority

---

## Subagent 2 — Risk Scout

**Khi nào dùng:** Sau khi requirement đã rõ, trước khi viết test case

**Input:**
```
Context: [Feature description + business context]
Task: Identify risk matrix mở rộng:
  1. Business risk (sai nghiệp vụ)
  2. Financial risk (ảnh hưởng tiền/voucher)
  3. Data risk (corrupt, mất, duplicate)
  4. Security risk
  5. Edge cases tiềm ẩn
Output: Risk matrix theo priority (High/Medium/Low)
```

---

## Subagent 3 — Test Designer

**Khi nào dùng:** Bước 3 — Thiết kế test scenarios

**Input:**
```
Context: [Feature + Risk list từ Risk Scout]
Task: Sinh test ideas theo risk-based approach:
  1. Test scenarios cho từng risk (High trước)
  2. Happy path + negative cases
  3. Boundary conditions
  4. Integration points cần check
Format: Nhóm theo risk level, có rationale
```

---

## Subagent 4 — Bug Hunter

**Khi nào dùng:** Trong quá trình test, khi muốn đào sâu

**Input:**
```
Context: [Feature đang test + những gì đã test]
Task: Gợi ý cách "phá" hệ thống:
  1. Attack vectors chưa test
  2. Negative scenarios có thể gây crash
  3. Concurrent/race condition scenarios
  4. Security edge cases
  5. Data manipulation scenarios
```

---

## Subagent 5 — RCA Assistant

**Khi nào dùng:** Khi có bug production hoặc bug khó reproduce

**Input:**
```
Context: [Hệ thống liên quan]
Bug: [Mô tả bug, steps to reproduce, expected vs actual]
Logs: [Log liên quan nếu có]
Task: 
  1. Liệt kê giả thuyết root cause (theo xác suất)
  2. Bước reproduce từng giả thuyết
  3. Hướng fix đề xuất cho từng case
  4. Prevention: cách test để bắt loại bug này trong tương lai
```

---

## Prompt Master Template

```
## Context
[Mô tả hệ thống/feature: tên, mục đích, luồng chính, tech stack nếu biết]

## Task
[Nhiệm vụ cụ thể của subagent — copy từ section tương ứng trên]

## Constraints
- Language: Tiếng Việt
- Format: Bullet points, nhóm theo priority
- Độ dài: Súc tích, không dài dòng
- Depth: Tập trung vào GotIt business context (voucher, e-gift, B2B)

## Input
[Paste tài liệu/requirement/bug description ở đây]
```

---

## Tracking kết quả

Sau mỗi lần dùng subagent, ghi vào project notes:
- Subagent nào đã dùng
- Kết quả: tìm được gì mới không?
- Effort tiết kiệm được ước tính
- Cải thiện prompt nếu output chưa tốt

→ Báo cáo AI adoption: Tab 3 Redmine Dashboard

---

## Liên kết

- [QC Testing Process](qc-testing-process.md)
- [Project Roster — QC AI Strategy](../roster/project-roster.md#qc-ai-strategy)
- BookStack: https://bookstack.gotit.vn/books/pd-c-qc-team/page/plan-apply-ai
