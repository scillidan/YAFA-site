
## Head

preview
environment
install
configuration
keymap
troubleshoot

title
url
  - description
tag
groupx2
## clip


### Reference

1. https://vladar.bearblog.dev/public-domain-art-preparation-monochrome/
2. https://vladar.bearblog.dev/public-domain-art-preparation-in-color/


## startfetch

### Reference: 

1. https://en.wikipedia.org/wiki/Deneb
2. https://en.wikipedia.org/wiki/Altair
3. https://en.wikipedia.org/wiki/Vega
4. https://en.wikipedia.org/wiki/Right_ascension
5. https://en.wikipedia.org/wiki/Declination
6. https://en.wikipedia.org/wiki/Square_degree

## av1an

### Start

```cmd
scoop install vapoursynth vmaf
py .../vsrepo.py update
py .../vsrepo.py install lsmas ffms2
scoop install nasm emscripten
emsdk install latest
emsdk activate latest
```

### Build aom

```cmd
git clone https://github.com/m-ab-s/aom
mkdir aom-windows-build
cmake -S ./aom -B ./aom-windows-build -G "Visual Studio 17 2022"
cd ./aom-windows-build
cmake --build .
```

## seamless

### CMD

```
ffmpeg -i video.mp4 frame%04d.png
seamless fast -e=png -o=mygif.gif .
```

## fonts

### Reference

1. https://vladar.bearblog.dev/creating-fonts-from-public-domain-reference/
2. https://github.com/LingDong-/tk-fangsong-font

## pdf

1. PDF浏览器[xpdfreader](https://www.xpdfreader.com/download.html)：用来检视.pdf。
2. Golang包[](https://github.com/FiloSottile/mkcert)：用来生成自信任的为PCCS（Public-Key Cryptography Standards）#12格式、扩展名为.p12或.pfx的证书文件证书。
3. Java应用[BatchPDFSign](https://github.com/jmarxuach/BatchPDFSign)：使用.pfx给.pdf写入自签名数字ID，进行数字签名（Digital Signature）。
4. Golang包[markpdf](https://github.com/ajaxray/markpdf)：给.pdf的每页添加水印。这里用来铺一层很浅的装饰底纹。

## mkdocs

### code-block

``` title="extra.css"
--8<-- "assets/css/extra.css"
```

- https://squidfunk.github.io/mkdocs-material/reference/code-blocks/#embedding-external-files
- https://github.com/squidfunk/mkdocs-material/issues/5526

### table

{{ read_csv("../data/currency-code.csv") }}

- https://squidfunk.github.io/mkdocs-material/reference/data-tables/#import-table-from-file
- https://squidfunk.github.io/mkdocs-material/reference/grids
- https://squidfunk.github.io/mkdocs-material/reference/lists/#using-definition-lists


## Yomichan

- https://github.com/MarvNC/yomichan-dictionaries/blob/master/how-to-make-yomichan-dictionaries.md
- https://gist.github.com/shoui520/25460fd2e9fb194d3e5152fa2ce42ca2#installing-dictionaries-and-basic-usage
- https://github.com/themoeway/yomichan-dict-css
- https://www.zdic.net/aboutus/copyright/
- https://github.com/lxs602/Chinese-Mandarin-Dictionaries
