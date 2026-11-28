---
tag: markdown-tables
author: jay-flowers
category: gotcha
created_at: 2026-11-27T00:13:57Z
identity: markdown-tables-20261127T001357-jay-flowers
tier: draft
as_of: "website@48e37b99af109e32646797fda0eb3eb7e01ed81c"
---

In fully space-padded Markdown tables (like the-divisor.md's ownership boundaries table), a new row whose cell content exceeds the established column width by even one character shifts that row's pipes out of alignment — pad new rows to the existing column width or widen the whole table uniformly.
