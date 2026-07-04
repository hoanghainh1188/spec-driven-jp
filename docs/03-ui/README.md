# UI reference — link Figma, KHÔNG phải file thiết kế thật

Figma là nguồn UI duy nhất. Thư mục này mỗi feature chỉ chứa:
- `figma-links.md` — link file/node ID Figma, **ghi rõ mỗi node là màn hình gì** (design-intake
  dựa vào đó để map UI ↔ behavior).
- `screenshots/` — ảnh chụp mốc TẠI THỜI ĐIỂM code được sinh; đây là **fallback** khi không có
  Figma MCP (design-intake sẽ đọc snapshot thay vì gọi Figma trực tiếp).

Format gợi ý cho `figma-links.md`:

```
## <ten-man-hinh>
- Figma node: <link>
- Snapshot ngày: YYYY-MM-DD
- Sinh code từ commit: <git-sha>
```
