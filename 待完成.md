# Tag
```dataview
TABLE without id G as tag, rows.file.link as files
FROM "Diary"
FLATTEN file.tags as G
GROUP BY G
```

# Archive
```dataview
TABLE without id file.link as files, date AS "create time"
FROM "Archive"
WHERE !file.inlinks
```