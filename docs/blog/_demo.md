---
title: DEMO
date:
  created: 2024-01-01
  updated: 2024-01-01
keywords: MkDocs, Plugin, Test, Demo
locale: en
---

↪ https://squidfunk.github.io/mkdocs-material/reference/code-blocks/#embedding-external-files

↪ https://github.com/jfcmontmorency/mkdocs-audio

![type:audio](../../assets/audio/melancholy.ogg)

↪ 

```asciinema
"assets/asciinema/setup.cast"
{
  "title": "",
  "theme": "nord",
  "cols": 120,
  "rows": 20,
  "terminal_font_size": "small",
  "terminal_font_family": "Sarasa Mono SC",
  "terminal_line_height": "1.3",
  "pause_on_markers": true
}
```

↪ https://github.com/scillidan/mkdocs-asciinema-player

```asciinema-player
{
    "file": "assets/asciinema/setup.cast",
    "title": "",
    "mkap_theme": "no-border",
    "theme": "nord",
    "cols": 120,
    "rows": 20,
    "terminal_font_size": "small",
    "terminal_font_family": "Sarasa Mono SC",
    "terminal_line_height": "1.3",
    "pause_on_markers": true
}
```

↪ https://github.com/mondeja/mkdocs-include-markdown-plugin

```
# {%
#     include "C:/Users/User/Github/My_Draft/linux_arch.md"
# %}
```