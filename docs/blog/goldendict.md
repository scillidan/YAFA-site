---
title: "配置GoldenDict"
date:
    created: 2023-12-11
    updated: 2023-12-20
keywords: >
    GoldenDict, GoldenDict-ng, Setting, Dictionaries, Pronunciation, Morphology, Programs
locale: en
---

!!! read ""
  
    日期：2023-12-20  
    归属：[CC-BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/deed.zh-hans)  
    备注：有些少量的细节上的校正

[GoldenDict](http://goldendict.org/)是一款开源的桌面端词典检索应用，跨平台，[支持多种词典格式](https://github.com/goldendict/goldendict/wiki/Supported-Dictionary-Formats)。它有十分丰富的可配置项，包括可离线使用的字典和字典组，拼写检查、单词发音、语音合成，和在线词典、在线翻译等。[GoldenDict-ng](https://github.com/xiaoyifang/goldendict-ng)是基于前者重写的新一代GoldenDict，修复了[一些长期累积的问题](https://github.com/xiaoyifang/goldendict-ng/issues/587)，目前仍然处于积极开发中，安装时可看[发行版块](https://github.com/xiaoyifang/goldendict-ng/releases)和[打包的版本](https://xiaoyifang.github.io/goldendict-ng/install/)。

GoldenDict向前兼容一些Windows XP之类的老旧系统，我个人也一般使用软件的稳定版，这里就以配置GoldenDict为例。软件版本为[GoldenDict 1.5.0](https://github.com/goldendict/goldendict/releases/tag/1.5.0)、[GoldenDict-ng-v23.09.29](https://github.com/xiaoyifang/goldendict-ng/releases/tag/v23.09.29-MoonCake.7f0f8778)的`6.5.2-****.zip`。二者在菜单的配置步骤上，可能有小部分差异，而配置文件几乎可以共用（不推荐）。更多的使用说明见[GoldenDict文档](https://github.com/goldendict/goldendict/wiki)和[GoldenDict-ng文档](https://xiaoyifang.github.io/goldendict-ng/)。

我用了Scoop来安装便携版，还需要手动删除`C:\Users\YourName\scoop\apps\goldendict\current\portable`目录，也就是挂掉配置文件夹`C:\Users\YourName\scoop\persist\goldendict`里的系统目录链接，而使用安装版的配置文件位置`C:\Users\YourName\AppData\Roaming\GoldenDict`，见[issue](https://github.com/goldendict/goldendict/issues/1560)。

## 黑暗主题

1. 下载[GoldenDict-Full-Dark-Theme](https://github.com/yozhic/GoldenDict-Full-Dark-Theme)，并按照说明安装（移动文件）
2. GoldenDict → 编辑 → 首选项 → 附加样式 → Dark

我额外进行了一些调整，编辑`C:\Users\YourName\AppData\Roaming\GoldenDict\styles\Dark\article-style.css`：

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

// 「语音合成」词典的文字显示上有些问题
.voiceengines_play {
  margin-top: 22.5px;
}
```

一些字典内置的样式也可以修改，如编辑[ODE 3/e](https://mdict.org/post/0011/)的`ODE.css`：

``` css
// 字更小
.Od3 {
    font-size: 102%;
    line-height: 97%;
}
```

主窗口的布局可在菜单栏的「查看」中设置，窗口大小可手动调整。对于弹窗窗口，参考该[issue](https://github.com/goldendict/goldendict/issues/1010)来操作。

![](dark-theme.png)

我个人使用的设置是：主窗口只保留「查询面板」，屏幕的全高，宽的1/3；弹窗窗口是，屏幕高的1/2，宽的1/4。
## 修改字典字体

去下载并安装收录全、完成度高的字体。

编辑`article-style.css`，在开头添加：

```css
// 添加本地字体
@font-face {
  font-family: YourFont;
  src: local('Your Font') url('file:C:\\Users\\YourName\\AppData\\Local\\Microsoft\\Windows\\Fonts\\YourFont.ttf') format('truetype');
  font-weight: normal;
  font-style:  normal;
}
```

在末尾添加：

```css
body
{
  font-family: "Your Font", NotoSans;
  font-size:   10pt;
}

.dsl_t
{
  font-family: "Your Font", NotoSerif;
}

```

我使用的词典字体是[缝合楷](https://github.com/lxgw/FusionKai)，不生效于拥有内置样式的、或`.zim`格式的词典。
## 添加词典

去下载字典文件，如：

- [Mdict.org](https://mdict.org/)上的，添加了中文释义的[新牛津英汉双解词典](https://mdict.org/post/0011/)、[企鹅英语词典](https://mdict.org/post/penguin-english-dictionary-3rd/)、[柯林斯COBUILD英语用法大全](https://mdict.org/post/collins-cobuild-english-usage/)、[韦氏发音词典](https://mdict.org/post/0010/)等等
- 还是Mdict.org上的[汉语词典](https://mdx.mdict.org/%E6%8C%89%E8%AF%8D%E5%85%B8%E8%AF%AD%E7%A7%8D%E6%9D%A5%E5%88%86%E7%B1%BB/%E6%B1%89%E8%AF%AD/%E5%AD%97%E5%85%B8/)，推荐`汉语大词典(简体精排).mdx`、`漢語大詞典.mdx`
- [简明英汉字典增强版#css-版本](https://github.com/skywind3000/ECDICT/wiki/%E7%AE%80%E6%98%8E%E8%8B%B1%E6%B1%89%E5%AD%97%E5%85%B8%E5%A2%9E%E5%BC%BA%E7%89%88#css-%E7%89%88%E6%9C%AC)
- 离线的[Online Etymology Dictionary](https://downloads.freemdict.com/%E5%B0%9A%E6%9C%AA%E6%95%B4%E7%90%86/%E5%85%B1%E4%BA%AB2020.5.11/content/1_english/z_Grammar_others/Online%20Etymology%20Dictionary/)
- 工具书，如[翻译人名](https://github.com/lxs602/Chinese-Mandarin-Dictionaries/tree/main/Chinese%20Names%20Corpus/English-Chinese%20Names)
- 别的语种，[Latin dictionaries](https://latin-dict.github.io/)上的拉丁语词典、[proteusx](https://github.com/proteusx)制作的古希腊语词典

GoldenDict → 编辑 → 词典 → 词典来源/文件 → 添加 → 词典文件所在的目录 → 递归搜索 On → 应用。

## 添加词典群组

... 词典 → 群组 → 添加群组 → `TheGroup` → 将可用词典拖入群组 → 应用。

群组中，从上往下，可参考以下顺序：

```
在线翻译
单词发音
语音引擎
词典
专业词典
例句写作
拼写检查 
其他
```

GoldenDict主界面 → 查找于 → TheGroup。

我目前使用的分组是：

```
default
  英汉汉英在线翻译
  英汉词典(无例句)
  专业词典
  拼写检查
  其他
write
  例句写作
more
  词源词典
  其他语种
  单词发音
  语音引擎
```

## 添加在线词典（可选）

参考[GoldenDict++](https://github.com/nonwill/GoldenDict-OCR)里的默认配置，可添加[DICT.org](https://dict.org/bin/Dict)作为在线词典库。

GoldenDict → 编辑 → 词典 → 词典来源/词典服务器 → 添加：

- 已启用 On
- 名称 `dict.org`
- 地址 `dict://dict.org`
- 数据库 `*`
- 策略 `*`

## 添加拼写检查词库

软件已内置了部分词库，如果需要额外添加：

1. 去下载「Hunspell Dictionary」，如[English Spell Checker Dictionaries](http://wordlist.aspell.net/dicts/)上的「en_US」。如果要添加多个拼写词典，需将所有的`.aff`、`.dic`文件放在同一目录
2. ... 词典 → 词典来源/构词法规则库 → 变更 → `.aff`、`.dic`文件所在的目录 → 勾选库中条目 → 应用
3. ... 群组 → 将拼写词典拖入词典群组

![](spellcheck.png)

## 添加单词发音

1. 下载[pronunciations](https://github.com/yousefvand/pronunciations)。仅英语，约2GB大小
2. 音频压缩包都放在同一个文件夹内， 解压到同名的文件夹
3. ... 词典 → 词典来源/音频文件目录 → 添加 → 音频文件所在文件夹 → 应用

也可以从[该帖](https://rutracker.org/forum/viewtopic.php?t=6211002)下载[Forvo](https://forvo.com/)的多语言语音，使用[qBittorrent Enhanced](https://github.com/c0re100/qBittorrent-Enhanced-Edition)一类的工具。根据帖子上的内容，这样操作：

1. `.opus`格式体积更小，音质也十分相近，所以下载时只勾选`Forvo_pronunciations/export/opus`，约24GB大小。下载完成后不用解压
2. 对话提到了「音频文件目录」这个功能不方便也不可靠，推荐使用`.dsl`文件，可以当作是目录册，指向了音频文件`.zip`
3. 补充的新版本的`.dsl`文件，这是一个[MEGA分享链接](https://mega.nz/folder/3CwCmR7D#05Q08F-cTKPiOTdjKXAcuw)。[MEGAsync](https://mega.nz/sync)是一个安全可靠的云备份、同步服务。下载后解压`ForvoDSL-20220513.7z`
4. 重命名`Forvo_pronunciations/export/opus`里的`en.zip`为`ForvoEnglish.dsl.files.zip`，然后将它和`ForvoDSL/ForvoDSL-20220513`里的`ForvoEnglish.dsl`，放入一个在别处新建的文件夹，如`gd_Forvo_pronunciations`
5. ... 词典 → 词典来源/文件 → 添加 → `...\gd_Forvo_pronunciations` → 应用

这里，如果要添加所有发音词典，考虑到不去移动`Forvo_pronunciations`，且不重命名文件。利用`ForvoDSL/ForvoDSL-20220513/00README.txt`中的信息，可以编辑成一个长命令行脚本用来批量`mklink`。

1. 从`00README.txt`制作`.cmd`脚本。或者下载[mklink_for_dslfiles.cmd](https://gist.github.com/scillidan/6d5ceab14883a2c7a890e88e9f5532a7)
2. 每一行命令格式都是`mklink ...\gd_Forvo_pronunciations\ForvoEnglish.dsl.files.zip ...\Forvo_pronunciations\export\opus\en.zip`。替换所有的`...`为正确的路径
3. 终端中运行`mklink_for_dslfiles.cmd`
4. ... 词典 → 词典来源/文件 → 添加 → `...\gd_Forvo_pronunciations` → 应用

![type:video](https://raw.githubusercontent.com/scillidan/YAFA-site/main/docs/assets/media/goldendict/pronunciation.mp4){ .skip-lightbox }

## 添加语音引擎

1. ... 词典 → 词典来源/语音合成 → 预览/可用语音引擎 → `Microsoft Huihui Desktop - Chinese (Simplified)` → 添加
2. ... 预览/可用语音引擎 → `Microsoft David Desktop - English (United States)` → 添加

此处的语音引擎来自：Windows设置 → 时间和语言 → 语音 → 语音/选择语音。

## 添加离线维基（仅gd-ng）

Kiwix是一个离线的维基阅读器，也适用于另一些网络内容，如[StackExchange](https://stackexchange.com/)([Stack Overflow](https://stackoverflow.com/))、[Project Gutenberg](https://www.gutenberg.org/)等等，用于提供高速、稳定的档案访问服务。它支持高度压缩的`.zim`格式，可包含元数据、HTML、图像等资源。

从[Content in all languages](https://wiki.kiwix.org/wiki/Content_in_all_languages)的列表中选择档案，例如「wikipedia (English)」，型号「all maxi」包含除音频、视频等大型媒体文件外的所有内容。各型号的详细说明见该[issue1](https://github.com/openzim/zim-requests/issues/129)，[issue2](https://github.com/openzim/mwoffliner/issues/485)。配置步骤：

1. 下载`.zim`档案。如果是大体积的档案，推荐每个`.zim`单独放置，如下载`wikipedia_en_all_maxi_****.zim`到`gd_wikipedia`文件夹
2. GoldenDict-ng → 编辑 → 词典 → 词典来源/文件 → 添加 → `.zim`所在的文件夹

![](wiki.png)

也推荐使用[Kiwix客户端](https://kiwix.org/en/applications/)来阅读`.zim`。或者[Kiwix JS for PWA](https://github.com/kiwix/kiwix-js-pwa)，如果要部署到本地，可参考笔记[kiwix-js-pwa.md](https://github.com/scillidan/PM2-demo/blob/main/_readme/kiwix-js-pwa.md)。

![](kiwix-js-pwa.png)

另外，这里再演示一下GoldenDict的「全文搜索」功能。例如，查找「古腾堡计划」的电子书库中提到了某个词的段落：GoldenDict-ng → 搜索 → 全文搜索 → `TheWord` → 搜索 → 单击条目。

![](full-text-search.png)

## 添加在线翻译脚本

在「词典来源/程序」一栏，可添加命令行或者脚本工具。

大部分提供API的翻译引擎，例如[DeepL](https://www.deepl.com)、[谷歌翻译](https://cloud.google.com/translate/docs)、[微软翻译](https://www.microsoft.com/zh-cn/translator/)、[腾讯翻译君](https://fanyi.qq.com/)、[有道翻译](https://fanyi.youdao.com)等，如果有了脚本，应该都可以嵌入GoldenDict，来提供在线翻译功能。考虑到网速和稳定性，我目前使用腾讯翻译君。它的文本翻译一项，[每月有500万字符免费额度](https://cloud.tencent.com/document/product/551/35017)，重度使用也完全够用。参考脚本[tencent-translate-for-goldendict](https://github.com/LexsionLee/tencent-translate-for-goldendict)的[申请翻译API](https://github.com/LexsionLee/tencent-translate-for-goldendict#%E7%94%B3%E8%AF%B7%E7%BF%BB%E8%AF%91api)说明去获得API。流程略麻烦，但从长期看，同个API还能用在[沙拉查词](https://saladict.crimx.com/)、[沉浸式翻译](https://immersivetranslate.com/)、[字幕组机翻小助手](https://github.com/1c7/Translate-Subtitle-File)等插件、软件。之后的配置步骤：

1. 安装[Python](https://www.python.org/downloads/)，我装的是`python3.9`。安装过程全部选默认，勾选「添加Python到PATH」
2. 下载修改版的[TencentTrans_en22zh.py](https://github.com/scillidan/tencent-translate-for-goldendict/blob/master/TencentTrans_en22zh.py)并填写`SecretId`和`SecretKey`
3. `pip install tencentcloud-sdk-python`
4. ... 词典 → 词典来源/程序 → 添加：
  - 已启用 On
  - 类型 纯文本
  - 名称 `TencentTrans-en22zh`
  - 命令行 `python .../TencentTrans-en22zh.py %GDWORD%`

![](tencenttrans-en22zh.png)

不足之处有：

- 目前只支持中英文互译。有其他语言、特殊字符时可能会报错
- 输出的文本没有段落，因为[GoldenDict会删除换行符](https://github.com/goldendict/goldendict/issues/1606)

特别一提，对于在线翻译功能，[GoldenDict tools](https://github.com/Ajatt-Tools/gd-tools)的[gd-mecab](https://github.com/Ajatt-Tools/gd-tools#gd-mecab)、[gd-marisa](https://github.com/Ajatt-Tools/gd-tools#gd-marisa)脚本演示了一种梦幻般的辅助功能，可置顶句子、分词、动态断句。但不支持Windows（我没有安装成功）。

## 添加其他脚本

[ety-python](https://github.com/jmsv/ety-python)是一个python工具，能够打印词源的各历史时期的树状信息图，数据来自[Etymological Wordnet](http://www1.icsi.berkeley.edu/~demelo/etymwn/)项目，大部分信息是从[Wiktionary](https://en.wiktionary.org/wiki/Wiktionary:Main_Page)中挖掘来。安装并运行：

```sh
pip install ety
```

运行：

```sh
ety -r -t apple
```

将输出：

```sh
apple (English)
└── appel (Middle English (1100-1500))
    └── æppel (Old English (ca. 450-1100))
```

不过要在Windows平台的GoldenDict里使用时，有些我不懂的问题（见[issue](https://github.com/goldendict/goldendict/issues/1678), [blog](https://isaacong.me/posts/unicodeencodeerror-when-redirecting-python-output/)）。可以让GoldenDict使用另外一个在隔离环境里的有修改的`ety-python`包，这样：

```sh
git clone https://github.com/jmsv/ety-python
cd ety-python
python -m venv venv
venv/Scripts/activate.bat
pip install -e .
```

修改`ety-python/ety/cli.py`。在开头的`import ety`后，加上一行：

```py
import sys
```

注释末尾的`print(output.strip())`，加上：

```py
encoded_output = output.strip().encode("utf-16", errors="replace")
sys.stdout.buffer.write(encoded_output)
sys.stdout.buffer.flush()
```

词典 → 词典来源/程序 → 添加：
  - 已启用 On
  - 类型 纯文本
  - 名称 `ety-python`
  - 命令行 `...\ety-python\venv\Scripts\python.exe ...\ety-python\ety\__main__.py -r -t %GDWORD%`

这样做没有优雅地解决问题，依旧在输入一些字符如空格时，显示乱码，但对我来说能够使用了。

另外，还可以添加其他脚本如：拼写提示[DoYouMean](https://github.com/hisbaan/didyoumean)、同义词词典[thes](https://github.com/grantshandy/thes)、缩写查询[abbr-cli](https://github.com/mhadidg/abbr-cli)等。

![](scripts.png)

## 设置鼠标取词

用到了[AutoHotKey](https://www.autohotkey.com/)脚本[GoldenDictOCR](https://github.com/VimWei/GoldenDictOCR)。在它的「鼠标选择取词」模式，可以双击选词、划词来查词。GoldenDict默认的查词快捷键是`Ctrl+C+C`，如果已被占用，可修改为其他键。脚本中的键位也就需要一并修改。例如我用的是`Alt+Z`：

GoldenDict → 编辑 → 首选项 → 热键 → 使用下列热键翻译剪切板中的单词 → `Alt+Z`。

`clone`或下载[GoldenDictOCR](https://github.com/VimWei/GoldenDictOCR)，进入文件夹。

编辑`IncludeAHK/GdOcrTool.ahk`，填写`GoldenDict.exe`所在的正确位置:

```
Global GoldenDictFileName := "...\GoldenDict.exe"
```

编辑`GoldenDict.ahk`，修改两处键位：

- [118行](https://github.com/VimWei/GoldenDictOCR/blob/3ffa7079de4c32c05619c3a8d25b22236eeb8528/IncludeAHK/GoldenDict.ahk#L118)附近，`Send ^{c 2}` → `Send !z`
- [37行](https://github.com/VimWei/GoldenDictOCR/blob/3ffa7079de4c32c05619c3a8d25b22236eeb8528/IncludeAHK/GoldenDict.ahk#L37)附近，`^!i::` → `^!g::`

启动`GoldenDictOCR.ahk`后，按`Ctrl+Alt+G`即可开关「鼠标选择取词」。

![type:video](https://raw.githubusercontent.com/scillidan/YAFA-site/main/docs/assets/media/goldendict/with-mouse.mp4){ .skip-lightbox }

## 设置OCR取词

[Umi-OCR](https://github.com/hiroi-sora/Umi-OCR)是一款开源、可离线、功能灵活的OCR软件。用于一般OCR时，它可提供高速稳定、准确率较高的中英文混合识别。并且可以再配置一组快捷键，在OCR后触发GoldenDict查词，即[「截图联动」](https://github.com/hiroi-sora/Umi-OCR/issues/166)。配置步骤：

1. Umi-OCR → 设置 → 截图联动 On → 快捷键/修改 → 例如`Win+Shift+X`
2. ... 联动发送按键/修改 → `Alt+Z`

按`Win+Shift+X`进行框选即可查词。

![type:video](https://raw.githubusercontent.com/scillidan/YAFA-site/main/docs/assets/media/goldendict/umi-ocr.mp4){ .skip-lightbox }

另外提一提[GoldenDictOCR](https://github.com/VimWei/GoldenDictOCR)的「OCR取词」模式，不是特别推荐，可以跳过。该模式下，按`Ctrl+右键单击`可识别鼠标附近字符，按`Ctrl+反引号`可进行框选。体验上比GoldenDict内置的「屏幕取词」好一些。

需要先安装OCR工具[Capture2Text](https://capture2text.sourceforge.net/)。默认OCR语言为英语，可通过[Installing Additional OCR Languages](https://capture2text.sourceforge.net/#install_additional_languages)的说明添加额外的语言。

安装好后，进入`GoldenDictOCR`文件夹。编辑`IncludeAHK/GdOcrTool.ahk`，填写`Capture2Text.exe`所在的位置:

```
Global Capture2TextFileName := "...\Capture2Text.exe"
```

按`Ctrl+Alt+O`开关一次「OCR取词」。在`C:\Users\YourName\AppData\Roaming\Capture2Text`目录下，会生成配置文件`Capture2Text.ini`。

打开配置示例`IncludeAHK/Capture2Text.ini`，复制从`[BubbleCapture]`到`[Hotkey]`的内容，粘贴到配置文件的末尾。再添加或编辑以下两处，用于设置3个快捷键，来切换OCR语言，如：

```
[Hotkey]
Lang1=Shift+Alt+1
Lang2=Shift+Alt+2
Lang3=Shift+Alt+3
```

```
[OCR]
QuickAccessLang1=English
QuickAccessLang2=...
QuickAccessLang3=...
```

用来识别标准字形的单个英文小字时，识别率还可以，稳定性较差。

## 一些实用键位

[全部快捷键](https://xiaoyifang.github.io/goldendict-ng/ui_shortcuts/) ，也可在主菜单的「帮助」中查看。以及一些[常用快捷键](https://github.com/goldendict/goldendict/wiki/Useful-Shortcuts)，如：
- `Ctrl+L` 聚焦到输入框
- `Alt+Left/Right` 前个/后个查询记录