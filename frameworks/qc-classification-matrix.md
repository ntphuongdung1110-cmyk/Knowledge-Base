# QC Classification Matrix — Phân loại sản phẩm

**Owner:** Dung Nguyễn | **Tạo:** 2026-05-04 | **Cập nhật:** 2026-05-06
**Áp dụng cho:** 2 Quality Engineers (TBD — Thắng nghỉ việc, Khải → PO tháng 05/2026)

---

## Mục đích

Xác định rõ sản phẩm nào cần **QC sweep bắt buộc**, sản phẩm nào Dev tự chứng nhận — để 2 QC tập trung đúng chỗ, không bị kéo vào test toàn bộ.

**QA audit layer áp dụng trên tất cả tiers:**

| Tier | QC action | QA action |
|---|---|---|
| Tier 1 — Critical | Sweep bắt buộc | Audit Layer 1+2+3 + Go/No-Go mỗi release |
| Tier 2 — Standard | Sample 20–30% | Spot-check 15–20% stories/sprint |
| Tier 3 — Basic | Dev self-certify | Spot-check khi có production bug |

> QA audit là **độc lập** với QC sweep — QA kiểm tra evidence, không test lại.

---

## Tiêu chí phân loại

| Tiêu chí | Trọng số | Mô tả |
|---|---|---|
| **Rủi ro tài chính** | Cao | Liên quan đến tiền, voucher, điểm, thanh toán |
| **Visibility người dùng** | Cao | External user thấy trực tiếp vs internal only |
| **Revenue impact** | Cao | Nếu bug lọt → ảnh hưởng doanh thu như thế nào |
| **Tần suất thay đổi** | Trung bình | Thay đổi nhiều = rủi ro regression cao hơn |

---

## Bảng phân loại

### 🔴 Tier 1 — Critical (QC sweep bắt buộc trước mỗi release)

> 2 QC phải sweep, không được bỏ qua. Nếu thiếu người → Dung quyết định go/no-go.

| Sản phẩm | Lý do Critical | Scrum Team | QC phụ trách sweep |
|---|---|---|---|
| **GotIt App** | DAU cao nhất, direct consumer, financial flow | User Solutions | TBD |
| **Voucher Link (v/g/e)** | Core revenue product, voucher redemption | User Solutions | TBD |
| **GI Mini App (ZaloPay)** | Financial integration bên thứ 3, high risk | User Solutions | TBD |
| **Core API (Biz/POS)** | B2B revenue critical, API contract với partners | Voucher Service | TBD |
| **Home Delivery** | External consumer, real-time coordination | User Solutions | TBD |
| **Campaign Loyalty** | Financial logic (điểm/thưởng), rule phức tạp | Client Solutions | TBD |
| **Biz Order** | B2B revenue, contract với doanh nghiệp | Voucher Service | Thắng (API) |

**Sweep trigger:** Mọi release có code change ở Tier 1 → sweep bắt buộc, không exception.

---

### 🟡 Tier 2 — Standard (AI pipeline + QC sampling)

> Dev chạy SA0–SA5 + DoR. QC sample 20–30% stories mỗi sprint. QC sweep chỉ khi có dấu hiệu rủi ro cao (feature mới lớn, sau incident).

| Sản phẩm | Lý do Standard | Scrum Team | QC action |
|---|---|---|---|
| **Campaign Tool** | Thay đổi nhiều nhưng AI cover được logic | Client Solutions | Sample + sweep khi feature mới lớn |
| **Campaign CMS** | Internal B2B config, bounded scope | Client Solutions | Sample hàng sprint |
| **Cover Link** | Consumer-facing nhưng tần suất thay đổi thấp | User Solutions | Sample + sweep khi release mới |
| **GI Web** | Web version của App, lower traffic | User Solutions | Sample hàng sprint |
| **Webview** | Embedded trong App, thay đổi ít | User Solutions | Sample khi có change |
| **Scanit AI** | AI product mới, cần theo dõi chất lượng AI output | Client Solutions | Sample + manual khi update model |
| **Biz CMS** | Internal B2B tool, low external visibility | Voucher Service | Sample hàng tháng |
| **SMS Gateway** | Backend service, ít UI | Voucher Service | Automation cover |
| **Fraud** | Risk detection, thay đổi rule thỉnh thoảng | Voucher Service | Sweep khi rule change |
| **Portal** | Partner-facing nhưng ít thay đổi | Voucher Service | Sample khi có release |
| **IPO (Odoo)** | Internal ERP, Dung là PM — UAT riêng | — | Theo UAT plan riêng |

---

### 🟢 Tier 3 — Basic (Dev self-certify, không cần QC sweep)

> Dev tự chứng nhận theo DoR. QC không sweep. Nếu production bug → RCA và xem xét nâng tier.

| Sản phẩm | Lý do Basic | Scrum Team | Dev action |
|---|---|---|---|
| **Admin** | Internal only, không có financial logic | Voucher Service | Dev self-certify |
| **Internal Tools / HCV** | Internal team tools | — | Dev self-certify |
| **PD Support tools** | Internal utility | — | Dev self-certify |
| **BackOffice** | *(Đã confirm trong qc-scope-v2)* — Dev self-certify, rủi ro chấp nhận được | Voucher Service | Dev self-certify |

---

## Override Rules — Nâng tier tự động

Bất kể sản phẩm ở tier nào, **tự động nâng lên Tier 1** khi:

| Điều kiện | Lý do |
|---|---|
| Release sau production incident trong 30 ngày | Risk recurrence cao |
| Feature mới ảnh hưởng financial flow | Bất kể product tier |
| Migration data hoặc schema change | Data integrity risk |
| Tích hợp API bên thứ 3 mới | External dependency, khó predict |
| Dev mới làm tính năng lần đầu | Thiếu context domain |
| Release bị delay >2 sprint | Tích lũy change lớn |

---

## Sweep cadence theo Tier

| Tier | Khi nào sweep | Thời gian tối đa | Báo cáo |
|---|---|---|---|
| **Critical** | Mọi release có code change | 4 giờ/sản phẩm | Post-sweep report trong ngày |
| **Standard** | Khi override trigger, hoặc sample hàng sprint | 2 giờ/sample | Weekly quality health |
| **Basic** | Không sweep — nhưng monitor bug từ production | — | Monthly nếu có incident |

---

## Liên kết

- [QA Role Definition](qa-role-definition.md) — QA audit layer và Go/No-Go
- [QA Audit Checklist](qa-audit-checklist.md) — QA làm gì với từng tier
- [Dev Test DoR](dev-test-dor.md) — Tiêu chí Dev phải đạt trước khi handover
- [QC Sweep Checklist](qc-sweep-checklist.md) — Checklist khi thực hiện sweep
- [AI Subagent Guide](ai-subagent-guide.md) — SA0–SA5
- [QC Scope v2](~/second-brain/projects/qc-transformation/qc-scope-v2.md)
