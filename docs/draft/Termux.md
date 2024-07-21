↪ https://termux.dev/en

```
cp /storage/emulated/0/Download/font.ttf ~/.termux/font.ttf""termux-reload-settings
```

## neofetch

↪ https://github.com/dylanaraps/neofetch

```
pkg install neofetch
neofetch
```

## Ubuntu

↪ https://github.com/hctilg/root-termux

```
apt update && apt upgrade
apt install sudo
apt install curl wget git vim
```

## ohmyzsh

↪ https://ohmyz.sh/#install

```
apt install zsh
wget --no-check-certificate https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh
    bash install.sh
    exec zsh
```

↪ https://www.reddit.com/r/termux/comments/ndt0zo/how_to_switch_to_zsh_in_ubuntu_with_prootdistro/

↪ https://github.com/zplug/zplug

```
git clone https://github.com/zplug/zplug .zplug
source .zshrc
```

```
apt install fzf
```

```
apt install cargo
```

↪ https://github.com/atuinsh/atuin

Add `.cargo/bin/;` to PATH

↪ https://github.com/olets/zsh-abbr  
↪ https://github.com/olets/zsh-abbr/issues/88

```
apt install exa
```

↪ https://github.com/solidiquis/erdtree

```
cargo install erdtree
```

## SSH

↪  https://github.com/dmdhrumilmistry/Termux-SSH  
↪  [Termux 上运行SSH Server](https://zhuanlan.zhihu.com/p/226393968)

## Glyr

↪  https://github.com/sahib/glyr/wiki/Compiling

```
git clone https://github.com/sahib/glyr
cd glyr
pkg install cmake
cmake -DCMAKE_INSTALL_PREFIX=/usr .
make
make install
```

## cmus

```
:a storeage/Download
```


↪ https://github.com/newsboat/newsboat

```sh
echo URL >> ~/.newsboat/urls
gh-download newsboat/newsboat contrib/colorschemes
newsboat --config-file=contrib/colorschemes/plain
```

## asciinema

```
pkg install asciinema
```

## broot

↪ https://dystroy.org/broot/install/  

## xplr (Cache)

↪ https://github.com/newsboat/newsboat

## zellij

Download `zellij-aarch64-unknown-linux-musl.tar.gz` from release.

↪ https://github.com/zellij-org/zellij/blob/main/zellij-utils/assets/themes  
↪ https://zellij.dev/documentation/theme-gallery#tokyo-night-light  
↪ https://zellij.dev/documentation/layouts#layout-default-directory

```
default_shell "fish"
```

↪ https://github.com/zellij-org/zellij/blob/main/zellij-utils/assets/config/default.kdl

## dict

↪ [A command line dictionary.](https://nchrs.xyz/stardict.html)

在手机上，我之前使用[Aard 2](https://github.com/itkach/aard2-android)来查生词，词典用的是[ECDICT](https://github.com/skywind3000/ECDICT)的[ecdict-stardict-28.zip](https://github.com/skywind3000/ECDICT/releases/tag/1.0.28)。目前我在[Termux](https://termux.dev/en/)中使用多个工具来查询，[sdcv](https://github.com/Dushistov/sdcv)、

## pyenv

↪ [Installation](https://github.com/pyenv-win/pyenv-win/blob/master/docs/installation.md)  
↪ [Python 杂记之 解决Win 7中安装与配置pyenv-win时因secure channel错误所引起的问题](https://zicowarn.github.io/2021/06/19/0813-pyenv-install-error-secure-channel-issue/)  
↪ [在国内更便利的安装使用Homebrew、Pyenv以及pip](https://danielliou.wordpress.com/2023/03/18/homebrewpyenvpip/)  
↪ [pyenv를 termux에서 써보자 (2)](https://gwangyi.github.io/posts/pyenv-in-termux-2/)

## Ohter

↪ https://github.com/adi1090x/termux-style  
↪ https://github.com/termux/termux-styling#how-to-use  
↪ https://www.darkwiki.in/how-to-customize-termux-make-termux-terminal-look-awesome-ancii-color-font-style/  
↪ https://github.com/adi1090x/termux-desktop  
↪ https://github.com/Yisus7u7/termux-desktop-xfce  
↪ https://github.com/AdarshAddee/root  
↪ https://github.com/wasabeef/droid  
↪ https://github.com/Haruno19/starfetch#installation  
↪ https://github.com/solidiquis/erdtree  
↪ https://www.rust-lang.org/tools/install

## Troubleshoot

- [apt-get update fails to fetch files, “Temporary failure resolving …” error](https://askubuntu.com/questions/91543/apt-get-update-fails-to-fetch-files-temporary-failure-resolving-error)
- [cargo install: specify a /tmp substitute?](https://stackoverflow.com/questions/64572901/cargo-install-specify-a-tmp-substitute/64616981#64616981)
- [Can not install on android - target 'aarch64-linux-android' not found in channel.](https://github.com/rust-lang/rustup/issues/2872)
