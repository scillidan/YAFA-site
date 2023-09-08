---
tags: [reference#, used#]
---

## Used

```
[sublime_text.exe]
str=___
key=^d
clr={del}
```

这里需要先检查`key`键位是否被占用，在Sublime Text里按`Ctrl + D`测试。

如命令:

```sh
webify --no-eot --no-svg %1.ttf && cat %1.ttf | ttf2woff2 > %1.woff2
```

可以写成

```sh
webify --no-eot --no-svg ^|___.ttf && cat ^|___.ttf | ttf2woff2 > ^|___.woff2
```

## Reference

- [Interactive bundle text](https://lintalist.github.io/#InteractiveBundleText)
- [Lintalist MultiCaret support](https://github.com/lintalist/lintalist/blob/master/docs/MultiCaret.md)