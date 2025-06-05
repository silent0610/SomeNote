---
Type:
  - Query
aliases: 
tags: 
Status:
modifiedDate: 星期三, 五月 28日 2025, 9:58:49 晚上
---

## Projects

```dataview
table
Type as Type,
tag as "tag",
Status as "Status"
from "1.Pages"
WHERE contains(Type, "Project")
sort  Status Desc
```
