---
tags: [cache#]
---

## Install

```sh
scoop install vapoursynth vmaf
py .../vsrepo.py update
py .../vsrepo.py install lsmas ffms2
scoop install nasm emscripten
emsdk install latest
emsdk activate latest
```

## Build `aom`

```sh
git clone https://github.com/m-ab-s/aom
mkdir aom-windows-build
cmake -S ./aom -B ./aom-windows-build -G "Visual Studio 17 2022"
cd ./aom-windows-build
cmake --build .
```