# spec-driven-jp — Template dự án tích hợp Spec Kit + đọc tài liệu thiết kế tiếng Nhật

Template cho các dự án greenfield dùng Claude Code, đặc biệt phù hợp với bối cảnh **BrSE làm việc với khách hàng Nhật**: có tài liệu basic design (基本設計), detail design (詳細設計), và Figma làm nguồn thiết kế đầu vào.

Tích hợp:
- **[GitHub Spec Kit](https://github.com/github/spec-kit)** làm khung xương spec-driven (constitution + specify → clarify → plan → tasks → analyze → implement)
- **2 subagent chuyên biệt** bù đắp phần Spec Kit không có: `design-intake` (đọc tài liệu Nhật + Figma qua MCP) và `code-reviewer` (đối chiếu code với 4 nguồn sau khi implement)
- **Glossary Nhật-Việt-Anh** làm nguồn thuật ngữ chung
- **Permission gate** cấu hình sẵn theo nguyên tắc deny → ask → allow

## Có 2 cách dùng template này

### Cách 1: Dùng như GitHub template repo (khuyến nghị cho hầu hết trường hợp)

1. Fork/clone repo này lên GitHub tài khoản của bạn.
2. Vào Settings → chọn "Template repository" để biến nó thành template.
3. Với mỗi dự án mới, bấm "Use this template" → "Create a new repository".
4. `git clone` repo mới về máy, chạy `bash bootstrap.sh` để hoàn tất setup (chạy `specify init` để tạo `.specify/` và các slash command Spec Kit).
5. Chạy `/speckit.constitution` trong Claude Code — sau đó xem `docs/CONSTITUTION_APPEND.md` để bổ sung các article cho bối cảnh Nhật.

### Cách 2: Cài đặt như Claude Code plugin

Cách này phù hợp khi bạn muốn dùng bộ agent + command cho **nhiều dự án đã có sẵn**, không phải chỉ dự án mới.

```bash
# Trong Claude Code
/plugin marketplace add hoanghainh1188/spec-driven-jp
/plugin install spec-driven-jp@hoanghainh1188
```

Plugin sẽ cài `.claude/agents/design-intake.md`, `.claude/agents/code-reviewer.md`, và command `/design-to-code` vào user scope (`~/.claude/`). Xem chi tiết ở `plugin/README.md`.

**Lưu ý**: cách plugin không tự tạo cấu trúc `docs/` hay chạy `specify init` — bạn cần làm những bước đó thủ công nếu dự án chưa có sẵn. Cách 1 (template repo) tự động hóa cả 2.

## Cấu trúc dự án được sinh ra

```
<project>/
├── CLAUDE.md                       Quy ước dự án, agent đọc đầu tiên
├── bootstrap.sh                    Chạy specify init + bổ sung agent/command
├── .claude/
│   ├── settings.json              Permission deny/ask/allow đã cấu hình sẵn
│   ├── agents/
│   │   ├── design-intake.md       Đọc docs Nhật + Figma, sinh input cho /speckit.specify
│   │   └── code-reviewer.md       Review code sau /speckit.implement
│   └── commands/
│       └── design-to-code.md      Điều phối toàn bộ pipeline hybrid
├── docs/
│   ├── 00-glossary.md              Thuật ngữ 日本語 / VI / EN — nguồn duy nhất
│   ├── 01-basic-design/            基本設計 gốc từ khách hàng (không sửa tay)
│   ├── 02-detail-design/           詳細設計 gốc
│   ├── 03-ui/                      Link Figma + snapshot
│   ├── 04-decisions/               Câu trả lời cho /speckit.clarify
│   ├── intake/                     Output của design-intake
│   └── CONSTITUTION_APPEND.md      Gợi ý các article cho bối cảnh Nhật
├── src/
├── tests/
└── plugin/                         Metadata để dùng như Claude Code plugin
    ├── plugin.json
    └── README.md
```

Sau khi chạy `bootstrap.sh`, thêm:
```
├── .specify/
│   ├── memory/constitution.md      Do /speckit.constitution sinh
│   ├── templates/                  Template Spec Kit
│   └── scripts/                    Helper script Spec Kit
├── .claude/commands/speckit.*.md   7 slash command của Spec Kit
└── specs/                          Do Spec Kit sinh mỗi feature 1 thư mục
```

## Workflow cho 1 feature

1. `git checkout -b 001-<feature-slug>` — Spec Kit dùng tên branch để phát hiện feature.
2. Đặt tài liệu vào `docs/01-basic-design/<feature>/`, `docs/02-detail-design/<feature>/`.
3. Link Figma vào `docs/03-ui/<feature>/figma-links.md`.
4. Gõ `/design-to-code` trong Claude Code — command điều phối toàn bộ pipeline, dừng xin xác nhận ở các checkpoint quan trọng.

## Yêu cầu môi trường

- Claude Code (bản mới nhất, hỗ trợ subagent + command + settings.json)
- `uv` hoặc `pipx` để cài Spec Kit CLI (`uvx --from git+https://github.com/github/spec-kit.git specify init ...`)
- `git`
- **Figma MCP server** — để `design-intake` đọc Figma tự động, server PHẢI được đăng ký đúng tên
  `figma` (agent grant `mcp__figma__*`). Kiểm tra bằng `claude mcp list`; nếu server của bạn tên khác
  (VD `claude_ai_Figma`), đổi tên server hoặc sửa dòng `tools:` trong `.claude/agents/design-intake.md`.
  Nếu không có Figma MCP, `design-intake` vẫn chạy — nó đọc link/snapshot trong `docs/03-ui/`.
- **Skill đọc tài liệu** (`docx`, `xlsx`, `pdf`) — cần để `design-intake` trích nội dung file Office/PDF
  nhị phân (tool `Read` thuần không parse được). Nếu skill chưa cài, chuẩn bị sẵn bản export
  text/markdown của tài liệu đặt cạnh file gốc.

## Giấy phép

MIT — dùng thoải mái, sửa thoải mái. Nếu bạn cải tiến template, PR về là rất được hoan nghênh.
