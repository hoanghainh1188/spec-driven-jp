---
name: Feature
about: Khởi tạo 1 feature mới — số issue này sẽ là ID feature (branch NNN-<slug>)
title: "[feature] <slug ngắn gọn>"
labels: feature
---

<!--
Số issue này = FEATURE ID. Sau khi tạo, branch làm feature đặt tên:
    <NNN>-<slug>    với NNN = số issue zero-pad tối thiểu 3 chữ số
    VD issue #42 → branch 042-user-reservation
Xem docs/TEAM-WORKFLOW.md.
-->

## Slug đề xuất
<!-- kebab-case, ngắn, tra docs/00-glossary.md cho nhất quán nghiệp vụ. VD: user-reservation -->

## Tài liệu design nguồn
- Basic design (基本設計): `docs/01-basic-design/<feature>/...`
- Detail design (詳細設計): `docs/02-detail-design/<feature>/...`
- Figma (link + node): `docs/03-ui/<feature>/figma-links.md`

## Owner / Assignee
<!-- Ai sở hữu feature này END-TO-END (1 người, xem TEAM-WORKFLOW mục 3). Gán luôn GitHub Assignee
     cho đúng người này để cả team biết ai đang cầm feature. -->

## Blocked by
<!-- Liệt kê issue phải xong TRƯỚC (VD "#12 auth module"). Để trống nếu độc lập.
     Giúp xếp thứ tự merge, giảm rebase/conflict chồng chéo. -->

## Affected domains / shared surface
<!-- Feature này chạm domain/module nào? Có đụng vùng DÙNG CHUNG không?
     (src/shared/, config, router, docs/00-glossary.md, constitution…)
     Càng nhiều feature cùng chạm 1 surface → càng dễ conflict/drift (TEAM-WORKFLOW mục 4–5).
     VD: "domain 予約; append glossary; KHÔNG đụng src/shared". -->

## Tóm tắt acceptance
<!-- Chức năng làm gì, cho ai, tiêu chí "xong" chính -->

## Ghi chú
<!-- Ràng buộc từ khách? Mâu thuẫn đã thấy trong tài liệu? -->
