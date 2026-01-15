---
title: "使用MPV播放歌词"
created: 2023-09-15
modified: 2023-09-18
locale: zh
keywords: >
    mpv, lyric, cover
---

!!! read "少数派⾸发"

	文章：[使用MPV播放歌词](https://sspai.com/post/82744)  
	日期：2023-09-19  
	归属：[CC-BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/deed.zh-hans)  
	备注：1)根据编辑建议从[原稿](../archive/play-lyrics-with-mpv_archive)大修的版本 2)排版上不同

## 关于MPV

[mpv](https://mpv.io/)，参见官方的介绍，它是一款免费（且自由开放的）媒体播放器。支持多种媒体格式、音视频编解码器、字幕类型。它有一些或基础、或高级的特性：

- **高质量视频输出** 支持主流的视频驱动、一些流行的功能。
- **屏幕控制器** 在视频的上层，有一栏可浮现的简单控件。
- **显卡硬件解码** 有需要时，也可发挥显卡的性能。
- **脚本编写** 支持脚本、插件。
- **可嵌入性** 对于开发者，mpv易于作为库使用。
- **积极开发** 它还在不断地发展。

<!-- more -->

## 作为视频播放器的MPV

直到去年，我都在使用轻便、好用、有真正的图形用户界面的播放器[MPC-HC](https://github.com/clsid2/mpc-hc)。但它在播放流行的HDR（High Dynamic Range Imaging，高动态范围成像）影像时有些问题，所以我安装了mpv。

在我装上两个插件，替换控件UI的[uosc](https://github.com/tomasklaen/uosc)、制作和显示键位小抄的[mpv-cheatsheet](https://github.com/ento/mpv-cheatsheet)后，我才适应，并将mpv作为了默认的视频播放器。

![](uosc_mpv-cheatsheet.png)

但mpv究竟是怎样的？

打个比方来说，MPC-HC始终是一个好的「建筑物」，mpv则有「建筑」的概念。除了建筑的壳，它还有种种的抽象的资产。比如说好读的[文档手册](https://mpv.io/manual/master/)、较活跃的用户氛围等等。

## 作为音乐播放器的MPV和想看歌词的我

日常里，我使用了多个本地音乐播放器。我在[Exaile](https://exaile.org/)中按文件夹播放，也会将一些临时的文件拖拽进[Qmmp](https://qmmp.ylsoftware.com/)。前些年时候，我用音频标签编辑器[TagScanner](https://www.xdlab.ru/en/)制作了一些自用的`.m3u`格式的播放列表文件，用来配合插件[Mpv Filenavigator](https://github.com/jonniek/mpv-filenavigator)、[Mpv-Playlistmanager](https://github.com/jonniek/mpv-playlistmanager)在mpv中播放歌曲。

有时候，我想看看歌词。在这件事上，**Exaile**，只静态地显示`.mp3`格式的、元数据中的`lyric`内容。**Qmmp**，在曲目的右键菜单中有一个`显示歌词`功能，但不能用。**mpv**，能够以字幕的形式、样式显示`.lrc`格式的歌词，但实质、视觉上都只是字幕。

有一天，我在[mpvacious](https://github.com/Ajatt-Tools/mpvacious#secondary-subtitles)这个库下面，看到了一段演示，提到了mpv的[`--secondary-sid`](https://mpv.io/manual/master/#options-secondary-sid)设置。这个选项用于选择第二字幕，这条字幕会显示在画面的顶部。也就因为这个契机，我打算试试配置mpv来播放歌词。

![](mpvacious.png)

## 事情经过

**起初**，我想要的呈现是：**要有原文和翻译两行歌词**、**歌词样式要美观**、**要有封面**。

有了想法后，我在[reddit](https://www.reddit.com/r/mpv/comments/myvgne/how_can_i_configure_2_subtitles_at_the_same_time/)的一个问答帖下，找到了「如何在mpv中设置双字幕」的基本方法。

**之后**，通过翻阅mpv官方手册，我一一找到并测试了关联的选项。

**中途**，我从可信的源[SourceForge](https://sourceforge.net/projects/mpv-player-windows/files/)上下载了mpv便携版，替换测试用的环境。因此，我发现并修改了不少的错误和疏忽。

**最后**，我用这个配置听了一遍[Until The Ribbon Breaks - Orca](https://genius.com/Until-the-ribbon-breaks-orca-lyrics)，还有一些其他的歌。

总结一下：**一**，关键功能几乎都基于mpv自带；**二**，视觉风格是根据自己偏好设置；**三**，使用了插件来获得更好的用户体验，或是处理特殊情况。

## 呈现效果

![](mpv-lrc.gif)

## 主要功能

- 音乐播放器一般功能：
    - 菜单
    - 随机播放
    - 列表循环
    - 单曲循环
    - 上一曲/下一曲
    - 当前播放列表/文件浏览器
- 可切换第二歌词（字幕轨）
- 可关闭第二歌词
- 可修改歌词的样式
- 可显示静态歌词，以`.srt`字幕格式
- 可添加多个图片作为封面
- 可添加循环的视频作为封面（视频轨）
- 播放时可切换封面，且窗口会根据画面比例自动调整

## 如何食用

### 安装MPV和插件

- [mpv](https://mpv.io/installation/)
    - [uosc](https://github.com/tomasklaen/uosc)
    - [mpv-quality-menu](https://github.com/christoph-heinrich/mpv-quality-menu)
    - [reload.lua](https://github.com/sibwaf/mpv-scripts#reloadlua)（可选的）

### 新建脚本文件

新建一个`lyric.cmd`文件，粘贴进下面的内容，并删除`//`和后面的注释：

```sh
mpv ^
	--video-aspect-override=no ^
	--force-window=yes ^
	--autofit=x500 ^
	--cover-art-files="cover01;cover02;cover03" ^
	--external-files="video-loop.mp4" ^
	--vid=1 ^ // 以上跟封面有关
	--sub-auto=fuzzy ^
	--sid=2 ^
	--secondary-sid=1 ^
	--sub-color="#ffffff" ^
	--sub-back-color="#000000" ^
	--sub-border-size=0 ^
	--sub-font="LXGW WenKai Mono GB Light" ^
	--sub-font-size=25 ^
	--sub-align-y=center ^
	--sub-margin-y=320 ^
	--sub-justify=left ^
	--vo=gpu-next ^ // 以上跟歌词字幕有关
	--loop-file=no ^
	--idle ^ 
	--ontop ^ // 一些可选项
	%*
```

`--cover-art-files`的值，填写成封面图片的完整路径。

`--external-files`的值，可留空或填写成视频封面的路径。

### 安装字体

推荐楷体、收录范围很广的「霞鹜文楷 GB」，从[Github](https://github.com/lxgw/LxgwWenkaiGB)或[猫啃网](https://www.maoken.com/freefonts/16864.html)下载都可以。

### 运行脚本

在设置好PATH后，就可以在终端中运行`lyric yourmusic`。

## 特殊情况

### 播放歌曲文件夹

当你拖拽文件夹进mpv来播放时，mpv不会将`.lrc`等文件排除在播放列表外。这时，你可以使用[reload.lua](https://github.com/sibwaf/mpv-scripts#reloadlua)进行重载。它会清除当前播放列表，而使用文件浏览器。

### 显示静态歌词

创建一个`.srt`文件，只写一条时间轴，就可以静态地显示多行歌词：

```
1
00:00:00,000 --> 01:00:00,000
some lyrics
some lyrics
\n
some lyrics
...
```

但在首次加载时，因为上面的设置关系，它会从居中位置、而不是顶部开始显示歌词。可以使用[reload.lua](https://github.com/sibwaf/mpv-scripts#reloadlua)进行重载，或者忽略掉也可以。

### 不想从终端启动

如果你不想从终端启动`lyric.cmd`，可以再新建一个`mpv_lyric.vbs`：

```
Set WshShell = CreateObject("WScript.Shell")
  WshShell.Run chr(34) & "yourpath/lyric.cmd" & Chr(34), 0
Set WshShell = Nothing
```

你可以把`mpv_lyric.vbs`当做是快捷方式，你能够像使用一般的应用那样去打开它，控制台窗口也被隐藏了。

## 结语

最后提一下，封面加载的默认顺序，依次是：内镶封面 → `--cover-art-files`处设置封面 → `--external-files`处设置的视频 → `同名文件.*`图片 → `cover.*`图片。

![](mpv-lrc.jpg)