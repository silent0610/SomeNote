---
Type: Query
aliases: 
tags: 
Status:
modifiedDate: 2025/06/09, 10:50
---

# Notes

## Notes

```dataview
table
Type as Type,
tag as "tag",
Status as "Status"
from "1.Pages"
WHERE contains(Type, "Note")
sort  Status Desc
```

```dataview
TABLE WITHOUT ID Type, length(rows.file.link) AS "Count" FROM "1.Pages" WHERE Type GROUP BY Type SORT Type ASC
```
