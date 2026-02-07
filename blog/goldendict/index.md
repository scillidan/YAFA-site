[GoldenDict](http://goldendict.org/)是一款开源的桌面端词典检索应用，跨平台，[支持多种词典格式](https://github.com/goldendict/goldendict/wiki/Supported-Dictionary-Formats)。它有十分丰富的可配置项，包括字典组，拼写检查、单词发音、语音合成、在线翻译等等。[GoldenDict-ng](https://github.com/xiaoyifang/goldendict-ng)是基于前者重写的新一代GoldenDict，修复了[一些长期累积的问题](https://github.com/xiaoyifang/goldendict-ng/issues/587)，目前仍然处于积极开发中，更多地说明见[GoldenDict-ng文档](https://xiaoyifang.github.io/goldendict-ng/)。

二者操作相近，配置文件几乎可以共用（但不推荐）。GoldenDict-ng采用了更新的技术，而GoldenDict向前兼容了一些像Windows XP之类的老旧系统。这里就主要以配置GoldenDict为例。软件版本为[GoldenDict 1.5.1](https://github.com/goldendict/goldendict/releases/tag/1.5.1)、[GoldenDict-ng-v24.05.05](https://github.com/xiaoyifang/goldendict-ng/releases/tag/v24.05.05-LiXia.ecd1138c)。

## 配置文件的位置

同于大多数的软件的安装版，GoldenDict的配置文件也位于`C:\Users\<user>\AppData\Roaming`的`<软件名>`。如果要保存GoldenDict的配置，可备份此处的`config`文件。

如果使用[Scoop](https://scoop.sh/)安装，配置文件默认位于`C:\Users\<user>\scoop\apps\goldendict\current\portable`。我个人的配置文件和其他文件都自行分开存放，再使用环境变量和[Link Shell Extension](https://schinagl.priv.at/nt/hardlinkshellext/linkshellextension.html)等完成软件配置。所以我没有去处理某些相关的问题，如[issue](https://github.com/goldendict/goldendict/issues/1560)。

文章分为「基础篇」和「番外篇」。「番外篇」更多地涉及到「程序」这一功能，有更多的实验性和Bug，如文本翻译等。也并不一定与GoldenDict有很强的相关，如语法检查等。

## 窗口布局

主窗口的布局可在菜单栏的「查看」中设置。我个人只保留了「查询面板」。

窗口的大小可以拖动窗口边框调整。对于调整弹窗窗口，参考该[issue](https://github.com/goldendict/goldendict/issues/1010)才操作。

## 显示风格

编辑 → 首选项 → 显示风格 → Modern。

对于高级用户，可以在搜索后按F12调出开发者工具，使用搜索功能直接在GoldenDict主窗口，查找CSS元素，编辑它的属性并实时预览效果。调整完成后，在GoldenDict配置文件目录下新建的CSS文件`article-style.css`，将修改编辑进去即可。例如：

```css
/* 使用本地字体，以Sarasa Term SC Nerd字体为例 */
@font-face {
  font-family: "Sarasa Term SC Nerd";
  src:
    local('Sarasa Term SC Nerd') format('truetype');
  font-weight: normal;
  font-style: normal;
}

body {
  font-family: "Sarasa Term SC Nerd", NotoSans;
  font-zise: 9pt;
}

.gdarticle {
  border-radius: 0;
  background: #fffff8;
}

.gdactivearticle .gddictname {
  font-size: .8em;
  background: #fffff8;
}

.gddictname {
  font-size: .8em;
  border-radius: 0;
}

span.gddicticon {
  display: none;
}

.dsl_m1 {
  padding-left: 0;
}

.programs_plaintext, .programs_html {
  margin-top: 1.8em;
}

.programs_play {
  margin-top: 1.8em;
  margin-left: 0;
}

.voiceengines_play {
  margin-top: 1.85em;
  margin-left: 0;
}
```

重启GoldenDict。

注意，字体不会覆盖有内置样式的词典（主要是MDX格式），或者[ZIM](https://wiki.openzim.org/wiki/ZIM_file_format)档案。

## 黑暗主题

1. `git clone https://github.com/yozhic/GoldenDict-Full-Dark-Theme`
1. 参考[Installation](https://github.com/yozhic/GoldenDict-Full-Dark-Theme#installation)段落进行安装
1. 编辑 → 首选项 → 附加样式 → Dark
1. 同样，可以修改样式，参考上文，编辑配置文件目录下`styles\Dark\article-style.css`

```css
@font-face {
  font-family: "Sarasa Term SC Nerd";
  src:
    local('Sarasa Term SC Nerd') format('truetype');
  font-weight: normal;
  font-style: normal;
}

body {
  font-family: "Sarasa Term SC Nerd", NotoSans;
  font-size: 9pt;
}

.gdarticle {
  margin-top: 0;
  margin-bottom: 0.2em;
}

.gddictname {
  font-size: .8em;
  border-radius: 0;
  border: none;
}

.gdarticle .gddictname {
  border-radius: 0;
}

.gdactivearticle .gddictname {
  font-size: .8em;
  margin-bottom: 0;
  border: none;
}

span.gddicticon {
  display: none;
}

.dsl_m1 {
  padding-left: 0;
}

.programs_plaintext, .programs_html {
  margin-top: 1.8em;
}

.programs_play, .voiceengines_play {
  margin-top: 1.8em;
  margin-left: 0;
}
```

## 添加词典

GoldenDict支持StarDict、Babylon、Lingvo、Dictd等多种格式。需注意，`.dict`格式和[StarDict](https://github.com/huzheng001/stardict-3)词典应用都是开源。而`.mdx`属于专有软件[MDict](https://www.mdict.cn/wp/?lang=en)的词典格式，更多信息见[fileformat](https://github.com/zhansliu/writemdict/blob/master/fileformat.md)。

我目前使用的（附StarDict文件）：

- [FreeDict](https://freedict.org/) & [WikDict](https://www.wikdict.com/) （[share_freedict](https://github.com/scillidan/share_freedict)）\
  由[社区](https://freedict.org/community/)驱动的双语字典，支持超过45种语言。人工编纂的项目公开在[Github](https://github.com/freedict/fd-dictionaries)上，其他的字典则是通过[工具](https://github.com/freedict/tools)导入，它提供`eng-zho`就实际上来自于WikDict。我额外下载了所有的`*-eng`字典，由英语作中继，来查询陌生语种。
- [CC-CEDICT](https://www.mdbg.net/chinese/dictionary?page=cedict) （[share_cc-cedict](https://github.com/scillidan/share_cc-cedict)）\
  英汉词典，且能进行部分汉英查询。适合快速查询。不含详细释义、语法、例句。
- [GCIDE](https://gcide.gnu.org.ua/)（GNU版国际协作英语词典） （[share_gcide](https://github.com/scillidan/share_gcide)）\
  英英定义词典，提供详细释义、词源信息、文学例句。适合了解词义概念，尤其是专业词汇等。可能缺少现代、当代的词汇、释义、日常用法，或者已经过时、弃用。
- [Online Etymology Dictionary](http://etymonline.com/) （[share_etymonline](https://github.com/scillidan/share_etymonline)）\
  英语词源词典。
- [WordNet](https://wordnet.princeton.edu/download) （[share_wordnet](https://github.com/scillidan/share_wordnet)）\
  非传统形式词典，实际上是英语词汇数据库、词汇网络。核心为同义词集，也含“反义词”等关系集。适合用于扩展词汇、理解。
- [汉语大词典](https://www.hanyudacidian.cn) （[share_hanyudacidian](https://github.com/scillidan/share_hanyudacidian)）\
  “大型的、历史性的汉语语文辞典”，详细见[《前言》](https://www.hanyudacidian.cn/about/preface)。

不推荐的：

- [KDr2.com](https://kdr2.com/resource/stardict.html)上的`汉语大词典 离线版`（StarDict）
- [Mdict.org上的汉语词典](https://mdx.mdict.org/%E6%8C%89%E8%AF%8D%E5%85%B8%E8%AF%AD%E7%A7%8D%E6%9D%A5%E5%88%86%E7%B1%BB/%E6%B1%89%E8%AF%AD/%E5%AD%97%E5%85%B8/)上的`汉语大词典(简体精排).mdx`、`漢語大詞典.mdx`

主要理由是我浏览了几个帖子：[汉语大词典从没经过转码的文本（自动转码谬种流传）](https://forum.freemdict.com/t/topic/3840/28)、[【结项】汉语大词典2.0源数据](https://forum.freemdict.com/t/topic/15998)等几个帖子。具体理由也还有别的，举一个来说，`word`和`meaning`这两个词，来自字典格式转换工具[pyglossary](https://github.com/ilius/pyglossary)定义的数据库格式字典文件，里面的数据表的关键字。对于这本字典，如果要进行繁简转换，对于meaning（释义）部分，需要排除其中的引用、专用词句部分，对于word（词条）部分，可能需要有一个替换表，等等。

繁简字相关的问题应考虑使用GoldenDict内置的繁简转写工具。

1. 词典 → 词典来源 → 转写 → 中文转换 → 勾选`简体转繁体`、`繁体转简体`
1. 词典 → 群组 → 将`转写工具`放在对应`字典`的前面。例如，《汉语大词典》包含了繁体（原文）内容，可将`简体转繁体`放在字典前：

其他的：

- [ECDICT/简明英汉字典](https://github.com/skywind3000/ECDICT) （[share_ecdict](https://github.com/scillidan/share_ecdict)\[[Ultimate版本](https://github.com/skywind3000/ECDICT-ultimate)\]）\
  汇编的收录广泛的英汉双解词典数据库，含音标、词频和考试大纲标注。我目前主要使用FreeDict、CC-CEDICT，和英英字典，所以移到其他类里。
- [Latin dictionaries](https://latin-dict.github.io/)\
  拉丁语字典和一些希腊语字典。
- [proteusx](https://github.com/proteusx)\
  古希腊语词典。

编辑 → 词典 → 词典来源 → 文件 → 添加 → 字典所在文件夹 → 递归搜索 (On) → 应用

## 添加词典群组

1. 词典 → 群组 → 添加群组 → `<group>` → 将词典拖入群组 → 应用
1. GoldenDict的主界面 → 查找于 → `<group>`

你可以在群组内拖动字典来调整顺序，如：

```text
拼写词库
单词音频库
英汉词典
英英词典
英汉汉英词典
...
```

## 添加拼写词库

通常称为「Hunspell Dictionary」，包含了所需的`.aff`、`.dic`文件。GoldenDict已内置了部分拼写词库，这里勾选所需即可：

编辑 → 词典 → 词典来源 → 构词法规则库

如果你添加了较新版本的词库，例如[English Spell Checker Dictionaries](http://wordlist.aspell.net/dicts/)上的「en_US」、[LibreOffice Hunspell dictionaries](https://github.com/LibreOffice/dictionaries)里的`en\en_US.*`：

构词法规则库 → 变更 → `.aff`、`.dic`文件所在目录 → 勾选库中条目 → 应用

## 添加单词音频库

[pronunciations](https://github.com/yousefvand/pronunciations)是一个英语语音资料仓库，但在`v1`分支里，它似乎已经清理了旧分支里的约2GB的音频文件，而使用`generate.sh`脚本来创建。这里假设你有了单词的音频文件：

词典 → 词典来源 → 音频文件目录 → 添加 → `pronunciations/`所在 → 应用

这里，如果使用的是便携版GoldenDict，可以将所有音频文件如：

```text
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
1. 某个成员提到了GoldenDict的「音频文件目录」功能：不方便也不可靠，建议使用`.dsl`文件。类似目录文件，指出音频文件的位置
1. 从[MEGA分享链接](https://mega.nz/folder/3CwCmR7D#05Q08F-cTKPiOTdjKXAcuw)下载新版本的`.dsl`文件，下载后解压`ForvoDSL-20220513.7z`到同名文件夹
1. `ForvoDSL-20220513`里的`.dsl`文件，需要同`Forvo_pronunciations/export/opus`里的`.zip`，放入同一个文件夹，例如`Forvo_pron/`。这里，也可以使用`mklink`创建系统链接完成，如`mklink <path_to>\Forvo_pron\ForvoEnglish.dsl.files.zip <path_to>\en.zip`
1. 修改`.zip`文件的文件名，一一对应。例如将`en.zip`重命名为`ForvoEnglish.dsl.files.zip`，对应`ForvoEnglish.dsl`
1. 词典 → 词典来源 → 文件 → 添加 → `Forvo_pron/`所在 → 应用

因为这些压缩档案包含了大量的小文件，移动、复制、解压上都非常慢。所以这里参考`ForvoDSL-20220513/00README.txt`的内容，使用Windows批处理脚本[mklink_forvo_pron.bat](https://gist.github.com/scillidan/fd36bd6c9a84b565c131cccc4508df7b)（或者Bash脚本[mklink_forvo_pron.sh](https://gist.github.com/scillidan/1b7c6cd69611320660ca4c2279dead78)）进行批量操作。运行前，需编辑脚本`4-8`行，参考我的目录设置，填写你的实际的目录位置。

在Windows上运行时：

命令提示符 → `<path_to>\mklink_forvo_pron.bat` → `Enter`

最后：

词典 → 词典来源 → 文件 → 添加 → `Forvo_pron/`所在 → 应用

## 添加语音引擎

在Windows 10上：

1. 设置 → 时间和语言 → 语音 → 管理语音 → 添加语音
1. 词典 → 词典来源 → 语音合成 → 预览 → 可用语音引擎 → `Microsoft David Desktop - English (United States)` → 添加 → 应用

效果较差。推荐使用[edge-tts](https://scillidan.github.io/YAFA-site/blog/goldendict/github.com/rany2/edge-tts)，见[番外篇](https://zhuanlan.zhihu.com/p/676424891)。

## 添加离线维基

[Kiwix](https://kiwix.org/en/)是一个跨平台的离线的维基阅读器，也可自部署服务端。可阅读的内容不限Wiki，也适用于如[StackExchange](https://stackexchange.com/)([Stack Overflow](https://stackoverflow.com/))、[Project Gutenberg](https://www.gutenberg.org/)等等，来提供高速、稳定的档案访问服务。它支持高度压缩的`.zim`格式，该格式可包含元数据、HTML、图像等资源。配置步骤：

1. 从[ZIM Ebook Library](https://library.kiwix.org/)选择档案，例如「wikipedia (English)」，当鼠标移到档案上时，或者点击档案在线阅读时，在浏览的左下角或者地址栏会提示档案全名。「all maxi」这个型号包含了除音频、视频等大型媒体文件外的所有内容。关于各型号的详细说明，见[issue1](https://github.com/openzim/zim-requests/issues/129)、[issue2](https://github.com/openzim/mwoffliner/issues/485)
1. 点击`Download - xx GB`。对于大型档案，选择Torrent file，之后在BT下载器中打开该文件并下载
1. GoldenDict → 编辑 → 词典 → 词典来源 → 文件 → 添加 → `.zim`所在目录
1. 等待GoldenDict创建索引

对于大型档案，无论是搜索条目，还是「全文搜索」，其速度称得上「很快」。例如，如果检索「古腾堡计划」的电子书库中提到某个词的段落：

GoldenDict → 搜索 → 全文搜索 → `<word>` → 搜索 → 单击条目

不过我目前使用的是[Kiwix Server](https://kiwix.org/en/applications/)，在浏览器里预览ZIM档案，见[kiwix-server.md](https://scillidan-cheat.vercel.app/?search=kiwix-server)。

## 设置鼠标取词

[GoldenDictOCR](https://github.com/VimWei/GoldenDictOCR)是一个[AutoHotKey](https://www.autohotkey.com/)脚本，在它的「鼠标选择取词」模式下，可通过「双击选词、划词」来查词。需要配合GoldenDict的默认查词快捷键`Ctrl+c+c`来使用。

```sh
git clone https://github.com/VimWei/GoldenDictOCR
cd GoldenDictOCR
```

编辑`IncludeAHK/GdOcrTool.ahk`，填写`GoldenDict.exe`所在的正确位置:

```text
Global GoldenDictFileName := "<path_to>\GoldenDict.exe"
```

运行`GoldenDictOCR.ahk`后，按`Alt+i`即可开关「鼠标选择取词」。

如果需要修改键位，例如：

1. 编辑 → 首选项 → 热键 → 使用下列热键翻译剪切板中的单词 → `Alt+z`
1. 编辑`GoldenDict.ahk`，修改两处：
   - 118行附近，此处的`Send ^{c 2}`，即`Ctrl+c+c`，改为`Send !z`，即`Alt+z`
   - 37行附近，此处的`!i::`，即`Alt+i`，改为`^!g::`，即`Ctrl+Alt+g`

这个脚本也能和[Text Grab](https://github.com/TheJoeFin/Text-Grab)、[YomiNinja](https://github.com/matt-m-o/YomiNinja)等工具很好的配合。

## 一些实用键位

所有的快捷键在：

帮助 → GoldenDict帮助 → `Shortcuts`

常用的有：

- `Ctrl+l` 聚焦到输入框
- `Alt+Left/Right` 前个/后个查询记录

2026-02-072026-02-07
