---
title: "tables used"
---

SQLite `get_tables_used` is very slow. We need to calculate this statically or cache all prepared versions of `tables_used` and the result(s).