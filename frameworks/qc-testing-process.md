# QC Testing Process — 7 Bước

**Nguồn:** BookStack — [C]QUY TRINH TỔNG QUÁT VÀ CHI TIẾT TEAM QC
**Cập nhật:** 2026-04-21

---

## Quy trình tổng quát

```
1. Tiếp nhận yêu cầu
   └── QC nhận thông tin dự án + tài liệu đặc tả từ QC Manager

2. Phân tích yêu cầu
   └── QC + BA phân tích tài liệu, đặt câu hỏi, lên kế hoạch kiểm thử
   └── [AI] Dùng Requirement Clarifier để phát hiện mâu thuẫn/assumption

3. Thiết kế kịch bản kiểm thử
   └── Viết test case/checklist bao phủ tất cả trường hợp
   └── [AI] Dùng Risk Scout + Test Designer để mở rộng coverage

4. Thực hiện kiểm thử trên STG
   ├── 4.1. Testing (chạy test case)
   ├── 4.2. Log bug vào Redmine/Jira
   └── 4.3. Retest sau khi Dev fix
   └── [AI] Dùng Bug Hunter để tìm bug khó thấy

4.5. QA Audit & Go/No-Go Gate  ← [BƯỚC MỚI — QA thực hiện độc lập]
   ├── QA audit evidence của Dev (SA pipeline, SonarQube, security)
   ├── QA audit TC của QC (format, coverage vs spec, pass/fail completeness)
   ├── QA smoke test trên STG (critical flows — không phải full regression)
   └── QA ra quyết định Go/No-Go — chỉ QA có quyền này
       → GO: Deploy lên Production
       → NO-GO: Dev/QC fix theo QA findings, quay lại bước 4
       → GO WITH CONDITIONS: Deploy với acknowledged risks

5. Kiểm thử trên Production
   └── Smoke test, verify chức năng sau deploy (QC thực hiện)

6. Đánh giá kết quả & báo cáo
   └── Tổng hợp kết quả, viết báo cáo
   └── [AI] Dùng RCA Assistant khi có bug production
   └── QA phân tích process gap nếu bug escaped qua gate

7. Đóng dự án
   └── Cập nhật document, lesson learned trên BookStack
```

---

## Trách nhiệm

### QA (Quality Assurance) — Team độc lập
- Audit evidence của Dev và QC — **không** test lại toàn bộ
- Quyết định **Go/No-Go** trước mỗi release — quyền độc quyền của QA
- Xây dựng và duy trì quality standards, templates, DoR
- Spot-check ngẫu nhiên bất kỳ bước nào trong quy trình
- Phân tích root cause bug lặp lại → cải tiến quy trình
- Xem chi tiết: [QA Role Definition](qa-role-definition.md)

### QC Manager (Dung)
- Theo dõi, quản lý và kiểm soát chất lượng toàn team
- Lên kế hoạch, thiết lập phương án kiểm thử
- Phân công công việc
- Đào tạo và phát triển team
- Đảm bảo tính bảo mật – toàn vẹn – sẵn sàng của sản phẩm

### QC Engineer
- Thực hiện nghiêm túc quy trình
- Phân tích tài liệu, viết test case, thực thi
- Phối hợp với Dev trong việc fix bug
- Cập nhật Daily Log hàng ngày
- Báo cáo kịp thời khi phát hiện sai sót

---

## Loại kiểm thử

| Loại | Người thực hiện | Tool |
|---|---|---|
| Manual Functional | QC Engineers | Redmine, Test case template |
| API Testing | QC Engineers | Postman |
| Automation | Automation Engineer (TBD) | Robot Framework, Selenium |
| Performance | Automation Engineer (TBD) | JMeter, K6 |
| Visual AI | Automation Engineer (TBD) | Applitools Eyes |
| Unit/Feature (Shift-Left) | Dev team | SonarQube, JUnit |

---

## Checklist trước khi bàn giao QC

- [ ] Requirement đã được làm rõ, không còn mâu thuẫn
- [ ] Test case đã viết đủ, có review chéo
- [ ] Test data đã chuẩn bị
- [ ] Risk đã được identify
- [ ] Dev đã pass unit test (Shift-Left)
- [ ] Code coverage ≥90% (code mới/changed) trên SonarQube — xem [Quality Standards](qa-quality-standards.md)

---

## Liên kết

- [QA Role Definition](qa-role-definition.md) — Scope và trách nhiệm QA
- [QA Audit Checklist](qa-audit-checklist.md) — QA làm gì ở bước 4.5
- [Release Checklist](release-checklist.md)
- [AI Subagent Guide](ai-subagent-guide.md)
- BookStack: https://bookstack.gotit.vn/books/pd-c-qc-team
