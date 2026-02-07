---
title: "浏览器中的机械翻译、电子词典插件"
created: 2023-09-26
modified: 2022-10-13
locale: zh
keywords: >
    translate, e-dictionary, Yomichan, Saladict, ODH
---

!!! read "少数派⾸发"

    文章：[在浏览器里使用机械翻译和电子词典](https://sspai.com/post/83385)  
    日期：2023-10-09  
    归属：[CC-BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/deed.zh-hans)  
    备注：1)排版上不同 2)有少量校正内容 3)补充了一个段落

早些年时候，由于剩余的精力和兴致，我在Wiki上翻过一些天文地理等等的不着边的页面。一些的中文页，只是翻译了原文的开头或部分的内容。有几次，我就点进了语言栏，切到英语，目测了篇幅。那些密密麻麻的鬼话，就构成了我很长一段时间里，对网络上的外语内容的印象。

2018年左右，我在搜索和阅读时，了解到了[Chocolatey](https://chocolatey.org/)、[Cmder](https://cmder.app/)、[Hexo](https://hexo.io/index.html)等一众软件或框架。翻着中文文章和文档，我作为小白渡过了担惊受怕的初见阶段。随着经验值提升，在某一天，我也终于装备上了[沙拉查词](https://saladict.crimx.com/)、[Yomichan](https://foosoft.net/projects/yomichan)等翻译、字典工具，开始到「本地化」外的地方野游。

<!-- more -->

## 沙拉查词

我在浏览器内外，都使用[沙拉查词](https://saladict.crimx.com/)来查段落或者句子，通过[剪切板作为中继](https://saladict.crimx.com/native.html#%E6%B5%8F%E8%A7%88%E5%99%A8%E5%A4%96%E5%88%92%E8%AF%8D)的方式。可以说，只要有英文，我就可能使用它。

为了让功能更稳定，我在「设置 → 词典帐号」中添加了[腾讯翻译君](https://fanyi.qq.com/)，也就是[腾讯云](https://cloud.tencent.com/)的[机械翻译TMT](https://cloud.tencent.com/act/cps/redirect?redirect=10324&cps_key=f92625eba44ff7159e1b611916c36826)服务。如果同时使用代理梯子，需要在梯子的规则中设置域名`tencentcloudapi.com`为「直连」。

作为一种廉价、机械的服务，机械翻译始终是一板一眼的。它生硬难啃，还保留了原文的些许痕迹，也倒方便了作对照。如果你不需要进行具体的翻译作业，那么沙拉查词足够好用。例如，这是博物学家[阿尔弗雷德·华莱士](https://en.wikipedia.org/wiki/Alfred_Russel_Wallace)的[「马来群岛」](https://book.douban.com/subject/25728832/)的[一篇介绍](https://vivliostyle.org/viewer/#src=//vivliostyle.github.io/vivliostyle_doc/samples/variable-page-size/TheMalayArchipelago/main.xhtml)的机翻文：

![](saladict_the-malay-archipelago.png)

## 沉浸式翻译

当我查Wiki时，有时候会使用[沉浸式翻译](https://immersivetranslate.com/)预处理页面，方便扫一眼段落大意。基本上，我不会单独使用它。

![](immersivetranslate.png)

## 在线词典助手

翻译工具以外，我还需要词典。我想要它「速度快、排版密集、使用鼠标取词」。我找到了[ODH（Online Dictionary Helper）](https://github.com/ninja33/ODH)，它的中文名虽是「在线词典助手」，但我主要使用内置的、也就是离线的「柯林斯英汉双解」词典。

![](odh.png)

但它在交互功能上有些不足。在Chrome里，`Alt`键被浏览器默认占用，也因此闲置。ODH没能像沙拉查词和[Yomichan](https://foosoft.net/projects/yomichan)那样夺回`Alt`键。而分配`Shift`键时，输入文字时按到`Shift`，会有奇怪的Bug。也因此，这个插件默认处于关闭状态。

另外，因为我想密集地显示词义，也不常看例句，就按这个[issue](https://github.com/ninja33/ODH/issues/296#issuecomment-1474730378)里的步骤隐藏了英文例句。

## 在线词典

### Etymonline

我对词源很感兴趣。我在Chrome的地址栏中添加了[Etymonline](https://www.etymonline.com/)网站，以方便搜索它。需要这样设置：

Chrome地址栏 → 右键 → 管理搜索引擎和网站搜索 → 添加网站搜索 → 「网址格式」处填写`https://www.etymonline.com/search?q=%s` → 「搜索引擎」「快捷字词」处按需填写 → `Ctrl+L` → 输入`快捷字词`然后`Tab`即可搜索。

![](etymonline.png)

### 汉典

查中文字时，[汉典](https://www.zdic.net/)下罗列的典故很丰富。

![](zdic.png)

## Yomichan 

[Yomichan](https://foosoft.net/projects/yomichan)，按官方介绍，它最早出于个人用途而开发，主要用于培养日语素养，也曾想成为一个全方位的学习工具。因其灵活、丰富的特性，成为了另一些工具如[Kamite](https://github.com/fauu/Kamite)、[Lazy Guide](https://xelieu.github.io/jp-lazy-guide/setup/)、[Memento](https://github.com/ripose-jp/Memento)的套件。

就一般用户来说，优点主要有：

1. 可离线使用、支持多词典、速度快
2. 默认排版紧凑美观
3. 配置项丰富、实用
4. 交互灵活、体验好

额外的，我使用了[缝合楷](https://github.com/lxgw/FusionKai)作为字典的字体。先下载并安装这个字体，然后在 Yomichan 里进行设置：选项 → Popup Appearance → Configure custom CSS → Popup CSS → Custom CSS

```
body {
    font-family: "Fusion Kai G", sans-serif;
    font-style: normal;
}
```

### 啃动画生肉

我一般看翻译的字幕。但按照[这段说明](https://github.com/fauu/Kamite#animevideo-text-extraction)，可以用于啃日语生肉。

首先，安装[Yomichan](https://foosoft.net/projects/yomichan)、[Kamite](https://github.com/fauu/Kamite#animevideo-text-extraction)、[mpv](https://mpv.io/)。

然后，在Yomichan拓展程序的图标处右键 → 选项 → Dictionaries → Configure installed and enabled dictionaries → 启用词典「JMdict」。

之后，新建一个`kamite.cmd`：

```sh
mpv ^
  --input-ipc-server=\\.\pipe\kamite-mpvsocket ^
  --sub-file=%2 ^
  --sid=2 ^
  --secondary-sid=1 ^
  --secondary-sub-visibility=no ^
  --save-position-on-quit ^
  %1
```

启动Kamite后，去终端里运行`kamite yourmovie your-jpn-subtitle`就行了。

![type:video](https://raw.githubusercontent.com/scillidan/YAFA-site/main/docs/assets/media/e-dict/Kamite_Preview_01.mkv){ .skip-lightbox }

### 啃漫画生肉

也可以用于看日版漫画。

在有了Yomichan、Kamite后，参考[Setting up “Manga OCR” (Local)](https://github.com/fauu/Kamite#setting-up-manga-ocr-local)段落，使用[pipx](https://pypa.github.io/pipx/)安装[manga-ocr](https://github.com/kha-white/manga-ocr)。

之后，编辑配置文件`C:/Users/yourname/AppData/Roaming/kamite/config.hocon`的`ocr`部分，没有提提及的内容，保留默认设置即可：

```
ocr: {
  engine: mangaocr
  mangaocr: {
    pythonPath: "C:/Users/yourname/.local/pipx/venvs/manga-ocr/Scripts/python.exe"
  }
}
```

如果你还需要机翻功能，修改`config.hocon`的`lookup`和`LOOKUP_TARGETS`部分：

```
lookup {
  targets = [
    ${LOOKUP_TARGETS.reverso}
    ...
  ]
}

LOOKUP_TARGETS {
  reverso {
    symbol = DEP // 借用DeepL的图标
    name = Reverso
    url = "https://www.reverso.net/text-translation#sl=jpn&tl=eng&text={}"
  }
  ...
}
```

在风味上，[Reverso](https://www.reverso.net/)的机翻味很重。如果想使用[Deepl](https://www.deepl.com)，需要参考[Lookups](https://github.com/fauu/Kamite#lookups)段落来设置（略麻烦）。

最后，启动Kamite，再打开漫画阅读窗口就行了。

![type:video](https://raw.githubusercontent.com/scillidan/YAFA-site/main/docs/assets/media/e-dict/Kamite_Preview_02.mkv){ .skip-lightbox }

### 啃古典文学PDF

之前听播客时，我听到了一期[「红楼梦」](https://www.ximalaya.com/sound/587170813)。按照播客内容推荐的版本，我在网络上找到了两个简体版的`.pdf`、和一个繁体竖排版的`.epub`。按版本号排序的话，应该是这样的：

![](hong-lou-meng_cover.jpg)

需要注意的是，`.pdf`由社区发布，供个人免费、正当使用。`.epub`则是由出版公司发行，可按页面上的网址购入，不过页面上给到的信息不多。文件应该是用在[Kindle](https://en.wikipedia.org/wiki/Amazon_Kindle)上的`.mobi`格式，可以自己试试用[Calibre](https://calibre-ebook.com/)转换成`.epub`。

![](hong-lou-meng_shuoming.jpg)

在后一个`.pdf`里，当我复制其中文字时，字符没有被正确识别。我不清楚怎么去修复，所以这里以前一个`.pdf`为例。

首先，下载[漢語大詞典(简体版)](https://github.com/MarvNC/yomichan-dictionaries#mandarin-chinese)。

然后，在Yomichan的设置里导入词典：选项 → Dictionaries → Configure installed and enabled dictionaries → Import → 选择下载的压缩包 → 待导入完成，启用词典。

在交互方面，进行「鼠标取词」，这里需要使用Web端PDF阅读器，如[doqment-demo](https://shivaprsd.github.io/doqment/src/pdfjs/web/viewer.html)或者[PDF.js-demo](https://github.com/mozilla/pdf.js#online-demo)。

![](yomichan_pdf_01.png)

如果你使用本地阅读器如[Sumatra PDF](https://www.sumatrapdfreader.org/free-pdf-reader)，或者浏览器插件如[doqment PDF Reader](https://github.com/shivaprsd/doqment#installation)——因为Yomichan不能在浏览器外取词，也不能在插件里用插件——需要用「剪切板」的方式交互：选项 → Clipboard → Enable background clipboard text monitoring (开) → Enable search page clipboard text monitoring (开)。

之后，当你在阅读器里`Ctrl+C`时，就会弹出字典面板。面板出现时尺寸偏小，去设置一下弹窗的尺寸：选项 → Window → Size → 设置宽和高。

![](yomichan_pdf_02.png)

### 啃古典文学EPUB

电子出版物「ePub」，也就是`.epub`，是一种泛用的电子书格式，在大部分智能手机、平板、电脑上都有相应的软件可使用。

[ePubViewer](https://github.com/pgaskin/ePubViewer)是一个美观易用的Web端ePub阅读器。它的外观和菜单如图：

![](epubviewer_01.png)
![](epubviewer_02.png)

使用方法有好几种，步骤从简单到复杂：

- 直接使用原作者的[网页](https://pgaskin.net/ePubViewer/)或者[fork网页](https://gm-epubviewer.vercel.app/)
- `clone`源码到本地 → 在浏览器中打开`index.html`
- 从源码fork → 按需修改（参考此处的[index.html](https://github.com/scillidan/ePubViewer/blob/gh-pages/index.html)、[style.css](https://github.com/scillidan/ePubViewer/blob/gh-pages/style.css)、[script.js](https://github.com/scillidan/ePubViewer/blob/gh-pages/script.js)） → 部署fork库到[gh-page](https://docs.github.com/zh/pages/getting-started-with-github-pages/creating-a-github-pages-site)或者[Vercel](https://vercel.com) → 使用网页
- 从源码fork → `clone`fork库到本地 → 按需修改 → 使用[PM2](https://pm2.keymetrics.io/)部署它 → 使用网页（运行在本地）

它们在主要功能上没有多大区别。区别之一是，如果你使用的是平台托管的Web服务，在设置文字字体时，会用到`.woff`、`.woff2`格式的Web字体。如果你在本地使用，可以用电脑上已安装的字体：

1. 以在Chrome中为例 → 设置 → 外观 → 自定义字体 → Serif字体 → 选择本地字体
2. ePubViewer → 更多菜单 → 设置 → Font → 选择任何一个Serif字体

字体方面，推荐上图中的[思源宋体](https://github.com/adobe-fonts/source-han-serif)和下图的[缝合楷](https://github.com/lxgw/FusionKai)。词典则使用[漢語大詞典](https://gist.github.com/shoui520/25460fd2e9fb194d3e5152fa2ce42ca2#dictionaries)。

![](yomichan_epub_02.png)

作为电子书阅读器，需要注意的是：

- 如果不清除浏览器的缓存，它能够保存阅读进度。
- 它不能记住上一次打开的文件。如果不想每一次打开网页，都要重新定位`.epub`文件，可以参考这个[issue](https://github.com/pgaskin/ePubViewer/issues/4#issuecomment-1687223964)，在网址里带上`文件地址`。

### 作为桌面端词典

通过在Chrome设置全局快捷键，可以把Yomichan当作桌面端的词典使用：[键盘快捷键](chrome://extensions/shortcuts) → Yomichan → Open the popup window → `Alt+Shift+Y` → 全局。

### 在安卓上

参考[Setup Yomichan Android](https://xelieu.github.io/jp-lazy-guide/setupYomichanOnAndroid/)里的详细说明，可以在安卓手机上使用Yomichan。安装和设置好后：在浏览器里打开[fork网页](https://gm-epubviewer.vercel.app/) → 浏览器 → 更多 → 桌面版网站 (开)。

![](yomichan_android.jpg)

我只进行了测试。如果你有触控笔，可能使用体验会好一些。

