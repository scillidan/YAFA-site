---
tags: [cmd#]
---

## CMD

```
curl -k https://raw.githubusercontent.com/scillidan/WALLPAP-ENG-resource/main/table.md ^
  | sd "\[\d{10}\]\(" "" ^
  | sd "(\)\|\S+subsc)" "|![](//img.shields.io/steam/subsc" ^
  | mdtable2csv ^
  | sd "//steamc" "https://steamc" ^
  | xsv select source,version,urlid ^
  | csview
```