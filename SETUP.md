# Setup Guide — Knowledge Base

> Hướng dẫn cài đặt để Knowledge Base này hoạt động với Claude.

---

## 1. Cấu trúc thư mục

```
~/knowledge-base/
├── Home.md                    ← Central hub
├── Memory.md                  ← AI onboarding doc (QUAN TRỌNG NHẤT)
├── SETUP.md                   ← File này
│
├── roster/
│   ├── team-roster.md         ← QC team members
│   └── project-roster.md     ← Active projects
│
├── actions/
│   └── Action-Tracker.md     ← Open tasks & deadlines
│
├── frameworks/
│   ├── qc-testing-process.md ← 7-bước QC process
│   ├── release-checklist.md  ← Release checklist
│   ├── 1on1-framework.md     ← 1:1 guide
│   └── ai-subagent-guide.md  ← AI subagent prompts
│
└── templates/
    ├── meeting-notes.md      ← Meeting template
    ├── call-summary.md       ← Call notes template
    ├── 1on1.md               ← 1:1 template
    ├── sprint-review.md      ← Sprint review template
    └── daily-brief.md        ← Daily brief template
```

---

## 2. Cài đặt Obsidian (tuỳ chọn nhưng khuyến nghị)

1. Download Obsidian miễn phí tại obsidian.md
2. Open vault → chọn thư mục `~/knowledge-base/`
3. Tất cả file markdown sẽ hiển thị với links clickable

**Tip:** Đặt vault trong Google Drive để sync đa máy:
```
~/Google Drive/knowledge-base/
```

---

## 3. Cài đặt Obsidian MCP cho Claude

MCP server cho phép Claude đọc/ghi trực tiếp vào vault.

### Bước 1 — Cài Node package
```bash
npm install -g obsidian-mcp
```

### Bước 2 — Cấu hình trong `.mcp.json`
Thêm vào `~/.claude/mcp.json` hoặc `~/second-brain/.mcp.json`:

```json
{
  "mcpServers": {
    "obsidian-kb": {
      "command": "obsidian-mcp",
      "args": ["--vault", "/Users/dung/knowledge-base"]
    }
  }
}
```

### Bước 3 — Verify
Mở Claude Code và test:
```
Đọc file Memory.md trong knowledge base
```

---

## 4. Custom Instruction cho Claude

Thêm vào `CLAUDE.md` hoặc user preferences của Claude:

```
Trước khi trả lời bất kỳ câu hỏi nào, luôn đọc ~/knowledge-base/Memory.md để có context đầy đủ.

Khi nhận được thông tin từ cuộc họp hoặc session:
- Action items → ~/knowledge-base/actions/Action-Tracker.md
- Quyết định dự án → ~/knowledge-base/roster/project-roster.md
- Thông tin team → ~/knowledge-base/roster/team-roster.md
- Ghi chú họp → ~/second-brain/meetings/YYYY-MM-DD-tên.md
- Ghi chú 1:1 → ~/second-brain/people/tên.md
```

---

## 5. Tích hợp với second-brain

Knowledge Base này **không thay thế** second-brain mà bổ sung:

| | `~/knowledge-base/` | `~/second-brain/` |
|---|---|---|
| **Mục đích** | Context cho AI, quy trình, templates | Raw notes, meetings, daily log |
| **Tần suất đọc** | Đầu mỗi session (AI đọc) | Khi cần tìm chi tiết |
| **Cập nhật** | Khi có thay đổi lớn (team, dự án, goals) | Hàng ngày |
| **Format** | Structured, synthesized | Raw, detailed |

**Luồng thông tin:**
```
Cuộc họp → second-brain/meetings/ → Claude xử lý → knowledge-base/actions/ + roster/
```

---

## 6. Workflow hàng ngày gợi ý

**Sáng:**
```
Bạn: "daily brief hôm nay"
Claude: đọc Memory.md + Action Tracker + calendar → tạo daily brief
```

**Sau cuộc họp:**
```
Bạn: [paste transcript hoặc ghi chú họp]
Claude: extract actions → Action Tracker, quyết định → project roster
```

**1:1:**
```
Bạn: "chuẩn bị 1:1 với Duyên"
Claude: đọc people/duyen.md + Action Tracker → tóm tắt + chủ đề gợi ý
```

**Tổng kết tuần:**
```
Bạn: "tổng kết tuần này"
Claude: đọc Action Tracker + meetings tuần này → summary + next week plan
```
