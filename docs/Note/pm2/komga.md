---
tags: [tool#, cmd#]
---

## Tool

- [Directory Opus](https://www.gpsoft.com.au/)
- [Xnconvert](https://www.xnview.com/en/xnconvert/)
- [FastStone](https://www.faststone.org/FSViewerDetail.htm)
- [Advanced Renamer](https://www.advancedrenamer.com)
- [To CBZ](https://github.com/italomaia/to-cbz)
- [Komga Cover Extractor](https://github.com/zachstultz/komga-cover-extractor)

## CMD

```sh
for /d %%X in (*) do arenc.exe -e ".../v01.aren" -t folders -p . -m rename ^
	&& for /d %%X in (*) do arenc.exe -e ".../001.aren" -t files -p %%X -m rename ^
	&& for /d %%X in (*) do cp %%X/001.jpg %%X/cover.jpg ^
	&& for /d %%X in (*) do py39 to-cbz/to_cbz.py %%X ^
	&& py39 D:/binp/komga-cover-extractor/komga_cover_extractor.py -c "True" -cq "70" -p .
``