# Basic design (基本設計) — tài liệu GỐC từ khách hàng

Mỗi feature 1 thư mục: `docs/01-basic-design/<feature>/`
Versioned (`v1.docx`, `v2.docx`…) + `CHANGELOG.md` khi có bản mới.
Agent KHÔNG BAO GIỜ sửa nội dung ở đây.

## Để `design-intake` dùng được (quan trọng)

`Read` KHÔNG parse được `.docx/.xlsx/.pdf` nhị phân. Cho mỗi feature, đảm bảo **một trong hai**:
- Skill `docx/xlsx/pdf` đã cài, HOẶC
- Đặt sẵn **bản export text/markdown** cạnh file gốc (VD `basic-design-v1.md`).

Layout gợi ý:

```
docs/01-basic-design/<feature>/
├── basic-design-v1.docx     # gốc khách (không sửa)
├── basic-design-v1.md       # ★ export text nếu chưa có Skill đọc Office
└── CHANGELOG.md
```

`design-intake` trích từ basic design: **user story / behavior** (làm gì, cho ai). Đảm bảo tài liệu
nêu rõ mục tiêu + phạm vi feature — đây là input cho `/speckit-specify`.
