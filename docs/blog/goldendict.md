---
title: "配置GoldenDict"
created: 2023-12-20
modified: 2024-09-20
locale: zh
keywords: >
    GoldenDict, GoldenDict-ng, Setting, Dictionaries, Pronunciation, Morphology, Programs
---

[GoldenDict](http://goldendict.org/)是一款开源的桌面端词典检索应用，跨平台，[支持多种词典格式](https://github.com/goldendict/goldendict/wiki/Supported-Dictionary-Formats)。它有十分丰富的可配置项，包括字典组，拼写检查、单词发音、语音合成、在线翻译等等。[GoldenDict-ng](https://github.com/xiaoyifang/goldendict-ng)是基于前者重写的新一代GoldenDict，修复了[一些长期累积的问题](https://github.com/xiaoyifang/goldendict-ng/issues/587)，目前仍然处于积极开发中，更多地说明见[GoldenDict-ng文档](https://xiaoyifang.github.io/goldendict-ng/)。

二者操作相近，配置文件几乎可以共用（但不推荐）。GoldenDict-ng采用了更新的技术，而GoldenDict向前兼容了一些像Windows XP之类的老旧系统。这里就主要以配置GoldenDict为例。软件版本为[GoldenDict 1.5.0](https://github.com/goldendict/goldendict/releases/tag/1.5.0)、[GoldenDict-ng-v24.05.05](https://github.com/xiaoyifang/goldendict-ng/releases/tag/v24.05.05-LiXia.ecd1138c)。

## 配置文件的位置

同于大多数的软件的安装版，GoldenDict的配置文件也位于`C:\Users\<username>\AppData\Roaming`的`<软件名>`。如果要保存GoldenDict的配置，可备份此处的`config`文件。

如果使用[Scoop](https://scoop.sh/)安装，需要删除`C:\Users\<username>\scoop\apps\goldendict\current\portable`目录，也就是断开了和Scoop配置目录里的系统目录链接，位于`C:\Users\<username>\scoop\persist\goldendict`。见[issue](https://github.com/goldendict/goldendict/issues/1560)。

文章分为「基础篇」和「番外篇」。基础篇主要涉及「黑暗主题」、各类的「词典」、「取词方式」等。「番外篇」更多地涉及了「程序」这一功能，具有更多的实验性和Bug，也提及了一些和语言相关的第三方工具，如[LanguageTool](https://languagetool.org/)等。

## 窗口布局

主窗口的布局可在菜单栏的「查看」中设置。

窗口大小可手动调整。对于弹窗窗口，参考该[issue](https://github.com/goldendict/goldendict/issues/1010)。

我个人在主窗口只保留了「查询面板」，窗口尺寸约为屏幕高的`3/4`，宽的`1/3`，弹窗的尺寸约为屏幕高的`1/2，宽的1/4`。

## 黑暗主题

```sh
git clone https://github.com/yozhic/GoldenDict-Full-Dark-Theme
```

参考[Installation](https://github.com/yozhic/GoldenDict-Full-Dark-Theme#installation)段落安装样式。

原样式有几处圆角边框，我额外进行了一些调整。可以编辑`styles\Dark\article-style.css`：

```css
// 词典框的圆角改为直角等
.gdarticle
{
  border-radius: 0;
}

.programs_plaintext, .programs_html {
  margin-top: 20px;
}

.gdactivearticle .gddictname {
  border: none;
}

.gddictname {
  border: none;
  border-radius: 0;
}

// 处理「语音合成」词典的文字的显示问题
.voiceengines_play {
  margin-top: 22.5px;
}
```

编辑 → 首选项 → 附加样式 → Dark

![](dark-theme.png)

## 修改字典字体

首先去下载并安装收录全、完成度高的字体，例如[思源黑体](https://github.com/adobe-fonts/source-han-sans/)、[霞鹜文楷](https://github.com/lxgw/LxgwWenKai)等。

还是编辑`article-style.css`，在开头添加：

```css
// 添加本地字体
@font-face {
  font-family: <FontName>;
  src: local('<Font Name>') url('file:C:\\Users\\<username>\\AppData\\Local\\Microsoft\\Windows\\Fonts\\<FontName>.ttf') format('truetype');
  font-weight: normal;
  font-style:  normal;
}
```

在末尾添加：

```css
body
{
  font-family: "<Font Name>", NotoSans;
  font-size:   9.5pt;
}

.dsl_t
{
  font-family: "<Font Name>", NotoSerif;
}
```

字体不会生效于有内置样式的词典，或者[ZIM](https://wiki.openzim.org/wiki/ZIM_file_format)档案。

为了更好地显示脚本，我个人目前使用的是等宽字体[Sarasa Term SC Nerd 字体](https://github.com/laishulu/Sarasa-Term-SC-Nerd)。

## 修改字典内置样式

例如编辑[ODE 3/e](https://mdict.org/post/0011/)里的`ODE.css`：

``` css
// 让字更小
.Od3 {
    font-size: 102%;
    line-height: 97%;
}
```

## 添加词典

下载词典文件，例如：

- [Mdict.org](https://mdict.org/)上的[新牛津英汉双解词典](https://mdict.org/post/0011/)、[企鹅英语词典](https://mdict.org/post/penguin-english-dictionary-3rd/)、[柯林斯COBUILD英语用法大全](https://mdict.org/post/collins-cobuild-english-usage/)、[韦氏发音词典](https://mdict.org/post/0010/)等
- Mdict.org上的[汉语词典](https://mdx.mdict.org/%E6%8C%89%E8%AF%8D%E5%85%B8%E8%AF%AD%E7%A7%8D%E6%9D%A5%E5%88%86%E7%B1%BB/%E6%B1%89%E8%AF%AD/%E5%AD%97%E5%85%B8/)，推荐`汉语大词典(简体精排).mdx`、`漢語大詞典.mdx`
- StarDict格式的[汉语大词典(繁)](https://kdr2.com/resource/stardict.html)、[汉语大词典(简)](https://github.com/scillidan/file_StarDict)
- [简明英汉字典增强版](https://github.com/skywind3000/ECDICT)
- [21世纪英汉汉英双向词典](https://stardict.uber.space/zh_CN/index.html)
- [WikDict](https://www.wikdict.com/)上的[双语词典](https://download.wikdict.com/dictionaries/stardict/)
- [FireDict](https://tuxor1337.frama.io/firedict/dictionaries.html)上的自由的离线StarDict词典，如词源词典（[Douglas Harper's Online Etymology Dictionary](https://www.etymonline.com/)），GNU版国际协作英语词典（GNU Collaborative International Dictionary of English）
- [eBook Reader Dictionaries](https://github.com/BoboTiG/ebook-reader-dict)上的基于维基百科的[英英词典](https://www.reader-dict.com/en/download/en)
- 工具书，如[翻译人名](https://github.com/lxs602/Chinese-Mandarin-Dictionaries/tree/main/Chinese%20Names%20Corpus/English-Chinese%20Names)等等
- 其他的，[Latin dictionaries](https://latin-dict.github.io/)上的拉丁语词典、[proteusx](https://github.com/proteusx)制作的古希腊语词典、[佛學術語字辭典](https://glossaries.dila.edu.tw)等等

编辑 → 词典 → 词典来源 → 文件 → 添加 → ... → 递归搜索 (On) → 应用。

需要注意，`.dict`格式和[StarDict](https://github.com/huzheng001/stardict-3)词典应用都是开源。而`.mdx`属于专有软件[MDict](https://www.mdict.cn/wp/?lang=en)的词典格式，更多信息见[fileformat](https://github.com/zhansliu/writemdict/blob/master/fileformat.md)。

## 添加词典群组

1. 词典 → 群组 → 添加群组 → `<group>` → 将词典拖入群组 → 应用
2. GoldenDict的主界面 → 查找于 → `<group>`

我目前常用的`default`分组中，除开翻译脚本和语音、拼写检查等功能词典，大部分都是StarDict格式：

```
在线翻译脚本
单词发音
简明英汉字典
拼写词库
专业词汇词典
汉语大词典
```

## 添加拼写词库

通常称为「Hunspell Dictionary」，包含了所需的`.aff`、`.dic`文件。GoldenDict已内置了部分拼写词库，在这里勾选所需即可：

编辑 → 词典 → 词典来源 → 构词法规则库

如果你添加了较新版本的词库，例如[English Spell Checker Dictionaries](http://wordlist.aspell.net/dicts/)上的「en_US」、[LibreOffice Hunspell dictionaries](https://github.com/LibreOffice/dictionaries)里的`en\en_US.*`：

构词法规则库 → 变更 → `.aff`、`.dic`文件所在目录 → 勾选库中条目 → 应用

![](spellcheck.png)

## 添加单词音频库

对于英语语音资料[pronunciations](https://github.com/yousefvand/pronunciations)，在`v1`版本里，它似乎已经清理了旧版本里的约2GB的文件，而使用`generate.sh`脚本。这里就仅留作参考：

词典 → 词典来源 → 音频文件目录 → 添加 → `pronunciations/`所在 → 应用

这里，如果使用的是便携版GoldenDict，可以将所有音频文件如：

```
a.mp3
a'.mp3
A'asia.mp3
a's.mp3
a-.mp3
...
B.mp3
b'hoy.mp3
b's.mp3
B-.mp3
B-axes.mp3
...
```

压缩到`pronunciations-en.zips`，压缩包里不要有文件夹路径，然后作为一本词典导入即可。参考[issue](https://github.com/goldendict/goldendict/issues/150)。

对于[Forvo](https://forvo.com/)的多语言资料，可使用[qBittorrent](https://github.com/c0re100/qBittorrent-Enhanced-Edition)一类的工具，从[该帖](https://rutracker.org/forum/viewtopic.php?t=6211002)下载，参考帖子内容：

1. `.opus`格式的体积更小，音质同`.mp3`也十分相近，所以下载时只勾选`Forvo_pronunciations/export/opus`。约24GB大小，下载完成后不需要解压
2. 某个成员提到了GoldenDict的「音频文件目录」功能：不方便也不可靠，建议使用`.dsl`文件。类似目录文件，指出音频文件的位置
3. 从[MEGA分享链接](https://mega.nz/folder/3CwCmR7D#05Q08F-cTKPiOTdjKXAcuw)下载新版本的`.dsl`文件，下载后解压`ForvoDSL-20220513.7z`到同名文件夹
4. `ForvoDSL-20220513`里的`.dsl`文件，需要同`Forvo_pronunciations/export/opus`里的`.zip`，放入同一个文件夹，例如`Forvo_pron/`。这里，也可以使用`mklink`创建系统链接完成，如`mklink ...\Forvo_pron\ForvoEnglish.dsl.files.zip ...\en.zip`
5. 修改`.zip`文件的文件名，一一对应。例如将`en.zip`重命名为`ForvoEnglish.dsl.files.zip`，对应`ForvoEnglish.dsl`
6. 词典 → 词典来源 → 文件 → 添加 → `Forvo_pron/`所在 → 应用

对于所有语音包，因为这些压缩档案包含了大量小文件，移动、复制、解压上都很慢。所以这里参考`ForvoDSL-20220513/00README.txt`的内容，使用Windows批处理脚本[mklink_forvo_pron.bat](https://gist.github.com/scillidan/fd36bd6c9a84b565c131cccc4508df7b)或者Bash脚本[mklink_forvo_pron.sh](https://gist.github.com/scillidan/1b7c6cd69611320660ca4c2279dead78)进行批量操作。运行前，需编辑脚本`4-8`行，参考我的目录设置，填写你的实际的目录位置。

在Windows上运行时：

命令提示符 → `mklink_forvo_pron.bat` → `Enter`

最后：

词典 → 词典来源 → 文件 → 添加 → `Forvo_pron/`所在 → 应用

![type:video](https://raw.githubusercontent.com/scillidan/YAFA-site/main/docs/assets/media/goldendict/pronunciation.mp4){ .skip-lightbox }

## 添加语音引擎

在Windows 10上：

1. 设置 → 时间和语言 → 语音 → 管理语音 → 添加语音
2. 词典 → 词典来源 → 语音合成 → 预览 → 可用语音引擎 → `Microsoft David Desktop - English (United States)` → 添加 → 应用

## 添加离线维基（仅gd-ng）

[Kiwix](https://kiwix.org/en/)是一个离线的维基阅读器，也适用别的网络内容，如[StackExchange](https://stackexchange.com/)([Stack Overflow](https://stackoverflow.com/))、[Project Gutenberg](https://www.gutenberg.org/)等等，可用来提供高速、稳定的档案访问服务。它支持高度压缩的`.zim`格式，该格式可包含元数据、HTML、图像等资源。配置步骤：

1. 在[Content in all languages](https://wiki.kiwix.org/wiki/Content_in_all_languages)的列表中选择档案，例如「wikipedia (English)」，型号「all maxi」。这个型号包含了除音频、视频等大型媒体文件外的所有内容。关于各型号的详细说明，见[issue1](https://github.com/openzim/zim-requests/issues/129)，[issue2](https://github.com/openzim/mwoffliner/issues/485)
2. 下载档案
3. GoldenDict-ng → 编辑 → 词典 → 词典来源 → 文件 → 添加 → `.zim`所在目录
4. 等待GoldenDict-ng创建索引

![](wiki.png)

对于大型档案，无论是搜索条目，还是「全文搜索」，其速度称得上「很快」。例如，如果我需要检索「古腾堡计划」的电子书库中提到某个词的段落：

GoldenDict-ng → 搜索 → 全文搜索 → `<word>` → 搜索 → 单击条目

![](full-text-search.png)

题外话：也可使用[Kiwix客户端](https://kiwix.org/en/applications/)或者[Kiwix JS for PWA](https://github.com/kiwix/kiwix-js-pwa)阅读`.zim`档案。

## 设置鼠标取词

[GoldenDictOCR](https://github.com/VimWei/GoldenDictOCR)是一个[AutoHotKey](https://www.autohotkey.com/)脚本，在它的「鼠标选择取词」模式下，可通过「双击选词、划词」来查词。需要配合GoldenDict的默认查词快捷键`Ctrl+C+C`来使用。

```sh
git clone https://github.com/VimWei/GoldenDictOCR
cd GoldenDictOCR
```

编辑`IncludeAHK/GdOcrTool.ahk`，填写`GoldenDict.exe`所在的正确位置:

```
Global GoldenDictFileName := "...\GoldenDict.exe"
```

运行`GoldenDictOCR.ahk`后，按`Alt+i`即可开关「鼠标选择取词」。

![type:video](https://raw.githubusercontent.com/scillidan/YAFA-site/main/docs/assets/media/goldendict/with-mouse.mp4){ .skip-lightbox }

如果需要修改键位，例如：

1. 编辑 → 首选项 → 热键 → 使用下列热键翻译剪切板中的单词 → `Alt+Z`
2. 编辑`GoldenDict.ahk`，修改两处：
  - 118行附近，此处的`Send ^{c 2}`，即`Ctrl+C+C`，修改为了`Send !z`，即`Alt+Z`
  - 37行附近，此处的`!i::`，即`Alt+I`，修改为了`^!g::`，即`Ctrl+Alt+G`

## 设置OCR取词

[Umi-OCR](https://github.com/hiroi-sora/Umi-OCR)是一款开源、可离线、功能灵活的OCR软件。用于一般OCR时，它可提供高速稳定、准确率较高的中英文混合识别。

在[V1版本](https://github.com/hiroi-sora/Umi-OCR/releases/tag/v1.3.5)里，可配置一组快捷键，在OCR后触发GoldenDict查词，即[「截图联动」](https://github.com/hiroi-sora/Umi-OCR/issues/166)。

对于V2版本，或者说更通用的情况是，只需勾选「复制OCR结果」，在进行OCR后，接GoldenDict查词快捷键就可以了。配置步骤：

Umi-OCR → 添加 → 截图OCR → 设置 → 识图后的操作 → 复制结果 (On)

![type:video](https://raw.githubusercontent.com/scillidan/YAFA-site/main/docs/assets/media/goldendict/umi-ocr.mp4){ .skip-lightbox }

## 一些实用键位

我目前常用的有：

- `Ctrl+L` 聚焦到输入框
- `Alt+Left/Right` 前个/后个查询记录

所有的快捷键在：

帮助 → GoldenDict帮助 → `Shortcuts`