---
date: 2023-09-26
description: >
		Yomichan, translation, dictionary
---

早些年时候，由于剩余的精力和兴致，我有时在 Wiki 上翻一些不着边的天文地理等等的中文页。有时因为页面的中文内容，只是翻译了原文的开头部分，我就点到语言栏，切到英语，量了量篇幅。那些密密麻麻的鬼话，就构成了我很长一段时间里，对网络上的英语内容的印象。

2018 年左右，我在看一些中文文章时，里面介绍到了 [Chocolatey](https://chocolatey.org/)、[Hexo](https://hexo.io/index.html)、[Cmder](https://cmder.app/) 等软件和框架。我在读它们的中文文档、中文页时，更容易咀嚼和消化这些「本地化」的内容。它们也帮助我，作为担惊受怕的小白渡过了新手上路阶段。「源」对我来说，还是隐约在远处铺开的狂野、陌生的生境。

后来有了 [沙拉查词](https://saladict.crimx.com/) 等字典工具，我得以摸索「本地化」外的地盘。
## 沙拉查词

我在浏览器内外，都使用 [沙拉查词](https://saladict.crimx.com/) 来查段落或者句子，通过 [剪切板作为中继](https://saladict.crimx.com/native.html#%E6%B5%8F%E8%A7%88%E5%99%A8%E5%A4%96%E5%88%92%E8%AF%8D) 的方式。可以说，只要有英文，我就可能使用它。

为了让功能更稳定，我在「设置 → 词典帐号」中添加了词典帐号，在「设置 → 词典设置」添加了 [腾讯翻译君](https://fanyi.qq.com/)，也就是 [腾讯云](https://cloud.tencent.com/) 的 [机械翻译 TMT](https://cloud.tencent.com/act/cps/redirect?redirect=10324&cps_key=f92625eba44ff7159e1b611916c36826) 服务。如果同时使用代理梯子，需要在梯子的规则中设置域名`tencentcloudapi.com`为「直连」。

作为一种廉价、机械的服务，机械翻译始终是一板一眼的，也不是成品。它读起来生硬，还保留了原文的些许痕迹，也倒方便了作对照。如果你不需要进行具体的翻译作业，那么沙拉查词足够好。例如，这是博物学家 [阿尔弗雷德·华莱士](https://en.wikipedia.org/wiki/Alfred_Russel_Wallace) 的 [「马来群岛」](https://book.douban.com/subject/25728832/) 的 [介绍文](https://vivliostyle.org/viewer/#src=//vivliostyle.github.io/vivliostyle_doc/samples/variable-page-size/TheMalayArchipelago/main.xhtml) 的机翻文：

![](saladict_the-malay-archipelago.png)

## 沉浸式翻译

当我查 Wiki 时，有时候会使用 [沉浸式翻译](https://immersivetranslate.com/) 预处理页面，方便扫一眼段落大意。基本上，我不会单独使用它。

![](immersivetranslate.png)

## 在线词典助手

读过段落后，我可能想查一些词的词义。这方面，我想要词典「功能稳、速度快、排版密集、使用鼠标取词」。我找到了 [ODH（Online Dictionary Helper）](https://github.com/ninja33/ODH)，它的中文名虽是「在线词典助手」，但我主要使用内置的「柯林斯英汉双解」词典。

另外，因为我想显示更多更密，也很少去看例句，就按这个 [Issues](https://github.com/ninja33/ODH/issues/296#issuecomment-1474730378) 里的步骤隐藏了例句内容。

![](odh.png)
## 在线词典

### Etymonline

一些时候，我对英语词的词源感兴趣。我在 Chrome 的地址栏中添加了 [Etymonline](https://www.etymonline.com/)，以方便搜索它。需要这样设置：

Chrome地址栏 → 右键 → 管理搜索引擎和网站搜索 → 添加网站搜索 → 「网址格式」处填写`https://www.etymonline.com/search?q=%s` → 「搜索引擎」「快捷字词」处按需填写 → 在地址栏输入`快捷字词`然后`Tab`即可搜索

![](etymonline.png)

### 汉典

查中文字时，我喜欢浏览 [汉典](https://www.zdic.net/) 下列出的典故。不过我打算以后在 [Yomichan](https://foosoft.net/projects/yomichan/) 中使用 [简体版](https://github.com/MarvNC/yomichan-dictionaries#mandarin-chinese) 的 [漢語大詞典](https://gist.github.com/shoui520/25460fd2e9fb194d3e5152fa2ce42ca2#dictionaries)。

![](zdic.png)
![](yomichan_hanyudacidian.png)
## Yomichan 

[Yomichan](https://github.com/FooSoft/yomichan)，按官方介绍，它最早出于个人用途而开发，主要用于培养日语素养，曾想成为一个全方位的学习工具。因其灵活、丰富的特性，也成了另一些工具或软件如 [Kamite](https://github.com/fauu/Kamite)、[Lazy Guide](https://xelieu.github.io/jp-lazy-guide/setup/)、[Memento](https://github.com/ripose-jp/Memento) 的套件。

优点是「离线使用、多词典支持、自动分词、速度极快」。虽然没有 [Mouse Dictionary](http://github.com/wtetsu/mouse-dictionary) 快，但后者不算好用。

和使用 ODH 时一样，我使用鼠标来取词。它和 ODH 在呈现上有相似之处，细节上有些差别。就我的体验上，使用 Yomichan 更为舒适。另外一点，沙拉查词和 Yomichan 能够设置取词的热键为`Alt`，这个键在 Chrome 里，被浏览器默认占用了。这样一来，就可以把`Shift`键留给 ODH。

按照 [Kamite](https://github.com/fauu/Kamite#animevideo-text-extraction) 的说明，。最近，我想用它来读一些日版漫画，我不是很想去读英文版。使用 OCR 工具如[Vis2](https://github.com/iseahound/Vis2) 就可以）。后来，我发现它可以用于看一些文言文的书籍。虽然目前，我还没有看到这边来。

在导入特定格式的离线字典后，在浏览器内，按住设定的键，同时移动鼠标，就能对文本内容进行快速地扫描、分词、查词。
### 看动画

我原本没有使用 Yomichan 的场景。我按照 Kamite 自述下的 [一段说明](https://github.com/fauu/Kamite#animevideo-text-extraction)，配置了「看动画日文字幕」的场景。我一般看翻译的字幕，所以没有实际用上。

![type:video](Kamite_Preview_01.mkv)
![]()
### 看漫画


![type:video](Kamite_Preview_02.mkv)

https://shonenjumpplus.com/episode/3270375685341574016, raw,

[Deepl](https://www.deepl.com)，译成「chainsaw man」，https://www.reverso.net/，「チェンソーマン」译成「Chen Sawman」、「陈索曼」
### 看 PDF

只是在看`.pdf`时使用了 Yomichan。
### 看 EPUB

电子出版物「ePub」，也就是`.epub`，是一种泛用的电子书格式，在大部分智能手机、平板、电脑上都有相应的软件可使用。我主要在手机上使用 [Librera](https://f-droid.org/zh_Hans/packages/com.foobnix.pro.pdf.reader/)，而最近在电脑上，我尝试了 [ePubViewer](https://github.com/pgaskin/ePubViewer)。清理浏览器缓存的话，它能够记住读到的位置。

几乎被所有硬件阅读器支持，会重做一遍

- __OCF(OEBPS Container Format, 容器格式) 1.0__ 

所以，这里不包括`.epub`格式电子书。`.pdf`

有兴趣的去页面上的 [BookDNA](https://www.bookdna.cn/book-4905.html) 购买复刻版，但我并不清楚是什么格式。但`.mobi`、`.epub`等格式之间应该很好转换。

因为要查字、读，所以使用简体版。用FusionT，添加一种繁体字的风味。

Dictionaries → Configure installed and enabled dictionaries → Import → 选择压缩包，之后启用词典即可

汉语大词典 (On) → Priority (99)

Clipboard → Enable background clipboard text monitoring (On) → Enable search page clipboard text monitoring (On)