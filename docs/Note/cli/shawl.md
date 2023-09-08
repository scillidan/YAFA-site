---
tags: [reference#, cmd#]
---

## CMD

```sh
gsudo shawl add --name shawl-%1 -- %2 ^
	&& gsudo sc config shawl-%1 start= auto ^
	&& gsudo sc start shawl-%1
rem && gsudo sc create %1 binPath= "shawl.exe run -- %2"
```