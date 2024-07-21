
## mdx

查找剩余的`</br>`, `</>`

替换（）到()
替换`\s\s`到`\s`

选择Tab，替换`\s`到

移除`中英文间空格`
替换`]\s`到`]`
替换`abbr. `到`abbr.`
替换`；`

`word`+`translation`

Ctrl 选择 word和t，右键，导出数据 → SQL ... 仅选定的列 ... 目录/预览 

```
mdict -x ecdict.mdx --exdb -d ./_db
```

```
// xsv select entry,paraphrase ecdict.csv > ecdict_word_translate.csv
word,translation
```

```
xsv split -s 300000 ./ ecdict.csv
xsv fmt --quote-always ecdict.csv > ecdict2.csv
```

` <\/br>\r\n`到` ¶`

好的方法是从源文件修改，再制作成mdx，但一般是先参与到其中，再考虑

```
119,"[电话] 火警 </br>
"
120s,"[电话] 急救中心 </br>
(120 的复数) </br>
`2``4`原型: 120s 是 120 的复数`2`
"
```

` </br>\n` → `none`单行的末尾字符加换行符
`"\,"` `none` 匹配有空格的word的行
`\n"` → `"` 剩余的末尾换行符到
`,"` → `\t`
`"\n` → `\n`
`\t\s` → `\t`
查找`\t` → Alt+Enter + Shift+End → Ctrl+h → `(?![\t])\s`
 按`A-f`开启选择全部`\t`
 match `\s` not `\t`

all to half
复数`2` 到 复数
`2``4`原型:
`2``4`...

## cb1
Download img from https://www.armbian.com/bigtreetech-cb1/.

↪ https://docs.armbian.com/User-Guide_Getting-Started/

```
User: root
Password: 1234
```

关闭窗口，以新用户登录

```sh
sudo apt update
sudo apt upgrade
```

```sh
sudo apt list --installed
```

连接Wifi

```sh
sudo nmcli dev wifi list
sudo nmcli dev wifi connect "SSID" password YourPassword
```

```sh
nmtui-connect SSID
```

拔掉网线。输入IP地址

## av1an

```sh
scoop install vapoursynth vmaf
py .../vsrepo.py update
py .../vsrepo.py install lsmas ffms2
scoop install nasm emscripten
emsdk install latest
emsdk activate latest
```

```sh
venv/Scripts/streamlit.exe run web_demo2.py
```

```sh
av1an -i %1 ^
	-v "--cpu-used=3 --end-usage=q --cq-level=30 --threads=8" ^
	-w 10 ^
	--target-quality 95 ^
	-a "-c:a libopus -b:a 192k -ac 2" -l _log_%1 -o _%1"
```

## Jupyter

```sh
pip install --user ipykernel
ipython kernel install
jupyter-lab
```

## spongebob-cli

```sh
git clone https://github.com/trakBan/spongebob-cli
cd spongebob-cli
python39 -m venv venv
venv/Scripts/activate.bat
python setup.py install
python spongebob-cli
```

## tldr

```sh
scoop install tldr
tldr -c
```

Create symbolic link from `C:/Users/YourName/AppData/Roaming/tldr/pages.en` to `C:/Users/YourName/.cache/tldr/pages.en`

## PDF.js

@pdf
文本的
这是口头的
漂白，漂洗
纸化
平铺直叙的
编目
编辑
同word一样是二进制文件

Tool:

- Pdfalyzer
- Tabula
- Sublime_Text
	- Insert_Nums
- pdf-toc

Attribution:

The pdf edit from [The Pinouts Book](https://pinouts.org) by [NODE](https://n-o-d-e.net/index.html) & Baptiste / [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

Date:
2022-08-01

https://pspdfkit.com/blog/2021/how-to-build-a-javascript-pdf-viewer-with-pdfjs/#embedding-the-pdf-js-viewer-in-an-html-window

Cache:

- PDF浏览器[xpdfreader](https://www.xpdfreader.com/download.html)：用来检视.pdf
- Golang包[mkcert](https://github.com/FiloSottile/mkcert)：用来生成自信任的为PCCS（Public-Key Cryptography Standards）\#12格式、扩展名为.p12或.pfx的证书文件证书
- Java应用[BatchPDFSign](https://github.com/jmarxuach/BatchPDFSign)：使用.pfx给.pdf写入自签名数字ID，进行数字签名（Digital Signature）
- Golang包[markpdf](https://github.com/ajaxray/markpdf)：给.pdf的每页添加水印。这里用来铺一层很浅的装饰底纹

