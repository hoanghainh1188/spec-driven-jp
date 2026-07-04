<!-- Xem quy trình đầy đủ: docs/TEAM-WORKFLOW.md -->

## Feature
Closes #<!-- số issue = feature ID -->

## Tóm tắt
<!-- Feature này làm gì, dựa trên tài liệu design nào -->

## Traceability (đã commit vào branch)
- [ ] `docs/intake/<feature>.md` — output của `design-intake`
- [ ] `docs/04-decisions/*` — mọi câu trả lời `/speckit.clarify` (nếu có ambiguity)
- [ ] `specs/<feature>/` — spec.md / plan.md / tasks.md do Spec Kit sinh

## Chất lượng
- [ ] Đã chạy subagent `code-reviewer`, xử lý hết mục **Blocking**
- [ ] Đã chạy `glossary-steward` (term lệch đã sửa) và `security-reviewer` (nếu feature đụng data/auth/API)
- [ ] Test gate xanh: `npm run lint` / `test` / `build` (hoặc tương đương của dự án)
- [ ] Coverage đạt ngưỡng constitution (Article W — mặc định ≥ 80% business logic)
- [ ] CI `template-smoke-test` (và CI dự án) xanh

## File dùng chung (gác cổng)
- [ ] PR này **không** sửa `docs/00-glossary.md` hay `.specify/memory/constitution.md`
      → *nếu có sửa, đã tách thành PR riêng và được **code-owner** duyệt (xem `.github/CODEOWNERS`)*
- [ ] Đã rebase lên `main` mới nhất; nếu glossary/constitution vừa đổi → đã chạy lại `/speckit.analyze`

## Branch
- [ ] Tên branch dạng `NNN-<slug>` với `NNN` = số issue (zero-pad ≥ 3 chữ số)
