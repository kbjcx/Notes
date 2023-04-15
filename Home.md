```dataviewjs
let pages = dv.pages("#TODO");
dv.table(
    ["Name", "Area"],
    pages.sort(b => b.file.mtime, "desc").map(b => [b.file.link, b.area])
)
```

```dataviewjs
let pages = dv.pages("#Fleeting");
dv.table(
    ["Name"],
    pages.sort(b => b.file.mtime, "desc").map(b => [b.file.link])
)
```