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

[GoldenDict](http://goldendict.org/)是一款开源的桌面端词典检索应用，跨平台，[支持多种词典格式](https://github.com/goldendict/goldendict/wiki/Supported-Dictionary-Formats)。它有十分丰富的可配置项，包括字典组，拼写检查、单词发音、语音合成、在线翻译等等。[GoldenDict-ng](https://github.com/xiaoyifang/goldendict-ng)是基于前者重写的新一代GoldenDict，修复了[一些长期累积的问题](https://github.com/xiaoyifang/goldendict-ng/issues/587)，目前仍然处于积极开发中，安装时可看[发行版块](https://github.com/xiaoyifang/goldendict-ng/releases)和[打包的版本](https://xiaoyifang.github.io/goldendict-ng/install/)。

GoldenDict向前兼容一些Windows XP之类的老旧系统，我个人也一般使用软件的稳定版，这里就以配置GoldenDict为例。软件版本为[GoldenDict 1.5.0](https://github.com/goldendict/goldendict/releases/tag/1.5.0)、[GoldenDict-ng-v23.09.29](https://github.com/xiaoyifang/goldendict-ng/releases/tag/v23.09.29-MoonCake.7f0f8778)的`6.5.2-****.zip`。二者在菜单的配置步骤上，可能有小部分差异，而配置文件几乎可以共用（不推荐）。更多的使用说明见[GoldenDict文档](https://github.com/goldendict/goldendict/wiki)和[GoldenDict-ng文档](https://xiaoyifang.github.io/goldendict-ng/)。

安装上，我使用了[Scoop](https://scoop.sh/)上的便携版，额外要手动删除`C:\Users\YourName\scoop\apps\goldendict\current\portable`目录，也就是挂掉配置文件夹`C:\Users\YourName\scoop\persist\goldendict`里的系统目录链接，而使用安装版的配置文件位置`C:\Users\YourName\AppData\Roaming\GoldenDict`，见[issue](https://github.com/goldendict/goldendict/issues/1560)。可以备份这里的`config`文件。

在配置上，我主要的需求是「词典」「在线翻译」「语法检查」「可离线使用」。

## 黑暗主题

下载[GoldenDict-Full-Dark-Theme](https://github.com/yozhic/GoldenDict-Full-Dark-Theme)。原样式有几处圆角边框，我额外进行了一些调整，可以跳过。

编辑`GoldenDict-Full-Dark-Theme\styles\Dark\article-style.css`：

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

一些字典内置的样式也可以修改，例如编辑[ODE 3/e](https://mdict.org/post/0011/)里的`ODE.css`：

``` css
// 字更小
.Od3 {
    font-size: 102%;
    line-height: 97%;
}
```

参考[Installation](https://github.com/yozhic/GoldenDict-Full-Dark-Theme#installation)段落安装样式。

GoldenDict → 编辑 → 首选项 → 附加样式 → Dark

![](dark-theme.png)

主窗口的布局可在菜单栏的「查看」中设置，窗口大小可手动调整。对于弹窗窗口，参考该[issue](https://github.com/goldendict/goldendict/issues/1010)来操作。我个人使用的布局是：主窗口只保留「查询面板」，尺寸约为屏幕的3/4，宽的1/3；弹窗的尺寸约为屏幕高的1/2，宽的1/4。

## 修改字典字体

去下载并安装收录全、完成度高的字体，例如[思源黑体](https://github.com/adobe-fonts/source-han-sans/)、[霞鹜文楷](https://github.com/lxgw/LxgwWenKai)。

还是编辑`article-style.css`，在开头添加：

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
  font-size:   9.5pt;
}

.dsl_t
{
  font-family: "Your Font", NotoSerif;
}
```

字体不会生效于有内置样式的词典，或者`.zim`档案。我个人目前使用的是[更纱黑体](https://github.com/be5invis/Sarasa-Gothic)的等宽字体。

## 添加词典

首先去下载字典文件，例如：

- [Mdict.org](https://mdict.org/)上的[新牛津英汉双解词典](https://mdict.org/post/0011/)、[企鹅英语词典](https://mdict.org/post/penguin-english-dictionary-3rd/)、[柯林斯COBUILD英语用法大全](https://mdict.org/post/collins-cobuild-english-usage/)、[韦氏发音词典](https://mdict.org/post/0010/)等等
- Mdict.org上的[汉语词典](https://mdx.mdict.org/%E6%8C%89%E8%AF%8D%E5%85%B8%E8%AF%AD%E7%A7%8D%E6%9D%A5%E5%88%86%E7%B1%BB/%E6%B1%89%E8%AF%AD/%E5%AD%97%E5%85%B8/)，推荐`汉语大词典(简体精排).mdx`、`漢語大詞典.mdx`
- [简明英汉字典增强版（CSS 版本）](https://github.com/skywind3000/ECDICT/wiki/%E7%AE%80%E6%98%8E%E8%8B%B1%E6%B1%89%E5%AD%97%E5%85%B8%E5%A2%9E%E5%BC%BA%E7%89%88#css-%E7%89%88%E6%9C%AC)
- 离线的[Online Etymology Dictionary](https://downloads.freemdict.com/%E5%B0%9A%E6%9C%AA%E6%95%B4%E7%90%86/%E5%85%B1%E4%BA%AB2020.5.11/content/1_english/z_Grammar_others/Online%20Etymology%20Dictionary/)
- 工具书，如[翻译人名](https://github.com/lxs602/Chinese-Mandarin-Dictionaries/tree/main/Chinese%20Names%20Corpus/English-Chinese%20Names)等等
- 别的语种，[Latin dictionaries](https://latin-dict.github.io/)上的拉丁语词典、[proteusx](https://github.com/proteusx)制作的古希腊语词典等等

GoldenDict → 编辑 → 词典 → 词典来源/文件 → 添加 → 词典文件们所在的目录 → 递归搜索 On → 应用。

## 添加词典群组

1. ... 词典 → 群组 → 添加群组 → `TheGroup` → 将可用词典拖入群组 → 应用
2. GoldenDict主界面 → 查找于 → `TheGroup`

我目前使用的分组是：

```
default
  在线翻译
  简明英汉字典(仅含中文释义)
  专业词汇词典
  单词拼写检查
write
  句子语法检查
  双解词典(带例句)
etymology
  词源词典
  其他语种
  单词发音
  语音引擎
```

## 添加拼写检查词库

GoldenDict已内置了部分拼写词库，勾选所需即可。如果需要额外添加词库：

1. 搜索并下载名「Hunspell Dictionary」，里面包含`.aff`、`.dic`文件，例如[English Spell Checker Dictionaries](http://wordlist.aspell.net/dicts/)上的「en_US」、[LibreOffice Hunspell dictionaries](https://github.com/LibreOffice/dictionaries)[]。要添加多个词库时，需要将所有的`.aff`、`.dic`文件放在同一目录下，例如新建一个文件夹`gd_hunspell`
2. ... 词典 → 词典来源/构词法规则库 → 变更 → `gd_hunspell`所在 → 勾选库中条目 → 应用
3. ... 群组 → 将拼写词典拖入词典群组

![](spellcheck.png)

## 添加单词发音

1. 下载语音资料[pronunciations](https://github.com/yousefvand/pronunciations)。仅英语，约2GB大小，下载过程略麻烦
2. 下载后， 解压各个压缩包到同名的文件夹
3. ... 词典 → 词典来源/音频文件目录 → 添加 → 解压后的文件所在 → 应用

对于更多的语言，可以从[该帖](https://rutracker.org/forum/viewtopic.php?t=6211002)下载[Forvo](https://forvo.com/)档案，使用[qBittorrent Enhanced](https://github.com/c0re100/qBittorrent-Enhanced-Edition)一类的工具。依据帖子内容来操作：

1. `.opus`格式的体积更小，音质也十分相近，所以下载时只勾选`Forvo_pronunciations/export/opus`。约24GB大小，下载完成后不需解压
2. 成员提到了GoldenDict的「音频文件目录」这个功能不方便也不可靠，推荐使用`.dsl`文件。可以当成是目录册，指向音频文件的位置
3. 新版本的`.dsl`文件被补充了，需要从[MEGA分享链接](https://mega.nz/folder/3CwCmR7D#05Q08F-cTKPiOTdjKXAcuw)下载，下载后解压`ForvoDSL-20220513.7z`

这里，`ForvoDSL/ForvoDSL-20220513`里的`.dsl`文件，需要同`Forvo_pronunciations/export/opus`里的`zip`，放入同一个文件夹，例如`gd_forvo`。并且修改文件名，一一对应，例如将`en.zip`重命名为`ForvoEnglish.dsl.files.zip`，对应`ForvoEnglish.dsl`。然后：

... 词典 → 词典来源/文件 → 添加 → `gd_forvo`所在 → 应用

如果要添加所有的发音词典，且考虑不去移动文件夹`Forvo_pronunciations`，不重命名文件，可以进行批量`mklink`操作。

1. 利用`ForvoDSL/ForvoDSL-20220513`里的`00README.txt`包含的信息，可编辑成一个多行的命令脚本。或者直接下载[mklink_manytimes.bat](https://gist.github.com/scillidan/6d5ceab14883a2c7a890e88e9f5532a7)
2. 编辑脚本，批量替换所有`...`为正确的路径
3. 在终端中运行`mklink_manytimes.bat`
4. ... 词典 → 词典来源/文件 → 添加 → `gd_forvo`所在 → 应用

![type:video](https://raw.githubusercontent.com/scillidan/YAFA-site/main/docs/assets/media/goldendict/pronunciation.mp4){ .skip-lightbox }

## 添加语音引擎

1. ... 词典 → 词典来源/语音合成 → 预览/可用语音引擎 → `Microsoft Huihui Desktop - Chinese (Simplified)` → 添加
2. ... 预览/可用语音引擎 → `Microsoft David Desktop - English (United States)` → 添加

此处的语音引擎来自：Windows设置 → 时间和语言 → 语音 → 语音/选择语音。

## 添加离线维基（仅gd-ng）

Kiwix是一个离线的维基阅读器，也适用于另一些网络内容，例如[StackExchange](https://stackexchange.com/)([Stack Overflow](https://stackoverflow.com/))、[Project Gutenberg](https://www.gutenberg.org/)等等，可用来提供高速、稳定的档案访问服务。它支持高度压缩的`.zim`格式，该格式可包含元数据、HTML、图像等资源。配置步骤：

1. 从[Content in all languages](https://wiki.kiwix.org/wiki/Content_in_all_languages)列表中选择档案，例如「wikipedia (English)」，型号「all maxi」表示包含了除音频、视频等大型媒体文件外的所有内容。各型号的详细说明见该[issue1](https://github.com/openzim/zim-requests/issues/129)，[issue2](https://github.com/openzim/mwoffliner/issues/485)
2. 下载档案。建议大体积的`.zim`单独放置，例如下载`wikipedia_en_all_maxi_****.zim`到`gd_wikipedia`文件夹内3
3. GoldenDict-ng → 编辑 → 词典 → 词典来源/文件 → 添加 → `.zim`所在
4. 等待GoldenDict-ng创建完索引

![](wiki.png)

对于大型档案，无论是搜索条目，还是「全文搜索」，其速度算得上「快」。例如，我需要检索「古腾堡计划」的电子书库中提到某个词的段落：

GoldenDict-ng → 搜索 → 全文搜索 → `TheWord` → 搜索 → 单击条目。

![](full-text-search.png)

额外地，也可使用[Kiwix客户端](https://kiwix.org/en/applications/)阅读`zim`档案。或者[Kiwix JS for PWA](https://github.com/kiwix/kiwix-js-pwa)，如果要部署到本地，可参考笔记[kiwix-js-pwa.md](https://github.com/scillidan/PM2-demo/blob/main/_readme/kiwix-js-pwa.md)。缺点是检索功能相比GoldenDict-ng，较为不便，仅能从搜索框搜索条目，或者页内搜索。

![](kiwix-js-pwa.png)

## 设置鼠标取词

用到了[AutoHotKey](https://www.autohotkey.com/)脚本[GoldenDictOCR](https://github.com/VimWei/GoldenDictOCR)。在它的「鼠标选择取词」模式，可以双击选词、划词来查词。GoldenDict默认的查词快捷键是`Ctrl+C+C`，如果已被占用，可修改为其他键。脚本中的键位也就需要一并修改。例如我用的是`Alt+Z`：

GoldenDict → 编辑 → 首选项 → 热键 → 使用下列热键翻译剪切板中的单词 → `Alt+Z`。

```sh
git clone https://github.com/VimWei/GoldenDictOCR
cd GoldenDictOCR
```

编辑`IncludeAHK/GdOcrTool.ahk`，填写`GoldenDict.exe`所在的正确位置:

```
Global GoldenDictFileName := "...\GoldenDict.exe"
```

编辑`GoldenDict.ahk`，修改两处键位：

```
Send ^{c 2}  // 118行附近，此处的Ctrl+C+C改为Alt+Z，即Send !z 
!i::         // 37行附近，此处的Alt+I改为Alt+Shift+G，即^!g::
```

启动`GoldenDictOCR.ahk`后，按`Ctrl+Alt+G`即可开关「鼠标选择取词」。

![type:video](https://raw.githubusercontent.com/scillidan/YAFA-site/main/docs/assets/media/goldendict/with-mouse.mp4){ .skip-lightbox }

## 设置OCR取词

[Umi-OCR](https://github.com/hiroi-sora/Umi-OCR)是一款开源、可离线、功能灵活的OCR软件。用于一般OCR时，它可提供高速稳定、准确率较高的中英文混合识别。

在[V1版本]([https://github.com/hiroi-sora/Umi-OCR/releases/tag/v1.3.5](https://link.zhihu.com/?target=https%3A//github.com/hiroi-sora/Umi-OCR/releases/tag/v1.3.5))里，可以再配置一组快捷键，在OCR后触发GoldenDict查词，即[「截图联动」](https://github.com/hiroi-sora/Umi-OCR/issues/166)。配置步骤：

1. Umi-OCR → 设置 → 截图联动 On → 快捷键/修改 → 例如`Win+Shift+X`
2. ... 联动发送按键/修改 → `Alt+Z`

按`Win+Shift+X`进行框选即可查词。

![type:video](https://raw.githubusercontent.com/scillidan/YAFA-site/main/docs/assets/media/goldendict/umi-ocr.mp4){ .skip-lightbox }

!!! repotemplate "Win键"
    
    即Windows键，或者Windows系统的菜单键。这个键默认地与一些有关的窗口(Window)功能绑定，例如我不用或者不怎么用的「截屏」、「窗口布局」、「虚拟桌面」等等。我虽然设置了[关闭Windows键热键](https://scillidan.github.io/LOG-page/#/page/65c48e97-fc16-4af2-8604-0b282509f112)，但十分清晰地，我记得在设置的几天后，它幽灵般地起了作用，然后又经常在某个时候抽风。<br/>类似的情况，在后来的GoldenDict里，我需要在用过一次`Alt+Shift+G`后，才能正常使用`Alt+Z`。
    <br/>总之，在日常使用中，我布置了一些圣物、仪式，用于在各种日常里护身，例如：[Restart Explorer](https://www.sordum.org/9192/restart-explorer-v1-7/)、「设置Ctrl+Alt+Shift+R组合键来重启小狼毫算法服务」等等。

对于V2版本，或者说更通用的情况，只需要勾选「复制OCR结果」，在OCR之后按`Alt+Z`就可以了。配置步骤：

Umi-OCR → 添加 → Screenshot OCR → Action after recognition → Copy result (开)

## 一些实用键位

[全部快捷键](https://xiaoyifang.github.io/goldendict-ng/ui_shortcuts/) ，也可在主菜单的「帮助」中查看。以及一些[常用快捷键](https://github.com/goldendict/goldendict/wiki/Useful-Shortcuts)，如：

- `Ctrl+L` 聚焦到输入框
- `Alt+Left/Right` 前个/后个查询记录