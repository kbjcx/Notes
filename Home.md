```dataview
TABLE without id G as tag, rows.file.link as files
FROM "Diary"
FLATTEN file.etags as T
GROUP BY T as G
```