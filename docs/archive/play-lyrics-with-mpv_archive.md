---
title: 使用MPV播放歌词（初稿）
date:
    created: 2023-09-01
    updated: 2023-09-06
keywords: >
    mpv, lyric, cover
locale: en
---

!!! read ""
  
	日期：2023-09-06  
	归属：[CC-BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/deed.zh-hans)  

## 前情提要

我使用的本地音乐播放器，大多数时候是按文件夹播放的[Exaile](https://exaile.org/)。偶尔，也会选中文件，拖进[Qmmp](https://qmmp.ylsoftware.com/)播放。或者用[mpv](https://mpv.io/)打开播放列表`.m3u`。

另一方面，我有一些没使用的`.lrc`文件。Exaile只静态地显示`.mp3`格式的，元数据中的`Lyric`内容。Qmmp，在它的`设置 → 插件`界面，有一项已安装的`歌词插件`，但我一直没找到这个功能，直到我偶然间点开了右键菜单。mpv默认会加载同名的字幕文件（包括`.lrc`），也会显示名同名的，或者名为`cover`的图片（一般是`.jpg`、`.png`）。

我在浏览[Ajatt-Tools/mpvacious](https://github.com/Ajatt-Tools/mpvacious#secondary-subtitles)这个库时，还看到了一段关于mpv的[`--secondary-sid=`](https://mpv.io/manual/master/#options-secondary-sid)选项的介绍和演示，用于选择第二条字幕轨。这个功能的表现，有点像是分离式的双语`.ass`字幕文件，第二条字幕轨会显示在画面顶部。

<!-- more -->

## 命令

有了`mpv`，还需要在命令后，跟上一堆可选项：

```sh
mpv --force-window=yes --autofit=x500 --video-aspect-override=no --cover-art-files="cover1;cover2;cover3;..." --vid=1 --sub-auto=fuzzy --sid=2 --secondary-sid=1 --sub-color="#ffffff" --sub-back-color="#000000" --sub-font="LXGW WenKai Mono GB Light" --sub-font-size=25 --sub-align-y=center --sub-margin-y=375 --sub-justify=left --vo=gpu-next --idle %1
```

也可以按多行的风格来写：

```sh title="lrc.cmd"
mpv ^
	--force-window=yes ^
	--autofit=x500 ^
	--video-aspect-override=no ^
	--cover-art-files="cover1;cover2;cover3;..." ^
	--vid=1 ^
	--sub-auto=fuzzy ^
	--sid=2 ^
	--secondary-sid=1 ^
	--sub-color="#ffffff" ^
	--sub-back-color="#000000" ^
	--sub-font="LXGW WenKai Mono GB Light" ^
	--sub-font-size=25 ^
	--sub-align-y=center ^
	--sub-margin-y=375 ^
	--sub-justify=left ^
	--vo=gpu-next ^
	--idle ^
%*
```

另存为脚本文件`lyric.cmd`，也就可以使用它的别名来运行：

```sh
lyric somemusic
```

或者`lyric`，然后把`歌曲`拖进mpv窗口。
## 命令可选项

其中的`选项`和`值`的意思是：

- `--force-window=yes` 如果媒体文件没有任何视频轨，使用黑屏
- `--autofit=x500` 视频画面大小 → 高度为500px
- `--video-aspect-override=no` 不覆盖长宽比例
- `--cover-art-files="cover1;cover2;cover3;..."` 加载多个封面图片文件，会被视为视频轨
- `--vid=1` 视频 → 视频轨1
- `--sub-auto=fuzzy` 字幕加载方式 → 匹配包含媒体文件名的所有字幕
- `--sid=2` 第一字幕 → 字幕轨2
- `--secondary-sid=1` 第二字幕 → 字幕轨1
- `--sub-color="#ffffff"` 字幕颜色 → 白色
- `--sub-back-color="#000000"` 字幕背景色 → 黑色
- `--sub-font="LXGW WenKai Mono GB Light"` 字幕字体 → [霞鹜文楷等宽 GB Light](https://github.com/lxgw/LxgwWenkaiGB)
- `--sub-font-size=25` 字幕字号
- `--vo=gpu-next` 指定视频输出的硬件
- `--sub-align-y=center` 字幕在y轴上的对齐方式 → 居中
- `--sub-margin-y=375` 字幕在y轴上的外边距
- `--sub-justify=left` 字幕文本的对齐方式 → 左对齐

一些原因、说明、废话：

- `x500` 很多文件的内嵌封面是`500x500`，也有些不是。我也不打算使用高清质量的封面图。和字幕有关的`--sub-font-size`，`--sub-margin-y`选项的数值，跟这里的值相关，一边预览着，来设置这三个值就可以。
- `--video-aspect-override=no` 不同比例的封面间，进行切换时，画面不会出现黑边。
- `--vid=` 这里的视频轨的编号顺序，也是封面加载的优先顺序，依次是内镶封面 → `--cover-art-files`加载的封面 → `同名文件.*`图片 → `cover.*`图片。
- `--sid=2`和`--secondary-sid=1` 我一般保存原文歌词在`同名文件.lrc`，中文歌词在`同名文件_chs.lrc`，mpv在加载字幕时也是按`文件名称`这个顺序。如果调换两条字幕轨，就可以在播放时点开字幕控件，关掉字幕1，也就是中文歌词。（还需要安装插件）
- `LXGW WenKai Mono GB Light` 好些年前，我下载到了[*美国往事（Once Upon a Time in America）*](https://www.imdb.com/title/tt0087843/)的一个`.ass`字幕，它用的就是`楷体`。还在使用[MPC-HC](https://github.com/clsid2/mpc-hc)时，我还需要[xy-VSFilter](https://github.com/Cyberbeing/xy-VSFilter)去覆盖字幕的字体样式，和热加载，但有些小问题。使用和配置mpv后，是在配置文件`mpv.conf`里加一行`sub-font=`。这里倒是可以填中文名`霞鹜文楷等宽 GB Light`。另外，在mpv实机预览了几种`黑体`、`衬线体`等后，还是`楷体`好看。
- `--vo=gpu-next` 我使用了小字号，不设置这个的话，字很糊。
- `375` 通过设置`--sub-margin-y`，默认位于底部的字幕1会向上移动，位于顶部的字幕2会向下移动。
- `--sub-align-y=center`会将本来位于底部的字幕1居中，并且锁定。设置`--sub-margin-y`时，只会移动字幕2。
- `--sub-justify=left` 用于多行字幕。
- `--idle` 可以从终端里`Ctrl+C`或者`Q`直接关闭mpv。不然会让你确认。
- `%*` 可以理解成占位符（完形填空）。例如在终端敲入`lyric D:/文件夹/music1.mp3 D:/文件夹/music2.mp3`，运行时`%*`就成了`D:/文件夹/music1.mp3 D:/文件夹/music2.mp3`。一般可填写：
    - `歌曲文件`常见的音乐格式都可以。
    - `歌曲文件夹` 因为前面的`--sub-auto=fuzzy`设置，当前播放列表会包含上`.lrc`等字幕文件。
    - `播放列表` `.m3u`格式，本地播放器一般可以将当前播放列表保存为`.m3u`。

额外选项：

- `--ontop` 播放时保持画面在最前面
- `--loop-file=yes` 单曲循环
- `--cover-art-auto=all` 加载当前文件夹的所有图片为封面图片。可在挑选封面和预览时使用
- `--external-files` 在这里，可以加载一个没有音轨的视频，视频长度最好超过歌曲。在想使用循环视频作为封面时使用
- [`--wid=<ID>`](https://mpv.io/manual/master/#options-wid) 似乎可以选择要输出的屏幕。我没有副屏，所以没有测试。

## 基本效果预览

### 有内置封面

![](covers.jpg)

### 没有内置封面

![](sinus.jpg)

### 双字幕

![](baby-seal.jpg)

### 修改风格

修改字幕条配色：

```
--sub-color=#353535
--sub-back-color=#afafaf
```

![](lets-play-birds.jpg)

使用其他`高度`或`比例`的封面时，需要修改`--sub-font-size`、`--sub-margin-y`：

![](orca.jpg)
## 使用说明

主要会用到：

- [mpv](https://mpv.io/)
  - 插件[uosc](https://github.com/tomasklaen/uosc)
  - 插件[mpv-quality-menu](https://github.com/christoph-heinrich/mpv-quality-menu)
  - 插件[reload.lua](https://github.com/sibwaf/mpv-scripts#reloadlua)
- 终端

非新手可跳过下面部分。
### 安装mpv

只要从受信任的源下载mpv，都是可以的。这里介绍两种安装方式：

1. 便携安装：[mpv.io](https://mpv.io/) → installation → Windows → Windows builds 或者 [sourceforge.net](https://sourceforge.net/projects/mpv-player-windows/files/) →
2. 64bit → 选择一个压缩包 → 下载 → 解压缩
3. （差不多算）常规安装：[scoop](https://scoop.sh/)用户可以`scoop install mpv`

安装完后，你有了可执行程序`mpv.exe`，也是命令`mpv`的本体。
### 安装终端

虽然使用Windows自带的`命令提示符`就可以，但它也简陋得容易把人劝退。我使用的是[Cmder](https://cmder.app/)，你也可以去Windows商店安装[Windows Terminal](https://apps.microsoft.com/store/detail/windows-terminal/9N0DX20HK701)。

### 设置PATH

部分用scoop安装的软件，已经设置好了PATH。这里以安装了便携版mpv为例。

1. `Win+Q` → `编辑系统环境变量` → 高级 → 环境变量 → 用户变量 → 变量`Path` → 编辑 → 新建 → 浏览 → 选择`mpv.exe`所在的文件夹
2. 或者你也可以用[Rapid Environment Editor](https://www.rapidee.com/en/about)，一个环境变量编辑器
3. `Win+Q` → `Windows Terminal` → 命令提示符 → `mpv` → 回车
4. 如果终端没有找到命令，运行`refreshenv`，再重启终端

设置完PATH后，你就可以在终端里运行`mpv`命令。

### 运行命令

你可以先进行测试，和按需调整。复制上面`lyric.cmd`的内容，粘贴进终端运行。但要将`%1`，换成文件或文件夹的绝对路径，例如如`D:/文件夹/歌曲文件`或者`D:/歌曲文件夹`。

### 使用别名运行

因为在`PATH`设置好了`mpv文件夹`。命令调试完后，就可以在内，新建一个`lyric.cmd`文件。因为Windows的`资源管理器`的缘故，如果你没有[Directory Opus](https://www.gpsoft.com.au/index.html)这类文件管理器，你需要在终端中这样：

1. `D:` 切换到某个盘，这里以`D盘`为例
2. `cd D:/某文件夹/mpv文件夹`。你可以从资源管理器的地址栏，复制后面这个路径
3. `touch lyric.cmd`

如果你没有[Sublime Text](https://www.sublimetext.com/)这类文本编辑器，你需要：

1. 右键`lyric.cmd` → 打开方式 → `记事本`
2. 复制上面的`lyric.cmd`的内容，粘贴进去 → 保存

之后就可以在终端里：

```
lyric D:/路径/歌曲或歌曲文件夹
```
### 安装mpv插件

- 便携版mpv的配置文件夹是`解压缩后的文件夹/mpv`
- `scoop`版mpv位于`C:/Users/yourname/scoop/apps/mpv/current/portable_config`

文件夹内的结构一般是：

```
.
├── script-opts
│   └── ...
├── scripts
│   └── ...
├── fonts.conf
├── input.conf
└── mpv.conf
```

一般说明：

- `scripts/` 插件的脚本文件夹
- `scripts-opts/` 插件的配置文件夹
- `input.conf` 输入(键位)配置
- `mpv.conf` mpv全局配置

插件的安装几乎都是：

- 脚本文件放进 `scripts/`，一般是`*.lua`
- 脚本配置放进 `scripts-opts/`，一般是`*.conf`

仓库下一般还有安装说明。以[uosc](https://github.com/tomasklaen/uosc)为例：

1. 去[tomasklaen/uosc](https://github.com/tomasklaen/uosc)下载
  1. 或者`curl https://github.com/tomasklaen/uosc/archive/refs/heads/main.zip --output uosc.zip`
  2. 或者`curl -k ...`
  3. 或者去手机上用外部浏览器打开`https://github.com/tomasklaen/uosc/archive/refs/heads/main.zip --output`，把文件传到PC上
2. 解压`.zip`
3. 把`uosc_shared\`、`uosc.lua`放进`mpv文件夹/mpv/scripts`
4. 把`uosc.conf`放进`mpv文件夹/mpv/scripts-opts`
5. 重新运行`mpv`

### 获取字体

从[Github](https://github.com/lxgw/LxgwWenkaiGB)或[猫啃网](https://www.maoken.com/freefonts/16864.html)下载，并安装即可。

### 获取歌词

一般来说需要另外下载，你可以从[Github](https://github.com/)搜到相关的脚本。静态歌词可以从[Genius](https://genius.com/)获得，还会包括一些额外的注解，如[Until The Ribbon Breaks - Orca Lyrics](https://genius.com/Until-the-ribbon-breaks-orca-lyrics)。

### 获取封面

如果你没有[XnConvert](https://www.xnview.com/en/xnconvert/)这类工具去制作`高度500`的封面，也可以使用这张从[freesvg.org](https://freesvg.org/)下载的免版权的图：

![](cover.png)

或者视频封面，下载5分钟长的循环视频。使用时，在命令中加上一行：

```
--external-files="D:/路径/loop_480p.mp4"
```

![](video-cover.gif)
## 基本操作

安装了[uosc](https://github.com/tomasklaen/uosc)后，视频画面可显示美观实用的UI控件：
- 打开菜单
- 开关字幕（歌词）。仅限字幕1
- 开关随机播放。播放`歌曲文件夹`、`播放列表.m3u`时可用
- 开关列表循环
- 开关单曲循环
- 上一曲/下一曲
- 查看当前播放列表。也是一个文件浏览器

![](uosc.jpg)

## 特别操作

因为`lyric.cmd`里的预设的缘故，在一些特殊场景下，需要一些追加操作。

### 播放歌曲文件夹

播放`音乐文件夹`时，播放列表会加载文件夹里的字幕。音乐开始播放时，可用到插件[reload.lua](https://github.com/sibwaf/mpv-scripts#reloadlua)，默认键位`R`重新加载一下。但实际上这里是清除了当前播放列表，而使用文件浏览器，按`...`就可以返回上层文件夹。

![](play-folder.jpg)

如果文件夹内不是`.mp3`格式，还会有些奇怪的问题。所以不推荐用`歌曲文件夹`。
### 显示静态歌词

1. 创建一个`.srt`格式的字幕文件。只写一条时间轴，就可以用于显示静态歌词
2. 按`R`重加载，修复一下字幕位置
3. 歌词显示不全也挺常见。这时使用mpv默认键位，调整大小：
	- `Shift+f` 字幕调小10%
	- `Shift+g` 字幕调大10%

![](show-srt.jpg)

这时如果画面过小。在`input.conf`里加上：

```
+ add window-scale 0.5
- add window-scale -0.5
= set window-scale 1
```

- `+` 窗口尺寸增加视频尺寸的50%
- `-` 窗口尺寸减小视频尺寸的50%
- `=` 重置窗口尺寸为视频尺寸

### 关闭歌词

这里只关闭字幕1，即图中的`orca_chs.lrc`。

![](disable-subtile.jpg)

### 调整时间轴

除非你完全使用[lrc_editor](https://github.com/yiyizym/lrc_editor)这类工具自制`.lrc`，否则多少会遇到歌词提前或延迟。使用默认键位：
- `Z` 前移0.1秒
- `z` 后移0.1秒

### 切换封面

安装了[mpv-quality-menu](https://github.com/christoph-heinrich/mpv-quality-menu)后，就可以选择封面。

![](toggle-cover.jpg)

### 其他用途

如果用到[漢字→廣東話/粵語拼音轉換工具](https://hongkongvision.com/tool/cc_py_conv_zh)、[RomajiDesu](https://www.romajidesu.com/translator)这些转写工具，可以加一行辅助音。

![](pinyin.jpg)

## 最后

我在PC上使用便携版mpv进行了测试，不排除遗漏或者忽略了某一些条件或场景。事实就是，几乎都是mpv内置的功能。mpv即是实用的播放器，也兼有`lib`用途。

## 封面链接

1. [西日に照らされて](https://www.pixiv.net/en/artworks/83880563) by [ゲン助](https://www.pixiv.net/en/users/32008)
2. [有限](https://www.pixiv.net/en/artworks/83880563) by [ゲン助](https://www.pixiv.net/en/users/32008)
2. [1155615](https://danbooru.donmai.us/posts/1155615) by mugon
3. [風船の旅立ち](https://www.pixiv.net/en/artworks/54491212) by [しまざきジョゼ](https://www.pixiv.net/en/users/762663)
4. [宵の灯](https://www.pixiv.net/en/artworks/107887513) by [縹 京介](https://www.pixiv.net/en/users/2351667)

## 参考链接

1. [How can I configure 2 subtitles at the same time?](https://www.reddit.com/r/mpv/comments/myvgne/how_can_i_configure_2_subtitles_at_the_same_time/)
2. [mpv - manual](https://mpv.io/manual/master/)