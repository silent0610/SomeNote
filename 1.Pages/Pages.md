---
Type:
  - Query
aliases: 
Status:
tags: 
modifiedDate: 星期二, 五月 27日 2025, 6:37:57 晚上
---

```dataview
table
Type as Type,
tag as "tag",
Status as "Status"
from "1.Pages"
WHERE contains(Type, "Page")
sort file.mtime desc
```
