---
title: "配置GoldenDict·番外"
date:
    created: 2024-01-05
    updated: 2024-01-06
keywords: >
    GoldenDict, GoldenDict-ng, Setting, Dictionaries, Pronunciation, Morphology, Programs
locale: en
---

!!! read ""
  
    日期：2024-01-06  
    归属：[CC-BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/deed.zh-hans)

## 添加在线词典库

参考[GoldenDict++](https://github.com/nonwill/GoldenDict-OCR)的默认配置，可以添加[DICT.org](https://dict.org/bin/Dict)作为在线词典库。

GoldenDict → 编辑 → 词典 → 词典来源/词典服务器 → 添加：

```
已启用 On
名称 `dict.org`
地址 `dict://dict.org`
数据库 `*` # 启用所有的词典
策略 `*`
```

## 添加单词脚本

[ety-python](https://github.com/jmsv/ety-python)是一个Python工具，能够打印词源的各历史时期的树状信息图，数据来自[Etymological Wordnet](http://www1.icsi.berkeley.edu/~demelo/etymwn/)项目，大部分信息是从[Wiktionary](https://en.wiktionary.org/wiki/Wiktionary:Main_Page)挖掘来。

首先安装[Python](https://www.python.org/downloads/)，我装的是`python3.9`。安装过程中勾选「添加Python到PATH」，其他全部默认。

在终端中运行：

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

当在Windows 10的GoldenDict里使用`ety-python`时，我遇到了一些问题（见[issue #1678](https://github.com/goldendict/goldendict/issues/1678), [blog](https://isaacong.me/posts/unicodeencodeerror-when-redirecting-python-output/)）。最后虽然并未解决问题，但算是勉强能用了。使用了一个在隔离环境中的、有修改的`ety-python`：

```sh
git clone https://github.com/jmsv/ety-python
cd ety-python
python -m venv venv
venv/Scripts/activate.bat
pip install -e .
```

修改`ety/cli.py`，在开头的`import ety`下方，加上：

```py
import sys
```

注释掉末尾的`print(output.strip())`，加上：

```py
encoded_output = output.strip().encode("utf-16", errors="replace")
sys.stdout.buffer.write(encoded_output)
sys.stdout.buffer.flush()
```

GoldenDict → 编辑 → 词典 → 词典来源/程序 → 添加：

```
已启用 On
类型 纯文本
名称 `ety-python`
命令行 `...\ety-python\venv\Scripts\python.exe ...\ety-python\ety\__main__.py -r -t %GDWORD%`
```

也可以添加其他的脚本，例如：

- 拼写提示[DoYouMean](https://github.com/hisbaan/didyoumean)
- 同义词词典[thes](https://github.com/grantshandy/thes)
- 缩写查询[abbr-cli](https://github.com/mhadidg/abbr-cli)。不如原网页[All Acronyms](https://www.abbreviations.com/)靠谱

![](scripts.png)

## 添加多引擎翻译脚本

[deep-translator](https://github.com/nidhaloff/deep-translator)是一个支持了多引擎的翻译脚本工具。目前支持[DeepL](https://www.deepl.com)、[谷歌翻译](https://cloud.google.com/translate/docs)、[微软翻译](https://www.microsoft.com/zh-cn/translator/)、[腾讯翻译君](https://fanyi.qq.com/)、[百度翻译](https://api.fanyi.baidu.com/)等等翻译器。如何申请或创建API请看各翻译器官网介绍。

按照说明，需要添加API的两个变量值到系统环境中。以腾讯翻译为例，使用环境变量编辑器[Rapid Environment Editor](https://www.rapidee.com/en/about)来添加：

1. RapidEE → 用户变量 → 右键 → 添加环境变量 → 变量名称 `TENCENT_SECRET_ID` → 填写你的`SecretId`
2. ... 添加环境变量 → 变量名称 `TENCENT_SECRET_KEY` → 填写你的`SecretKey`

不怕Bug可以从源码安装：

```sh
git clone https://github.com/nidhaloff/deep-translator
cd deep-translator
pip install .
```

运行示例：

```sh
deep-translator --translator tencent --source "en" --target "zh" --text "Golden Apple"
```

有些翻译器支持检查`源语言`，然后都需要指明`目标语言`。例如我需要「中英文互译」的情况，有一个不太优雅但可行的方法是：在GoldenDict中添加两个脚本，其中一个的输入语言为`en`，目标语言为`zh`，另一个则将两者互换。

![](deep-translator.png)

## 添加专用翻译脚本

我目前的需求是：多语言（主要是英语）翻译到中文，中文翻译到英文，运行快而稳。所以我使用了特定翻译器的脚本，[用于Goldendict的腾讯云翻译](https://github.com/LexsionLee/tencent-translate-for-goldendict)。支持[多门外语](https://cloud.tencent.com/document/api/551/15620)，文本翻译一项上，[每月有500万字符免费额度](https://cloud.tencent.com/document/product/551/35017)，重度使用也完全够用。

1. 首先参考[申请翻译API](https://github.com/LexsionLee/tencent-translate-for-goldendict#%E7%94%B3%E8%AF%B7%E7%BF%BB%E8%AF%91api)说明，或者看[官方介绍](https://cloud.tencent.com/product/tmt)去获得API
2. 下载修改版的[TencentTrans_22.py](https://github.com/scillidan/tencent-translate-for-goldendict/blob/master/TencentTrans_22.py)，脚本中填写API的`SecretId`和`SecretKey`
3. `pip install tencentcloud-sdk-python langid`
4. ... 词典 → 词典来源/程序 → 添加：

```
已启用 On
类型 纯文本
名称 `TencentTrans-22`
命令行 `python ...\TencentTrans-22.py %GDWORD%`
```

![](tencenttrans_22.png)

不足之处有：

- 输出的文本没有段落，因为[GoldenDict会删除换行符](https://github.com/goldendict/goldendict/issues/1606)
- 翻译句子时的Bug，例如有些语言如韩语，当在GoldenDict中输出时，在翻译文字前会显示一串报错


## 添加分词脚本（仅gd-ng）

[GoldenDict tools](https://github.com/Ajatt-Tools/gd-tools)是一套GoldenDict-ng的增强脚本集，主要用于日语学习。它的[gd-marisa](https://github.com/Ajatt-Tools/gd-tools#gd-marisa)、[gd-mecab](https://github.com/Ajatt-Tools/gd-tools#gd-mecab)脚本，可以置顶句子、分词、断句。似乎也能用于中文，但并没有实际的「中文分词」的功能，可以用「划词再右键」来代替。

参考[issue #18](https://github.com/Ajatt-Tools/gd-tools/issues/18)下载`gd-tools_windows.zip`，解压后运行安装包。当安装包没有提供Hash值时，我习惯拖到[VirusTotal](https://www.virustotal.com)进行检查。虽然这个安装包有几个红色警告，但我在[HiBit Uninstaller](https://www.hibitsoft.ir/Uninstaller.html)的「安装监视程序」下，选择了任意一处位置进行安装。

GoldenDict-ng → 编辑 → 词典 → 词典来源/程序 → 添加：

```
已启用 On
类型 Html
名称 `gd-marisa`
命令行 `...\Ajatt-tools\gd-tools\gd-tools.exe marisa --word %GDWORD% --sentence %GDSEARCH% --path-to-dic ...\Ajatt-tools\gd-tools\marisa_words.dic`
```

![type:video](https://raw.githubusercontent.com/scillidan/YAFA-site/main/docs/assets/media/goldendict-expand/gd-marisa.mp4){ .skip-lightbox }


## 使用LanguageTool进行语法检查

[LanguageTool](https://languagetool.org/)是一个开源的多语言的拼写、语法、风格检查工具。在运行了它的本地服务应用后，可以去到它的浏览器插件中，切换服务源，从「云服务」切换到「本地服务」：

1. 安装[浏览器插件](https://languagetool.org/services#browsers)
2. 根据[说明](https://dev.languagetool.org/http-server)，下载`LanguageTool Desktop version`，解压缩
3. 安装[OpenJDK](https://jdk.java.net)，我使用的是[openjdk17](https://jdk.java.net/17/)，`scoop install openjdk17`
4. 终端中运行`java -cp ...\LanguageTool\languagetool-server.jar org.languagetool.server.HTTPServer --port 8081 --allow-origin`
5. 浏览器插件 → Settings → Advanced settings → Localserver (localhost)
6. ... General settings → Show in right-click menu (On)

![type:video](https://raw.githubusercontent.com/scillidan/YAFA-site/main/docs/assets/media/goldendict-expand/languagetool.mp4){ .skip-lightbox }

在一些场景，例如提交和回复Issues时，我需要使用简单的书面英语。虽然不进行检查，也有可能被理解意思，但我考虑在书写时用上语法检查。

本地版的LanguageTool插件，它似乎只方便于：在浏览器的「输入框」输入完后，进行检查。我不太想无论长短句都去到新窗口中处理。这里就用到[pyLanguagetool](https://github.com/Findus23/pyLanguagetool)，将它嵌入到GoldenDict来进行交互。

```sh
git clone https://github.com/Findus23/pyLanguagetool
cd pyLanguagetool
python39 -m venv venv
venv\Scripts\activate.bat
```

GoldenDict在处理一些字符时会出错。编辑 `pylanguagetool/cli.py`，修改:

```py
tick = colored(u"\u2713", Fore.LIGHTGREEN_EX) + " "
cross = colored(u"\u2717", Fore.LIGHTRED_EX) + " "
```

到:

```py
tick = colored(u"v", Fore.LIGHTGREEN_EX) + " "
cross = colored(u"x", Fore.LIGHTRED_EX) + " "
```

修改：

```py
    print(error["message"])
```

到:

```py
try:
    print(error["message"])
except UnicodeEncodeError:
    # Replace problematic character with an empty string
    print(error["message"].replace('\xa0', ''))
```

可以注释掉两个段落来隐藏一些提示信息：

```py
print(colored(
    "{} detected ({:.0f}% confidence)".format(language["detectedLanguage"]["name"],
                                              language["detectedLanguage"]["confidence"] * 100)
    , Fore.LIGHTBLACK_EX))
if language["detectedLanguage"]["code"] != language["code"]:
    print(colored(
        "checking as {} text because of setting".format(language["name"])
        , Fore.LIGHTBLACK_EX))
print()
```

```py
if explain_rule:
    col_len = max(len(d) for d, u in rule_explanations) + 1
    for descr, url in rule_explanations:
        print(descr + ":" + " " * (col_len - len(descr)) + url)
    print()

print(colored("Text checked by {url} ({version})".format(url=api_url, version=version), Fore.LIGHTBLACK_EX))
```

安装`pyLanguagetool`：

```sh
pip install -e .
```

我使用了[echo-cli](https://github.com/iamakulov/echo-cli)来代替`echo`命令，`pnpm add -g echo-cli`。

终端中测试，我指定了要检查的语言：

```sh
echo-cli "this is a exampl" | pylanguagetool --api-url http://localhost:8081/v2/ --input-type html --no-color --lang en-US
```

添加到GoldenDict里时需要写长点：

```
已启用 On
类型 纯文本
名称 `pyLanguagetool`
命令行 `C:\Users\YourName\AppData\Roaming\pnpm\echo-cli.CMD %GDWORD% | ...\gd_pyLanguagetool\venv\Scripts\pylanguagetool.exe --api-url http://localhost:8081/v2/ --input-type html --no-color --lang en-US`
```

![](pylanguagetool.png)

需要等宽字体才能对齐「字母」和「波浪线」。速度上不如插件。

作为本地服务，LanguageTool大概占用`500~600mb`内存，耗电量属非常低。在不使用「Windows计划任务」的情况下，开机后在后台运行LanguageTool服务，可以新建`srv_languagetool.cmd`:

```sh
...\java.exe -cp ...\LanguageTool\languagetool-server.jar org.languagetool.server.HTTPServer --port 8081 --allow-origin
```

和`srv_languagetool.vbs`，来隐藏运行窗口:

```sh
Set WshShell = CreateObject("WScript.Shell")
  WshShell.Run chr(34) & "...\srv_languagetool.cmd" & Chr(34), 0
Set WshShell = Nothing
```

右键`srv_languagetool.vbs`，新建快捷方式。将快捷方式，移动到`C:\Users\YourName\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\`。

目前，我一般集中地去处理（遗留的）Issues，所以我并未在后台常驻这个服务。

## 另一个语法检查脚本

[Gramformer](https://github.com/PrithivirajDamodaran/Gramformer)是一个Python库，可以用于检查句子的拼写、标点符号、语法或用词错误。曾经计划支持64个字符（包括空格和标点符号）以上长度的句子。虽然可以使用GPU模式，但是运行很慢。

```sh
git clone https://github.com/PrithivirajDamodaran/Gramformer
python39 -m venv venv
venv\Scripts\activate.bat
pip install -e .
```

先安装[CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit)。根据[PyTorch](https://pytorch.org/get-started/locally/)，我装了`CUDA 11.8.0`，下载特定版本的[torch](https://download.pytorch.org/whl/torch/)，再

```sh
pip install ...\torch-2.0.1+cu118-cp39-cp39-win_amd64.whl
```

参考[issue #31](https://github.com/PrithivirajDamodaran/Gramformer/issues/31):

```sh
python -m spacy download en
```

创建`cli.py`，详情参见[gramformer_cli.py](https://gist.github.com/scillidan/3fb5c4a9e2210e3974b599738e3dc5e5):

```py
import sys
import warnings
import torch
from transformers import AutoTokenizer, AutoModelForSeq2SeqLM
from gramformer import Gramformer

def set_seed(seed):
    torch.manual_seed(seed)
    if torch.cuda.is_available():
        torch.cuda.manual_seed_all(seed)

def main():
    if len(sys.argv) != 2:
        print("Usage: python cli.py INPUT")
        return

    set_seed(1212)

    warnings.filterwarnings("ignore", category=FutureWarning)

    use_gpu = torch.cuda.is_available()
    device = torch.device("cuda" if use_gpu else "cpu")
    gf = Gramformer(models=1, use_gpu=use_gpu)  # 1=corrector, 2=detector

    influent_sentence = sys.argv[1]
    corrected_sentences = gf.correct(influent_sentence, max_candidates=1)
    print("x ", influent_sentence)
    for corrected_sentence in corrected_sentences:
        print("v ", corrected_sentence)

    print("Device:", device)

if __name__ == "__main__":
    main()
```

可以修改`gramformer/gramformer.py`的：

```py
print("[Gramformer] Grammar error correct/highlight model loaded..")
```

来隐藏这段文字。将这行改为：

```py
print()
```

测试：

```sh
python cli.py "YourWriting"
```

... 词典 → 词典来源/程序 → 添加：

```
已启用 On
类型 纯文本
名称 `Gramformer`
命令行 `...\Gramformer\venv\Scripts\python.exe ...\Gramformer\cli.py %GDWORD%`
```

![](gramformer.png)

## 另一种联动OCR取词

也是使用[GoldenDictOCR](https://github.com/VimWei/GoldenDictOCR)，在它「OCR取词」模式下，按`Ctrl+右键单击`可识别鼠标附近字符，按`Ctrl+反引号`可进行框选。

先安装OCR工具[Capture2Text](https://capture2text.sourceforge.net/)。默认OCR语言为英语，可通过[Installing Additional OCR Languages](https://capture2text.sourceforge.net/#install_additional_languages)的说明添加额外的语言。

之后，进入`GoldenDictOCR`文件夹。编辑`IncludeAHK/GdOcrTool.ahk`。填写`Capture2Text.exe`所在的位置:

```
Global Capture2TextFileName := "...\Capture2Text.exe"
```

按`Ctrl+Alt+O`开关一次「OCR取词」。在`C:\Users\YourName\AppData\Roaming\Capture2Text`目录下，会生成配置文件`Capture2Text.ini`。

打开配置文件示例`IncludeAHK/Capture2Text.ini`，复制从`[BubbleCapture]`到`[Hotkey]`的内容，粘贴到前面的配置文件的末尾。再添加或编辑以下两处，用于设置3个快捷键，来切换OCR语言，如：

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