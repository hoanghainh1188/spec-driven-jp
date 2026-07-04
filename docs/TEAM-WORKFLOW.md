# Quy trình làm việc nhóm

Tài liệu này mô tả cách **nhiều người cùng làm 1 dự án** dựng từ template này mà **không gây git
conflict** và **không lệch ngữ cảnh** (glossary, constitution, design gốc). Đọc kèm `CLAUDE.md`
(quy ước bắt buộc) và `README.md` (workflow 1 feature).

---

## 1. Repo & branch model

- **1 repo chung**, nhánh mặc định `main` luôn xanh (CI pass, deploy được).
- Mỗi feature = **1 branch ngắn hạn** tách từ `main`, merge lại qua **Pull Request**.
- Merge vào `main` chỉ khi: **CI xanh** + **review pass** (code-owner nếu đụng file gác cổng).
- **Ưu tiên branch sống ngắn** (gộp trong vài ngày). Branch càng sống lâu, càng dễ lệch ngữ cảnh
  so với glossary/constitution mới trên `main`.

---

## 2. Quy ước đánh số feature — dùng số issue GitHub

Đây là chốt chặn chính chống **trùng ID feature** khi nhiều người tạo branch song song.

1. **Mọi feature bắt đầu bằng 1 GitHub issue** (dùng `Feature` issue template). **Số issue = feature ID.**
2. **Branch**: `NNN-<slug>` với `NNN` = số issue **zero-pad tối thiểu 3 chữ số**.
   - VD: issue `#42` → `042-user-reservation`; issue `#2342` → `2342-workflow-cli`.
   - Spec Kit validate branch sequential yêu cầu tiền tố **≥ 3 chữ số**, nên số nhỏ phải zero-pad.
   - Số issue là duy nhất toàn cục → **không bao giờ trùng** giữa các thành viên.
3. **Đánh số thư mục `specs/` = `timestamp` (bootstrap đã cấu hình sẵn — không cần làm gì thêm).**
   `bootstrap.sh` đặt `"feature_numbering": "timestamp"` trong `.specify/init-options.json`, nên
   `/speckit.specify` sinh thư mục `specs/<YYYYMMDD-HHMMSS>-<slug>` — **duy nhất toàn cục, không quét
   `specs/`**. Đã kiểm chứng qua tài liệu Spec Kit: base `/speckit.specify` dựng tên thư mục theo
   `feature_numbering` (quét `specs/` khi `sequential`, dùng timestamp khi `timestamp`), **không** suy
   từ tên branch — nên timestamp là cách chống trùng đáng tin nhất khi chạy song song.

**Hai lớp số — KHÔNG cần khớp nhau:**

| Lớp | Giá trị | Vai trò |
|---|---|---|
| Tên **branch** | `NNN-<slug>` (NNN = số issue) | PR / `Closes #` / traceability con người |
| Thư mục **`specs/`** | `<YYYYMMDD-HHMMSS>-<slug>` (timestamp) | Nội bộ Spec Kit, chống trùng song song |

Traceability đi qua **branch + số issue + `docs/intake` + `docs/04-decisions`**, KHÔNG qua tiền tố
`specs/`. Nếu team thực sự muốn thư mục `specs/` mang số issue: đổi `feature_numbering` → `sequential`
và tự quản (chấp nhận rủi ro trùng khi song song), hoặc dùng override số của Spec Kit git-extension.
Đây là lựa chọn có đánh đổi — **mặc định timestamp an toàn hơn cho nhóm.**

> Gợi ý: giữ `slug` kebab-case, ngắn gọn, tra `docs/00-glossary.md` để đặt tên nhất quán nghiệp vụ.

---

## 3. Vòng đời 1 feature (nhóm)

```
1. Tạo GitHub issue (Feature template)         → có số issue = ID
2. git checkout main && git pull               → nền mới nhất
3. git checkout -b NNN-<slug>                   (NNN = số issue, zero-pad ≥3)
4. Đặt tài liệu vào docs/01-basic-design/<feature>/, 02-detail-design/, 03-ui/
5. /design-to-code                              → intake → speckit → implement → review → test
6. Mở PR "Closes #<issue>"                      (điền PR template)
7. code-reviewer + glossary-steward + security-reviewer + human review; sửa Blocking
8. Test gate: xanh + coverage ≥ ngưỡng constitution (Article W, mặc định 80%)
9. CI xanh + review pass → merge vào main       → tự đóng issue
```

Một người **sở hữu 1 feature end-to-end** (pipeline `/design-to-code` không có điểm bàn giao giữa
chừng — chuyển tay giữa dòng dễ mất ngữ cảnh intake). Nếu buộc phải chuyển, ghi lại trạng thái vào
`docs/intake/<feature>.md` để người sau nạp ngữ cảnh.

---

## 4. Chống lệch ngữ cảnh (context drift)

Nguồn ngữ cảnh dùng chung: `docs/00-glossary.md`, `.specify/memory/constitution.md`, tài liệu gốc
`docs/01-03`, và `docs/04-decisions/`. Khi chạy song song, chúng dễ lệch giữa các branch.

- **Rebase `main` thường xuyên** (khuyến nghị hằng ngày) vào branch đang làm để hút glossary/constitution
  mới nhất.
- **Khi constitution hoặc glossary vừa đổi trên `main`**: mọi branch đang chạy phải **rebase** và
  **chạy lại `/speckit.analyze`** để phát hiện code/spec đã lệch nguyên tắc/thuật ngữ mới.
- **Giữ feature nhỏ** → cửa sổ lệch ngắn.
- **Trước khi hỏi lại 1 ambiguity**, tra `docs/04-decisions/` — có thể người khác đã quyết rồi.

---

## 5. Gác cổng glossary + constitution

`docs/00-glossary.md` và `.specify/memory/constitution.md` là **file dùng chung, tác động toàn dự án**.
Gác cổng **theo mức blast radius**, không chặn cứng mọi thứ (tránh kẹt dev giữa feature):

| Thao tác | Cách làm | Vì sao |
|---|---|---|
| **THÊM** thuật ngữ mới (append 1 dòng glossary) | Làm **ngay trong branch feature**; steward review phần glossary khi mở PR (CODEOWNERS tự kéo) | Append ít đụng nhau; chặn cứng sẽ kẹt dev đang cần đặt tên |
| **SỬA / đổi tên / xoá** thuật ngữ đã có | **PR riêng, nhỏ**, steward duyệt | Đổi nghĩa 1 term lan ra toàn codebase — rủi ro cao |
| **Mọi thay đổi** `constitution.md` | **PR riêng, nhỏ**, steward duyệt | Thắng mọi thứ trong Spec Kit — blast radius tối đa |

- PR gác cổng (SỬA glossary / constitution) merge **sớm** vào `main` để cả team rebase đồng bộ, giảm phân kỳ.
- Steward khai trong `.github/CODEOWNERS`.

> ⚠️ **CODEOWNERS chỉ là gợi ý cho tới khi bật branch protection.** Nếu chưa bật (mục 7), GitHub
> **không** bắt buộc steward duyệt — cơ chế gác cổng âm thầm vô hiệu. Đây là bước setup **load-bearing**.

---

## 6. Cập nhật tài liệu gốc `docs/01-03` giữa chừng

Khi khách gửi bản design mới trong lúc đang code:

- **Không ghi đè** bản cũ. Thêm **file version mới** + cập nhật `CHANGELOG.md` trong thư mục tương ứng.
- Mở **issue re-run** cho các feature bị ảnh hưởng → chạy lại pipeline từ `design-intake` để spec/code
  bám bản design mới, và ghi quyết định phát sinh vào `docs/04-decisions/`.

---

## 7. Điểm nóng conflict & cách tránh

| File / thư mục | Rủi ro | Cách tránh |
|---|---|---|
| `.specify/memory/constitution.md` | **Cao** — 1 file chung | PR riêng + steward duyệt (mục 5); rebase khi đổi |
| `docs/00-glossary.md` | Trung bình — 1 bảng chung | PR riêng + steward; append dòng mới, không sửa lung tung |
| Tên branch | Thấp | `NNN-<slug>` = số issue → duy nhất toàn cục (mục 2) |
| Thư mục `specs/<prefix>-*` | Thấp | `feature_numbering=timestamp` → không quét, không trùng (mục 2) |
| `docs/04-decisions/<date>-<slug>.md` | Thấp | Mỗi quyết định 1 file có timestamp |
| `docs/intake/<feature>.md` | Thấp | Mỗi feature 1 file |
| `specs/<feature>/` | Thấp | Mỗi feature 1 thư mục, cô lập theo branch |
| `src/` | Theo module | Chia module rõ; feature nhỏ; rebase thường xuyên |

### Bật branch protection cho `main` (bắt buộc để các cơ chế trên có hiệu lực)
Settings → Branches → Add rule cho `main`:
- ☑ Require a pull request before merging
- ☑ Require approvals (≥1)
- ☑ Require review from **Code Owners**
- ☑ Require status checks to pass → chọn `template-smoke-test` (và CI dự án)
- ☑ Require branches to be up to date before merging (ép rebase → giảm drift)

---

## 8. Tóm tắt "phải nhớ"

1. Tạo **issue trước** → số issue là **ID feature** → branch `NNN-<slug>` (zero-pad ≥3).
2. **1 người sở hữu 1 feature** end-to-end; branch ngắn hạn; PR `Closes #issue`.
3. Sửa **glossary/constitution** → **PR riêng, steward duyệt**; rồi cả team rebase.
4. **Rebase `main` hằng ngày**; constitution/glossary đổi → chạy lại `/speckit.analyze`.
5. Bật **branch protection** để CODEOWNERS + CI thực sự chặn merge sai.
