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

5. Kiểm thử trên Production
   └── Smoke test, verify chức năng sau deploy

6. Đánh giá kết quả & báo cáo
   └── Tổng hợp kết quả, viết báo cáo
   └── [AI] Dùng RCA Assistant khi có bug production

7. Đóng dự án
   └── Cập nhật document, lesson learned trên BookStack
```

---

## Trách nhiệm

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
| Manual Functional | Tất cả QC Engineers | Redmine, Test case template |
| API Testing | QC Core/Integration | Postman |
| Automation | Thắng Trương | Robot Framework, Selenium |
| Performance | Thắng Trương | JMeter, K6 |
| Visual AI | Thắng Trương (research) | Applitools Eyes |
| Unit/Feature (Shift-Left) | Dev team | SonarQube, JUnit |

---

## Checklist trước khi bàn giao QC

- [ ] Requirement đã được làm rõ, không còn mâu thuẫn
- [ ] Test case đã viết đủ, có review chéo
- [ ] Test data đã chuẩn bị
- [ ] Risk đã được identify
- [ ] Dev đã pass unit test (Shift-Left)
- [ ] Code coverage ≥60% trên SonarQube

---

## Liên kết

- [Release Checklist](release-checklist.md)
- [AI Subagent Guide](ai-subagent-guide.md)
- BookStack: https://bookstack.gotit.vn/books/pd-c-qc-team
