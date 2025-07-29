## 词典脚本和服务

### DICT.org

[DICT.org](https://dict.org/bin/Dict)是一个多语言、多词典在线词典平台。

GoldenDict → 编辑 → 词典 → 词典来源 → 词典服务器 → 添加：

```text
已启用 On
名称 `dict.org`
地址 `dict://dict.org`
数据库 `*` # 启用所有的词典
策略 `*`

```

### dictd.service

参考[dictd.md](https://scillidan.github.io/notes/srv/dictd.html)部署自用`dictd`服务，笔记中使用的是运行[Ubuntu Server for ARM 22.04](https://cdimage.ubuntu.com/releases/jammy/release/)的树莓派4（CM4）。`dictd`需要`.dz`等格式文件，推荐两种方法：

1. 使用[PyGlossary](https://github.com/ilius/pyglossary)转换`StarDict (.ifo)`格式到`DICT.org file format (.index)`格式。StarDict的`.ifo`文件里最好不要有中文。
1. 参考[dict-ecdict.md](https://scillidan.github.io/notes/bin/dict-ecdict.html)来制作，这里使用到了[dictzip](https://dictzip.github.io/)或者[dictzip for Windows 10 (x64)](https://github.com/KaseyJenkins/dictzip-win64)。

词典 → 词典来源 → 词典服务器 → 添加：

```sh
已启用 On
名称 `dictd`
地址 `dict://<your_host>:2628`
数据库 `*`
策略 `*`

```

### [Cambridge-Dictionary](https://github.com/spignelon/cambridge-dictionary)

> Once you search for the word and gets the meaning, it saves it into a local database from which it retrieves them if you search for the same word again in the future instead of fetching it from the server. This makes it quick, run even when there’s no internet connection (assuming that your local database is of substantial size) and prevents making too many queries to the server.\
> “一旦您搜索该单词并获得其含义，它就会将其保存到本地数据库中，如果您将来再次搜索同一单词，而不是从服务器获取它，它就会从该数据库中检索它们。这使得它快速运行，即使没有互联网连接（假设您的本地数据库规模很大），并防止向服务器进行太多查询。”

```sh
git clone --depth=1 https://github.com/spignelon/cambridge-dictionary
cd cambridge-dictionary
python -m venv venv
venv\Scripts\activate.bat
pip install requests bs4

```

词典 → 词典来源 → 程序 → 添加：

```text
已启用 On
类型 纯文本
名称 `Cambridge-Dictionary`
命令行 `<path_to>\cambridge-dictionary\venv\Scripts\python.exe <path_to>\cambridge-dictionary\cambridge.py "%GDWORD%"`

```

## 翻译脚本和服务

### deep-translator

[deep-translator](https://github.com/nidhaloff/deep-translator)是一个支持了多引擎的翻译脚本工具。如何申请或创建API请看各翻译器的官网介绍。我一般使用腾讯云机翻API，写文时，需要从源码安装这个工具，才能使用这个翻译器：

```sh
git clone --depth=1 https://github.com/nidhaloff/deep-translator
cd deep-translator
pip install -e .

```

设置系统环境变量，可以使用环境变量编辑器[Rapid Environment Editor](https://www.rapidee.com/en/about)：

1. RapidEE → 用户变量 → 右键 → 添加环境变量 → 变量名称 `TENCENT_SECRET_ID` → 填写你的`SecretId`
1. 添加环境变量 → 变量名称 `TENCENT_SECRET_KEY` → 填写你的`SecretKey`

```sh
deep-translator --translator tencent --source "en" --target "zh" --text "Golden Apple"

```

有些翻译器，支持检查`源语言`，然后都需要指明`目标语言`。但我需要「中英文互译」，有一个可行的方法是：在GoldenDict中添加两条脚本设置，其中一条的输入语言为`en`，目标语言为`zh`。

词典 → 词典来源 → 程序 → 添加：

```text
类型 纯文本
名称 `deep-translator en2zh`
命令行 `deep-translator --translator tencent --source "en" --target "zh" --text "%GDWORD%"`

```

另一条则将两者互换：

```text
名称 `deep-translator zh2en`
命令行 `deep-translator --translator tencent --target "zh" --source "en" --text "%GDWORD%"`

```

### TencentTrans_22.py

这是我常用的一个脚本，主要用于将多语言（主要是英语）翻译到中文，中文翻译到英文。脚本由[tencent-translate-for-goldendict](https://github.com/LexsionLee/tencent-translate-for-goldendict)修改而来，即「用于Goldendict的腾讯云翻译」，后者的翻译API支持[多门外语](https://cloud.tencent.com/document/api/551/15620)，文本翻译一项上，[每月有500万字符免费额度](https://cloud.tencent.com/document/product/551/35017)，重度使用也完全够用。我在[字幕机翻](https://github.com/1c7/Translate-Subtitle-File)、[沉浸式翻译](https://immersivetranslate.com/zh-Hans/)、[Translate Shell](https://github.com/soimort/translate-shell)里也都使用过这个API。

首先，参考[申请翻译API](https://github.com/LexsionLee/tencent-translate-for-goldendict#%E7%94%B3%E8%AF%B7%E7%BF%BB%E8%AF%91api)，以及[官方介绍](https://cloud.tencent.com/product/tmt)去获得API。

```sh
mkdir tencenttrans_2zh_zh2en
cd tencenttrans_2zh_zh2en
python -m venv venv
venv\Scripts\activate.bat

```

下载[tencenttrans_2zh_zh2en.py](https://gist.github.com/scillidan/e95773454d79dc047aeed016fb00daef)并编辑，填写你的翻译API的`SecretId`和`SecretKey`。

```sh
pip install tencentcloud-sdk-python langid
python tencenttrans_2zh_zh2en.py "golden apple"

```

GoldenDict → 字典 → 程序 → 添加：

```text
类型 纯文本
名称 `tencenttrans_2zh_zh2en`
命令行 `<path_to>\tencenttrans_2zh_zh2en\venv\Scripts\python.exe <path_to>\tencenttrans_2zh_zh2en\tencenttrans_2zh_zh2en.py "%GDWORD%"`

```

虽然有不少不足，如：

- 输出的文本没有段落，因为[GoldenDict会删除换行符](https://github.com/goldendict/goldendict/issues/1606)
- 未知的Bug，例如在GoldenDict里输出某些语言时，翻译文字前面会显示一串报错

截止文章写成，我已使用了近一年。这之前，我使用的是[沙拉查词](https://saladict.crimx.com/)。

### LibreTranslate

[LibreTranslate](https://github.com/LibreTranslate/LibreTranslate)是一个开源、有离线功能的机器翻译API。它被设计用于本地托管，允许用户在不依赖其他外部服务的情况下进行翻译，安装简单，高效经济。可作为一种备用。

可参考[libretranslate.md](https://scillidan.github.io/notes/srv/libretranslate.html)部署到家用服务器，并参考[libretrans.md](https://scillidan.github.io/notes/bin/libretrans.html)来使用。它在Windows版的GoldenDict里有一些字符问题，在Linux里可正常使用。

## 分词、断句脚本

### gd-mecab（仅gd-ng）

[GoldenDict tools](https://github.com/Ajatt-Tools/gd-tools)是一套GoldenDict-ng的增强脚本集，主要用于日语学习。它的[gd-marisa](https://github.com/Ajatt-Tools/gd-tools#gd-marisa)、[gd-mecab](https://github.com/Ajatt-Tools/gd-tools#gd-mecab)脚本，可以置顶句子、分词、断句。似乎也能用于中文，但并没有实际的「中文分词」的功能，可以用「划词再右键」来代替。

参考[issue #18](https://github.com/Ajatt-Tools/gd-tools/issues/18)下载`gd-tools_windows.zip`，解压后运行安装包。出于一个好习惯，当安装文件（非便携的）没有从[Github](https://github.com/)（虽然有时候文件也会报毒）、[SourceForge](https://sourceforge.net/)等可信网站提供时，建议上传到[VirusTotal](https://www.virustotal.com)，进行Hash检查。虽然这个安装包有几个红色警告，但应该（或许）可以忽略，或者在类似于[HiBit Uninstaller](https://www.hibitsoft.ir/Uninstaller.html)的「安装监视程序」模式下，进行安装。

程序 → 添加：

```text
类型 Html
名称 `gd-marisa`
命令行 `<path_to>\Ajatt-tools\gd-tools\gd-tools.exe marisa --word %GDWORD% --sentence %GDSEARCH% --path-to-dic <path_to>\Ajatt-tools\gd-tools\marisa_words.dic`

```

### [Sentences](https://github.com/neurosnap/sentences)

> This command line utility will convert a blob of text into a list of sentences.

“此命令行实用程序将文本块转换为句子列表。”

```sh
pnpm add -g echo-cli
# Test on go1.20.1
go get github.com/neurosnap/sentences
go install github.com/neurosnap/sentences/cmd/sentences@latest

```

程序 → 添加：

```text
类型 纯文本
名称 `Sentences`
命令行 `C:\Users\User\AppData\Roaming\pnpm\echo-cli.CMD "%GDWORD%" | sentences.exe`

```

## 语法检查脚本

### LanguageTool

[LanguageTool](https://languagetool.org/)是一个开源的多语言的拼写、语法、风格检查工具。在它的浏览器插件里，可切换服务源，从「云服务」切换到「本地服务」。作为服务应用，Java有很好的兼容性，但效能可能不是特别出色。

参考[languagetool.md](https://scillidan.github.io/notes/srv/languagetool.html)来部署`LanguageTool`服务。

### pyLanguagetool

还会有一些不在浏览器里编辑或者划句的情况。我尝试了在GoldenDict里对句子做语法检查。但这种方法不是很直观，而且在Windows的GoldenDict里去使用一些命令行工具，可能会遇到[字符相关问题](https://github.com/Twinblade-i/goldendict-openai-translator?tab=readme-ov-file#preface)。

[pyLanguagetool](https://github.com/Findus23/pyLanguagetool)是一个Python库和命令行工具，使用LanguageTool的[JSON API](https://languagetool.org/http-api/swagger-ui/#/default)。

```sh
uv venv .pyLanguagetool
.pyLanguagetool\Scripts\activate.bat
uv pip install --upgrade setuptools beautifulsoup4 pyLanguagetool
echo "This are a exampl" | pylanguagetool --lang en-US
echo "This are a exampl" | pylanguagetool --api-url http://<your_host>:8040/v2/ --input-type html --no-color --lang en-US

```

目前，我使用的是一个`check.bat`脚本：

```text
@echo off

C:\Users\User\Usr\Script\.pyLanguagetool\Scripts\pylanguagetool.exe --api-url http://<your_host>:8040/v2/ --input-type html --lang en-US -c

pause

```

和一个开机启动的[AutoHotkey](https://www.autohotkey.com/)脚本`user.ahk`：

```text
#NoEnv
SetWorkingDir %A_ScriptDir%
^!+c::Run "<path_to>/check.bat"

```

当我复制一段文本后按下`Ctrl+Shift+Alt+c`， 就会运行语法检查。

## 其他脚本

添加到「程序」里即可：

- 拼写提示[DoYouMean](https://github.com/hisbaan/didyoumean)
- 同义词词典[thes](https://github.com/grantshandy/thes)

### ety-python

[ety-python](https://github.com/jmsv/ety-python)是一个Python工具，能够打印词源的各历史时期的树状信息图，数据来自[Etymological Wordnet](http://www1.icsi.berkeley.edu/~demelo/etymwn/)项目，大部分信息是从[Wiktionary](https://en.wiktionary.org/wiki/Wiktionary:Main_Page)挖掘来。

```sh
pip install ety
ety -r -t apple

```

将输出：

```sh
apple (English)
└── appel (Middle English (1100-1500))
    └── æppel (Old English (ca. 450-1100))

```

在Windows 10的GoldenDict里使用`ety-python`时，我遇到了一些问题（见[issue #1678](https://github.com/goldendict/goldendict/issues/1678), [blog](https://isaacong.me/posts/unicodeencodeerror-when-redirecting-python-output/)）。虽然未解决，但是勉强能够使用。修改部分见[31a8be9](https://github.com/jmsv/ety-python/commit/31a8be922bf8402c430eb456591d729879a030ed)。

```sh
git clone --depth=1 https://github.com/scillidan/ety-python
cd ety-python
python -m venv venv
venv\Scripts\activate.bat
pip install -e .

```

程序 → 添加：

```text
类型 纯文本
名称 `ety-python`
命令行 `<path_to>\ety-python\venv\Scripts\python.exe <path_to>\ety-python\ety\__main__.py -r -t "%GDWORD%"`

```

Mark两个相关的Web应用：[etytree](https://github.com/agmmnn/etytree)、[jsetymology](https://github.com/myrriad/jsetymology)。

## 其他

### 另一种联动OCR取词

也是使用[GoldenDictOCR](https://github.com/VimWei/GoldenDictOCR)。在它「OCR取词」模式下，按`Ctrl+右键单击`可识别鼠标附近字符，按`Ctrl+反引号`可进行框选。

1. 安装OCR工具[Capture2Text](https://capture2text.sourceforge.net/)
1. 默认OCR语言为英语，可参考[Installing Additional OCR Languages](https://capture2text.sourceforge.net/#install_additional_languages)添加语言
1. 进入`GoldenDictOCR\IncludeAHK\`
1. 编辑`GdOcrTool.ahk`。补全`Capture2Text.exe`的完整路径
1. 按`Ctrl+Alt+O`开关一次「OCR取词」
1. 打开配置文件示例`Capture2Text.ini`，复制从`[BubbleCapture]`到`[Hotkey]`的内容
1. 编辑`C:\Users\User\AppData\Roaming\Capture2Text\Capture2Text.ini`，在文件末尾，粘贴复制的内容
1. 编辑以下两处，用于设置3个快捷键，来切换到对应的识别语言，如：

```text
[Hotkey]
Lang1=Shift+Alt+1
Lang2=Shift+Alt+2
Lang3=Shift+Alt+3
...
[OCR]
QuickAccessLang1=English
QuickAccessLang2=...
QuickAccessLang3=...

```

用来识别印刷体的单个英文词时，识别率还可以，稳定性略差。

2024-01-062024-09-24scillidan
