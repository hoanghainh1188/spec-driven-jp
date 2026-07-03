# spec-driven-jp — Chế độ Claude Code Plugin

Chế độ này chỉ cài **2 subagent + 1 command** vào Claude Code của bạn, KHÔNG tạo cấu trúc thư mục `docs/`, không chạy `specify init`. Phù hợp khi bạn muốn dùng bộ agent cho **nhiều dự án có sẵn**.

## Cài đặt

```
# Trong Claude Code
/plugin marketplace add <owner>/<repo-name>
/plugin install spec-driven-jp
```

Sau khi cài, các file sau sẽ có mặt trong `~/.claude/`:
- `~/.claude/agents/design-intake.md`
- `~/.claude/agents/code-reviewer.md`
- `~/.claude/commands/design-to-code.md`

## Bạn cần chuẩn bị thủ công (nếu dùng chế độ plugin)

Vì plugin không tự tạo scaffold, dự án của bạn cần có sẵn:

1. **Cài Spec Kit**: chạy `uvx --from git+https://github.com/github/spec-kit.git specify init . --integration claude` trong repo dự án.
2. **Tạo constitution**: chạy `/speckit.constitution` trong Claude Code.
3. **Cấu trúc docs/** cho tài liệu Nhật (khuyên dùng, không bắt buộc):
   ```
   docs/
   ├── 00-glossary.md
   ├── 01-basic-design/
   ├── 02-detail-design/
   ├── 03-ui/
   ├── 04-decisions/
   └── intake/
   ```
4. **Figma MCP server** (nếu muốn design-intake đọc Figma tự động).

Nếu bạn muốn tất cả những thứ trên được tạo tự động, hãy dùng **chế độ template repo** thay vì plugin — xem README chính ở gốc repo.

## Khi nào chọn plugin vs template?

| Tình huống | Chọn |
|---|---|
| Dự án mới hoàn toàn | Template repo (có sẵn scaffold) |
| Dự án đã có, muốn thêm workflow spec-driven | Plugin (nhẹ, không đụng cấu trúc sẵn) |
| Nhiều dự án cần dùng chung 1 bộ agent | Plugin (cài vào user scope, mọi project dùng chung) |
| Dự án 1 người, ít quan tâm chia sẻ | Template repo (đơn giản hơn) |

## Gỡ cài đặt

```
/plugin uninstall spec-driven-jp
```
