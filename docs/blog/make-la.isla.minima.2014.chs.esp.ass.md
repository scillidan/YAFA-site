---
title: "制作la.isla.minima.2014.srt.ass"
created: 2021-01-01
modified: 2021-12-31
locale: zh
keywords: >
    subtitle, translate, GoldenDict, Saladict, autosub, alass-cli
---

本篇记叙：外乡人如何借助道具，尝试翻译一门小语种的电影生肉文本，并制作字幕文件。

本次作业介绍了涉及的软件、插件、命令行等工具。内容主要有：安装，设置、操作等。且仅出于个人学习目的，其中所处理的字幕、影片截图没有授权，只在必要时，作为著作进行引用。

<!-- more -->

## .srt

一些影视DVD的光盘文件，有一定几率掉落母语版字幕，它通常有正确的时间轴。2014年的西班牙剧情、悬疑题材电影《沼泽地》，它的西语版.srt（SubRip Text）文件，通篇带有一些电影脚本内容——包含背景声、音效、人物演出等注释。这些通常是之后会剔除的部分，例如在它的英译版字幕里。

留下这些内容…这里挪用影片中的一个角色事件「无证狩猎」——先去拿到几个关键道具：电子词典浏览器[GoldenDict](http://goldendict.org)、网页浏览器插件[沙拉查词](https://saladict.crimx.com)、Python包[autosub](https://github.com/agermanidis/autosub)、Rust包[alass-cli](https://crates.io/crates/alass-cli)，便开始寻迹和偷猎。

## 翻译

主要步骤：
1. 播放媒体文件，逐句对照字幕文件
2. 使用在线翻译服务[reverso.net](https://www.reverso.net)快速输出整句意思，以及片段意思
3. 使用GoldenDict查找单词的详义。这里用的是西班牙语-英语词典，所以还需使用沙拉查词作英译中介
4. 基于母语的读写能力，从译意组织译文

### Scoop

如果网络条件较好，可参考[Scoop官网](https://scoop.sh)，先安装这个软件包管理器。

1. `Win`+`Q` → Windows PowerShell → 右键 → 以管理员身份运行
2. `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`
3. `Invoke-Expression (New-Object System.Net.WebClient).DownloadString('https://get.scoop.sh')`
4. （可选）安装一个下载工具，`scoop install aria2`

使用时如遇报错，可复制报错的信息，去此[问题板块](https://github.com/lukesampson/scoop/issues)自查。若需卸载，`scoop uninstall scoop`。

### GoldenDict

!!! repotemplate "「便携版」 2022-09"
    
    通过Scoop安装的该版本，在菜单项「编辑 → 词典」处，仅有固定的词典存放位置。可使用[linkshellextension](https://schinagl.priv.at/nt/hardlinkshellext/hardlinkshellext.html)，选择词典文件所在的文件夹，创建目录链接到该位置，然后「重新扫描」。

1. `scoop install goldendict`，或者到[GoldenDict官网](http://goldendict.org/download.php)下载32-bit Windows installer这个安装包
2. 下载词典。这里在[某个论坛](https://www.pdawiki.com/forum)新注册账号后，从[该帖](https://www.pdawiki.com/forum/forum.php?mod=viewthread&tid=38700)里的链接，下载了Oxford Spanish English（牛津西语-英语词典）这个文件夹——其中的.mdx（Mdict词典文件）就是所需的
3. 导入词典。`Win`+`Q` → Goldendict → 编辑 → 词典 → 词典来源 → 添加 → 选择文件夹 → 应用 → 等待扫描完成
4. 回到软件的主窗口，便可在导航栏搜索词汇

![](goldendict.png)

!!! repotemplate "「Next Generation」 2023-07"
    
    也可以试试这个版本，[GoldenDict-ng](https://github.com/xiaoyifang/goldendict-ng)。

GoldenDict默认会启用所有词典。当你有多本词典时，就可以给词典编组，以便筛选语种。

1. 编辑 → 词典 → 词典来源 → 添加 → 选择词典文件夹们所在的文件夹 → 递归扫描（On）
2. 编辑 → 词典 → 群组 → 添加群组
3. 将左栏的可用词典，拖进右栏的群组里 → 应用
4. 回到主窗口，下拉搜索框的左段，便可选择群组

### SalaDict

参考官网的[下载页](https://saladict.crimx.com/download.html)，根据使用的浏览器进行安装。

按需设置[浏览器外划词](https://saladict.crimx.com/native.html)，这里以Chrome为例：
1. Chrome → 设置 → 高级 → 系统 → 关闭Google Chrome后继续运行后台应用（On）
2. 在浏览器内划词，唤出查词窗口 → 打开设置
   ![](saladict.png)
3. 基本选项 → 后台保持运行（On）
4. 权限管理 → 读取剪切板（On），写入剪切板（On）
5. 隐私设置 → 设置快捷键 → 沙拉查词 在独立窗口中搜索剪贴板内容 → `Alt`+`Z`，全局（这里的键位可按需设置）
6. 在浏览器外划词，`Ctrl`+`C`，再`Alt`+`Z`，便可以唤出独立查词窗口
7. 如要关闭该窗口——可用Windows快捷键`Alt`+`Esc`切换到上一次操作的窗口

## 校对（上）

另外的主要问题是校对。如果有已校版本，会省去不少麻烦（也就没这个章节）。在这个.srt文件的末尾，可看到一条注释：

```
981
01:36:10,100 --> 01:37:00,100
Subtitulado por quetzalcoatl67
para subadictos
```

这两行分别是：上传者的ID，来源或分发的地址。找过去，在[该帖](https://www.subadictos.net/foros/showthread.php?t=36623)下，上传者提到：
- 使用了Subtitle Edit对.sup（Subtitle Presentation）文件进行OCR（光学字符识别），制作了.srt
- 将一些安达卢西亚（Andaluz）方言转写成了官方西班牙语卡斯蒂利亚（Castellano）语，以便于理解
- 删除了一些音画不同步的提示音乐的字行

这也解释了校对途中为什么较频繁遇到这些错误：

| 类型 | 示例 | 行号 | 处理 |
| - | - | - | - |
| 错别字  | Citroén → Citroen | 27，149 | 查找全部并替换 |
| 听写错误 | ~~Déjate~~ de tonterías | 622 | 用STT（Speech-to-Text）工具检查 |
| 听写错误 | ah, si. → ah, si? | 934 | 参考[电影脚本的PDF文件](https://aprendercine.com/descargar-guiones-de-cine-gratis/#:~:text=la%20isla%20minima) |
| 文本缺失 |   | 646-647 | 参考英语版字幕 |
| 语法错误 |   |   | 忽略 |

其中与STT有关的步骤是：
1. 使用媒体播放器[MPV](https://mpv.io)设置A-B点循环
2. 用视频录制、直播软件[OBS](https://obsproject.com)，录制视频片段或者音频片段
3. 使用命令行工具autosub识别片段，输出字幕文件

### MPV和OBS

`scoop install mpv`，或者[下载安装包](https://mpv.io/installation)。

`scoop install obs-studio`，或者[下载安装包](https://obsproject.com/download)。

1. `Win`+`Q` → MPV → 拖入媒体文件
2. 在MPV播放时，按`L`设置A点，再按`L`设置B点，并开始循环。（按第三下`L`则清除A-B循环）
3. `Win`+`Q` → OBS → 来源 → `加号` → 显示器采集。（或者选择音频输入采集）
4. 控件 → 开始录制。（录制时可循环播放多次）
5. 停止录制。会得到录像文件，以日期和时间命名

用AMD显卡的用户（曾经是）可参考[这个视频](https://www.youtube.com/watch?v=YaBdOHsg-4I)，设置OBS录制参数：
- 输出
    - 录像
        - 录像格式 → mp4
        - 编码器 → H264/AVC
        - 质量预设 → 质量
        - 速率控制方法 → 恒定QP（CQP）
        - I帧QP/P帧QP → 15~17
    - 音频
        - 音频比特率 → 320
- 视频
    - 常用FPS值 → 48

NVIDIA显卡上的参数，应该也可以参考这个。

### CMD和Cmder

命令提示符（Command Prompt），也就是可执行文件CMD.exe，是Windows系统默认的命令行解释器（Command-line Interpreter）。后者所解释的语境，通常是给定的操作系统或编程语言中的一种——这一类连接用户与系统内核、解释和执行指令、返回结果的，通常称为shell（壳层或壳）。

[Cmder](https://cmder.app)，自称是Windows上最好的shell，由提供命令行界面（CLI，Command-Line Interface）的[Clink](https://chrisant996.github.io/clink/)和提供图形用户界面（GUI，Graphical User Interface）的模拟终端[ConEmu](https://conemu.github.io)组合而成。

!!! repotemplate "「壳中壳」 2022-07"
    
    [Tabby](https://tabby.sh)，作为Cmder的「壳」，安装[Starship](https://starship.rs)并配置后，可以使它看着像原来的Cmder。启动更慢，功能更多。我需要它的窗口相关的功能。

因为Cmder需要进行配置，这里就以应急物品CMD为例。

### autosub

先安装Python语言包。去[Python官网](https://www.python.org/downloads)下载稳定版39版本的安装包，安装时一直选「下一步」即可。或者：
1. `Win`+`Q` → 命令提示符
2. `scoop bucket add versions`，参考[ScoopInstaller/Versions](https://github.com/ScoopInstaller/Versions)
3. `scoop install python39`

安装后可测试命令，`pip3`。如果提示「…不是内部或外部命令，也不是可运行的程序或批处理文件」，可尝试刷新系统环境`refreshenv`。或者重启电脑。重启后：
1. 根据[此仓库](https://github.com/jiaox99/autosub)的自述，安装依赖项，`scoop install ffmpeg`
2. `pip3 install autosub3`

进行语音识别前，可能需要改变CMD工作的目录，切换到之前的录像文件所在的文件夹：
1. 先切换所在的硬盘，`盘符:`。例如切换到E盘，即`E:`
2. 再切换所在的文件夹，`cd 文件夹`。例如切换到文件夹A的文件夹B里，即`cd E:/A/B`。文件夹名不要有空格

识别语音到字幕文件，`autosub -S 源语言 -D 目标语言 -o output.srt 媒体文件`。此处的语言代码可在[ISO 639-1](https://datahub.io/core/language-codes/r/0.html)（国际标准化组织639号标准第1部分）中查找到。

注意，在命令中，被提到的文件，默认已经在系统环境（在安装时）设置好了，或者在当前文件夹下。上面这个命令，也是指`盘符:/某个文件夹/autosub 其他参数 -o 当前文件夹/output.srt`

连接（concatenate）一下文件，`cat output.srt`。

![](autosub.png)

## 时间轴

### alass-cli

参考[此仓库](https://github.com/kaegi/alass)自述，可以去到它的[发行](https://github.com/kaegi/alass/releases)栏目，查找构建好的可执行文件。下载后，可在CMD中使用。

以下涉及另一种方法，需要Rust和它的包管理器Cargo。先安装Rust语言包。可能会提示需要`C++ build tools`：
1. `scoop install vcredist2019`。或者到[此页面](https://visualstudio.microsoft.com/zh-hans/downloads)下载Visual Studio 2019生成工具并安装
2. `Win`+`Q` → Visual Studio Installer → 已安装 → Visual Studio 2019生成工具 → 修改
3. 工作负荷 → 桌面应用和移动应用 → 使用C++的桌面开发：
    - MSVC v142 - VS2019 C++… （On）
    - Windows 10 SDK… （On）（[参考该问答贴](https://stackoverflow.com/questions/40504552/how-to-install-visual-c-build-tools)）
4. 重启电脑

`scoop install rustup`，或者从[此处](https://rustup.rs)下载rustup-init.exe并运行。

如遇到网络问题，提示「could not download file from…」，可参考[此文档](https://mirrors.gitcode.host/zzy/rust-crate-guide/3-env/3.1-rust-toolchain-cn.html)，执行：
1. `set RUSTUP_DIST_SERVER=https://mirrors.sjtug.sjtu.edu.cn/rust-static`
2. `set RUSTUP_UPDATE_ROOT=https://mirrors.sjtug.sjtu.edu.cn/rust-static/rustup`
3. 运行rustup-init.exe，一路默认即可

若需卸载，`rustup self uninstall`。

然后，`cargo install alass-cli`。

这里如果提示与`gcc`相关的错误，可以先`scoop install mingw-winlibs`，或去到官网[winlibs](https://winlibs.com/)下载和安装。有可能能够解决。

进行自动较轴，`alass-cli 媒体文件 源字幕 目标字幕`

![](alass.png)

在仓库所有者的[学士论文](https://github.com/kaegi/alass/blob/master/documentation/thesis.pdf)里，他说明该工具的工作原理是基于语音活动检测（Voice-Activity-Detectio）。这也提示了有更好书写规范的字幕文件所要去考虑的，以利于加工、后期作业和自动化。

## 校对（下）

来到了一般称作「二校」「三校」的阶段，差不多就是多次检查。这里列举部分段落：

![](La.isla.minima.2014.mkv_snapshot_00.03.26.png)

!!! repotemplate "「feria」 2023-04"
    
    应该可对应英语单词fair。词典中的解释是：州、县、乡村的集市或活动，会有商品、动物和娱乐、游戏、比赛。可译成「集会」、「市场」。

00:03:06处，文字注释feria译成「交游会」。来自一种早年在小地方举办的，供居民交易、游玩的季节性集会，当地村民乡人对此的称呼「交流会」，可能类似大都会的嘉年华。

![](La.isla.minima.2014.mkv_snapshot_00.03.51.png)

00:05:42至00:06:62，两位主人翁在玩打气枪。这里看机器翻译和英语字幕，不能去确定胡安说「现在不是招惹军人的时候」和佩德罗说到「那个法西斯份子」，都说哪件事情，和什么人。

此前，他们在旅店落脚时，在房间墙上见到了一把纳粹十字架——这么一来，佩德罗有可能因此「给报社寄稿」，举报了可能已退役的男掌柜——但事实上，他应该不会在初到小镇的头晚、晚饭与打枪的间隙，找到邮局去寄信。

此处确实是在交代佩德罗的人物背景，见该篇[报告](https://www.researchgate.net/publication/303856886_La_Isla_Minima_un_relato_sobre_la_identidad_andaluza)4.1.处。

![](La.isla.minima.2014.mkv_snapshot_01.07.58.png)

00:09:16处，胡安问「这是什么（酒）」，罗西奥回答「cangrejos de rio（河）」。这个词从电子辞典或搜索引擎检索出来是「小龙虾」「淡水鳌虾」，el cangrejo通常是「螃蟹」。影片01:35:56处，再次提到了fábrica（工厂）de cangrejos，而在01:07:57附近，画面中出现了一只龙虾。

![](La.isla.minima.2014.mkv_snapshot_00.16.09.png)

00:16:08处，涂鸦的人名Franco，在大部分在线字典和翻译器里对应「佛朗哥」。这里参考卡斯蒂利亚发音，与Francis/Frances[弗朗西斯]、Francine[弗朗西尼]、Francisco[弗朗西斯科]、Francesca[弗兰西斯卡]等人名音译保持一致，把Franco对应「弗朗哥」。

## .ass

字幕的主体「对白」，和画中文字、音响、演出等「注释」，它们的时间轴有重叠，就需要制成.ass（Advanced SubStation Alpha）等格式来保存。虽然在MPV里，可以同时载入多个.srt字幕。

之后涉及到文本编辑器[Sublime Text](https://www.sublimetext.com)、字幕编辑器[Subtitle Edit](https://nikse.dk/SubtitleEdit)、特效字幕编辑器[Aegisub](https://aegi.vmoe.info) 里的使用，这里就不赘述具体操作。

## thumbnail.jpg

在经过短或长的季节后，你就会得到最终的.ass文件。

额外的，用媒体缩略图工具[mt](https://github.com/mutschler/mt)或[mtn](https://gitlab.com/movie_thumbnailer/mtn/-/wikis/home)来生成（剥皮）一张4x4格的图：
1. 在`mt`的[发行栏](https://github.com/mutschler/mt/releases)，下载相关的Windows发行包，一般来说是`mt-版本号-x86_64-w64-mingw32.exe.tar.gz`，解压后运行`.exe`
2. `mt.exe -n 16 -c 4 --disable-timestamps --header=false 媒体文件`

![](La.isla.minima.2014.mt.jpg)

或者：
1. `scoop install mtn`
2. `mtn -c 4 -r 4 -g 3 -k 000000 -w 1920 -i -t -D 4 -P -o _mtn.png 媒体文件`

## 流程事件一览

### 特殊双语字幕の获得

1. 使用机器辅助翻译，但保留机器风格，（尽量）不做润色或修饰
2. 对拟声等同一类的句型采用全文一致的处理。例如出现复读时，字幕只写两组
3. 在标点处拆分句子，除非时间轴重叠或很靠近
4. 拆分对话，除非时间轴重叠或很靠近
5. 时间轴的宽度对齐音频，不在前后延长
6. 译文句子的语序对齐原文、音轨

### 机翻双语字幕の获得

1. 获得字幕源：
    - 从[Open Subtitles](https://www.opensubtitles.com)等大型字幕数据库搜索和下载字幕
    - 如果有文本字幕如.srt，在[FFmpeg Batch AV Converter](https://github.com/eibol/ffmpeg_batch)打开视频源，选择流多路复用功能，保存字幕。使用`Subtitle Edit`打开后，另存为`ass`字幕
    - 如果有图形字幕如.sub和.idx，在[Subtitle Edit](https://nikse.dk/subtitleedit)打开视频源，进行OCR扫描。保存字幕，选择`ass`格式
    - 如果是没有字幕轨、有英语音轨的情况，可以尝试用[whisper-ctranslate2](https://github.com/Softcatala/whisper-ctranslate2)来转写
2. 预处理：
    - 用[Sublime Text](https://www.sublimetext.com)打开.ass，批量选择和替换换行符`\N`为空格
3. 机翻：
    - 用[Tern](https://github.com/1c7/Translate-Subtitle-File)翻译字幕文件为双语字幕
4. 编辑：
    1. 用Sublime Text打开.ass，批量选择、剪切中文文本
    2. 打开一个新窗口，复制文本，按自己喜好编辑中文字幕的风格。编辑完之后，批量选择每行，剪切、粘贴回双语字幕文件
    3. 找到注释的所在的行，移到最后

### 其他事件

|date|tools|staff|link|note|
|:-|:-|:-|:-|:-|
|2023.05|Tern|机翻、后期|千禧年三部曲 (2009)：[1](https://github.com/scillidan/subtitle/blob/main/The%20Girl%20with%20the%20Dragon%20Tattoo%20(2009).chs.swe.srt)、[2](https://github.com/scillidan/subtitle/blob/main/The%20Girl%20Who%20Played%20with%20Fire%20(2009)%20Part%2001.chs.eng.srt)、[3](https://github.com/scillidan/subtitle/blob/main/The%20Girl%20Who%20Kicked%20the%20Hornet's%20Nest%20(2009).chs.eng.srt)|[.info](https://github.com/scillidan/subtitle/blob/main/The%20Girl%20with%20the%20Dragon%20Tattoo%20(2009).chs.swe.info)|
|2022.01|/|后后期|[平家物語 [The Heike Story][平家物语] (2021)](https://github.com/scillidan/subtitle/tree/main/The%20Heike%20Story%20(2021)/S01)|/|
|2021.09|DeepL|机翻、粗校|[Rosy (2018)](https://github.com/scillidan/subtitle/blob/main/Rosy%20(2018).chs.eng.srt)|[.info](https://github.com/scillidan/subtitle/blob/main/Rosy%20(2018).chs.eng.info)|
|2021.09|沙拉查词、GoldenDict|校对|[The Green Knight [绿骑士] (2021)](https://github.com/scillidan/subtitle/blob/main/The%20Green%20Knight%20(2021).chs.eng.srt)|[.info](https://github.com/scillidan/subtitle/blob/main/The%20Green%20Knight%20(2021).chs.eng.info)|
|2021.08|Reverso、GoldenDict、沙拉查词|翻译、校对、后期|[La isla minima [Marshland][沼泽地] (2014)](https://github.com/scillidan/subtitle/blob/main/Marshland%20(2014).chs.esp%5Bfull%5D.ass)|[.info](https://github.com/scillidan/subtitle/blob/main/Marshland%20(2014).chs.esp.info)|
|2019.12|/|校对、后期|[Lolita [洛丽塔] (1997)](https://github.com/scillidan/subtitle/blob/main/Lolita%20(1997).chs.eng.srt)|/|
|2019.01|谷歌翻译|翻译|词条: [Sibyl [西比拉]](https://zh.wikipedia.org/wiki/%E8%A5%BF%E6%AF%94%E6%8B%89)|/|
|2018.03|谷歌翻译|翻译|[Das Herz ist ein dunkler Wald [The Heart is a Dark Forest][心是一片暗林] (2007)](https://github.com/scillidan/subtitle/blob/main/The%20Heart%20is%20a%20Dark%20Forest%20(2007).chs.eng.srt)| [.info](https://github.com/scillidan/subtitle/blob/main/The%20Heart%20is%20a%20Dark%20Forest%20(2007).chs.eng.info)

## 其他道具一览

出现过的道具、物品。

1. [ ] 翻译记忆工具[Omegat](https://omegat.org)：并没有使用到。据介绍，它建立记忆库来辅助人工翻译，适合于「大型文档、专业术语、衍生案、修订、更新、多语言、共同作业」等。其记忆库文件应该还可兼容其他的软件，比如本地化工具[Poedit](https://poeditor.com)等。
2. [x] 截图工具[Snipaste PRO](https://docs.snipaste.com/zh-cn/pro)：截取软件应用的窗口。部分截图在写文字步骤时作参考，选一张图用于展示工具的主界面。
3. [ ] 图片压缩工具[pngquant](https://pngquant.org)和[guetzli](https://github.com/google/guetzli)：用来代替需要调用[tinypng](https://tinypng.com)的API（Application Programming Interface），也就是需要联网的[tinifier](https://github.com/tarampampam/tinifier)。
4. [x] 文本编辑器[Obsidian](https://obsidian.md)：以轻量级标记语言.md（Markdown）文本格式编辑了这篇文，并且用内置的功能，渲染和导出了.pdf（Portable Document Format）文件，作为并未使用的额外的附件。
    1. [ ] PDF浏览器[xpdfreader](https://www.xpdfreader.com/download.html)：用来检视.pdf。
    2. [ ] Golang包[](https://github.com/FiloSottile/mkcert)：用来生成自信任的为PCCS（Public-Key Cryptography Standards）#12格式、扩展名为.p12或.pfx的证书文件证书。
    3. [ ] Java应用[BatchPDFSign](https://github.com/jmarxuach/BatchPDFSign)：使用.pfx给.pdf写入自签名数字ID，进行数字签名（Digital Signature）。
    4. [ ] Golang包[markpdf](https://github.com/ajaxray/markpdf)：给.pdf的每页添加水印。这里用来铺一层很浅的装饰底纹。
5. [x] Rust包[lychee](https://github.com/lycheeverse/lychee): 排查文本里的链接的情况。可使用它的流程脚本[lychee-broken-link-checker](https://github.com/marketplace/actions/lychee-broken-link-checker)。
