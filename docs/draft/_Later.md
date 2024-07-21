## Komga

Tools:

- Directory_Opus
- Xnconvert
- FastStone
- Advanced_Renamer
- ki-cli-hyphen
- Mkdirs
- To_CBZ
- Komga_Cover_Extractor

Selfhost:

```sh
for /d %%X in (*) do arenc.exe -e ".../v01.aren" -t folders -p . -m rename
for /d %%X in (*) do arenc.exe -e ".../001.aren" -t files -p %%X -m rename
for /d %%X in (*) do cp %%X/001.jpg %%X/cover.jpg
for /d %%X in (*) do py39 to-cbz/to_cbz.py %%X
py39 D:/binp/komga-cover-extractor/komga_cover_extractor.py -c "True" -cq "70" -p .
```

```sh
cook c 1001-1188 | sd c1 "\55 c" > _.md && mkdirs _.md && trash _.md
```

## LaTeX

Reference:

- [LaTeX tables: Basics](https://vladar.bearblog.dev/latex-tables-basics/)
- [LaTeX page geometry and layout](https://vladar.bearblog.dev/latex-page-geometry-and-layout/)
- [LaTeX: text spacing and decoration](https://vladar.bearblog.dev/latex-text-spacing-and-decoration/)
- [LaTeX and its fancy fonts](https://vladar.bearblog.dev/latex-and-its-fancy-fonts/)
- [LaTeX document structure](https://vladar.bearblog.dev/latex-document-structure/)
- [LaTeX for tabletop](https://vladar.bearblog.dev/latex-for-tabletop/)
- [LaTeX tables: Advanced features](https://vladar.bearblog.dev/latex-tables-advanced-features/)
- [LaTeX images](https://vladar.bearblog.dev/latex-images/)

Tools:

- [TeXtidote](https://github.com/sylvainhalle/textidote)

## Weblate

```sh
sudo apt install -y \
   libxml2-dev libxslt-dev libfreetype6-dev libjpeg-dev libz-dev libyaml-dev \
   libffi-dev libcairo-dev gir1.2-pango-1.0 libgirepository1.0-dev \
   libacl1-dev libssl-dev libpq-dev libjpeg-dev build-essential \
   python3-gdbm python3-dev python3-pip python3-virtualenv virtualenv git
```

```sh
sudo apt install -y \
   libldap2-dev libldap-common libsasl2-dev \
   libxmlsec1-dev
```

```sh
sudo apt install -y nginx uwsgi uwsgi-plugin-python3 redis-server postgresql postgresql-contrib exim4 gettext
```

```sh
sudo apt-get install git-svn
```

## ReviOS

↪ https://learn.microsoft.com/zh-cn/windows/release-health/release-information
↪ https://www.revi.cc/docs/faq/before/features/
↪ https://www.revi.cc/docs/playbook/installwindows/
↪ https://www.revi.cc/docs/playbook/install/
↪ https://www.revi.cc/docs/post-install/

## maker

↪ https://github.com/tesseract-ocr/tessdata_best
↪ [Enable your device for development](https://learn.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development)
↪ [x] https://github.com/lakshay1296/ocrmyPDF_Windows
↪ [x] https://github.com/lakshay1296/OCR_Conversion_JPEG2PDF
↪ [x] https://github.com/pidydx/libmagicwin64
↪ [x] https://github.com/nscaife/file-windows

## starfetch

```sh
git clone https://github.com/Haruno19/starfetch
cd starfetch/res/constellations
```

```sh
git clone https://github.com/K1ngst0m/starfetch
cd starfetch
make
./starfetch.exe -r
```

Or:

```sh
scoop install msys2
```

```sh
git clone https://github.com/CoderCharmander/starfetch
cargo build
cd target/debug
starfetch.exe -d
starfetch.exe -l
```

## syncabook

Full → Language(Chinese) → Upload Files → VAD(silero-vad-skip-gaps) → Initial Prompt(对于普通话句子，以中文简体输出) → Diarization - Speakers(), Min Speakers(1), Max Speakers() → Submit

```sh
git clone https://github.com/r4victor/syncabook
cd syncabook
pvthon37 -m venv venv
venv\Scripts\activate.bat
git clone afaligner https://github.com/r4victor/afaligner _afaligner
cd _afaligner
pip install -e .
edit setup.py
cd ..
```

See [Check out this ShareGPT conversation](https://sharegpt.com/c/97thh2m)

```sh
pip install pytest epubcheck
python -m pytest -s tests/
```

```sh
syncabook download_files theurl thebook
```

```sh
syncabook split_text --mode opening --p theindex thebook\text.txt thebook\plaintext
syncabook split_text --mode delimeter --p theindex thebook\text.txt thebook\plaintext
syncabook split_text --mode equal --n 2 thebook\text.txt thebook\plaintext
```

```sh
syncabook to_xhtml thebook/plaintext/ thebook/sync_text/
syncabook sync thebook/
```

```sh
syncabook create thebook
```

```sh
cd _synclibrivox\books\_little_prince\ // Cache
syncabook.exe split_text --m opening --p 第一章 text.txt plaintext
iconv -f gbk -t utf-8 plaintext\1.txt > plaintext\_1.txt && trash plaintext\1.txt && move plaintext\_1.txt plaintext\1.txt
syncabook.exe to_xhtml plaintext sync_text\
iconv -f gbk -t utf-8 sync_text\1.xhtml > sync_text\_1.xhtml && trash sync_text\1.xhtml && move sync_text\_1.xhtml  sync_text\1.xhtml
syncabook sync _synclibrivox\books\_little_prince
syncabook create thebook
```

... calibre-web → Apphabetical Books → ALL → `The Black Cat` → Import (EPUB)
My Books → `The Black Cat` → ButtonOfPlayAudio

## cumaean

- CUETools
- XMedia_Recode
- MP3TAG
- renamer
- Advanced_Renamer
- AudioShell
- TagScanner
- Lidarr

Note:

```sh
pdf-toc -t %2 -d _toc_%1 %1
```

## Pi OS

### SSH

On uConsole: Preferences → Raspberry Pi Configuration → Interfaces → SSH (On)r

```sh
hostname -I
```

On PC:

```sh
scoop install winsshterm
scoop install putty wimscp vcxsrv
```

Reference:

- [How to SSH into Raspberry Pi](https://www.onlogic.com/company/io-hub/how-to-ssh-into-raspberry-pi/)

### Restart Wifi

```sh
sudo raspi-config
```

System Options → Wireless LAN → Enter SSID → Enter Password ... Finish

```sh
sudo reboot
```

## Arch Linux ARM

### fdisk

↪ https://github.com/PotatoMania/uconsole-cm3  
↪ https://github.com/PotatoMania/uconsole-cm3/blob/dev/doc/how-to-install-archlinux-from-scratch.md

```
sudo wipefs --all /dev/sdc
sudo fdisk --list
```

```
sudo fdisk /dev/sdc
```

```
sudo mkfs.vfat /dev/sdc1
sudo mkfs.ext4 /dev/sdc2
```
### mount

```
mount
sudo umount /mnt/boot
sudo umount /mnt
mount
```

```
sudo fdisk --list
sudo mount /dev/sdc2 /mnt
sudo mkdir /mnt/boot
sudo mount /dev/sdc1 /mnt/boot
```

```
sudo pacman -Sy
sudo pacman -S qemu-user-static qemu-user-static-binfmt arch-install-scripts
```

```
sudo bsdtar -xpf ArchLinuxARM-aarch64-latest.tar.gz -C /mnt
ls -l /mnt
sudo genfstab -U /mnt | sudo tee -a /mnt/etc/fstab
```
### linux-uconsole-cm3-rpi64*.pkg.zst

```
sudo arch-chroot /mnt
pacman-key --init
pacman-key --populate archlinuxarm
pacman -Sy raspberrypi-bootloader firmware-raspberrypi
pacman -R linux-aarch64
pacman -U --noconfirm linux-uconsole-cm3-rpi64*.pkg.zst
pacman -U --noconfirm ap6256-firmware*.pkg.tar
pacman -S iwe
```
### makepkg

```
git clone https://github.com/PotatoMania/uconsole-cm3
git clone https://github.com/systematiccaos/uconsole-cm3-cm4
cd uconsole-cm3/PKGBUILDs/linux-uconsole-cm3-rpi64
```

```
git clone --depth=1 -b rpi-6.1.y https://github.com/raspberrypi/linux.git
tar -czvf linux.tar.gz linux
```

```
tar -xzvf linux.tar.gz linux
cd linux
git status
git restore --source=HEAD :/
```

```
sudo pacman -S cpio pahole aarch64-linux-gnu-gcc make flex bison patch
makepkg
pacman -Syu
```

### ap6256-firmware*.pkg.zst

```
useradd -m auruser
passwd auruser
echo "auruser ALL=(ALL) ALL" > /etc/sudoers.d/auruser
chmod 440 /etc/sudoers.d/auruser
chmod u+w /home/auruser/ap6256-firmware
pacman -S fakeroot sudo
su - auruser
ls -l /home/auruser/ap6256-firmware
export PKGDEST=/tmp/my_package_destination
export SRCDEST=/tmp/my_source_directory
export BUILDDIR=/tmp/my_build_directory
makepkg
pacman -U /tmp/my_package_destination/ap6256-firmware-0.1.20231120-1-any.pkg.tar.xz

```
### config.txt

```
sudo vim /mnt/boot/config.txt
```

```
ignore_lcd=1
disable_fw_kms_setup=1
max_framebuffers=2
arm_boost=1

# setup headphone detect pin
gpio=10=ip,np

# boot kernel directly
kernel=Image.gz
arm_64bit=1
initramfs initramfs-linux.img followkernel

# overlays
dtoverlay=dwc2,dr_mode=host
dtoverlay=vc4-kms-v3d
dtoverlay=audremap,pins_12_13
dtparam=audio=on
dtoverlay=uconsole
```

↪ https://github.com/PotatoMania/uconsole-cm3-arch-image-builder/issues/1

```
[all]
ignore_lcd=1
disable_fw_kms_setup=1
disable_audio_dither
pwm_sample_bits=20

# setup headphone detect pin
gpio=10=ip,np

# boot custom kernel
kernel=Image.gz
arm_64bit=1
initramfs initramfs-linux.img followkernel

dtoverlay=dwc2,dr_mode=host
dtoverlay=audremap,pins_12_13
dtparam=audio=on

[pi3]
dtoverlay=vc4-kms-v3d
dtoverlay=uconsole

[cm4]
arm_boost=1
max_framebuffers=2
dtoverlay=vc4-kms-v3d-pi4
dtoverlay=uconsole,cm4

[all]
# whatever you need
```

```sh
sudo umount /mnt/boot /mnt
```

## nvim

### on Windows

Install neovim with [Bob](https://github.com/MordechaiHadad/bob), get `bob.exe` from [releases](https://github.com/MordechaiHadad/bob/releases)

```sh
bob install 0.9.0
```

Install lunarVim in PowerShell:

```sh
$env:Path += ";C:\Users\YourName\AppData\Local\bob\v0.9.0\nvim-win64\bin"
```

```sh
pwsh -c "`$LV_BRANCH='release-1.3/neovim-0.9'; iwr https://raw.githubusercontent.com/LunarVim/LunarVim/release-1.3/neovim-0.9/utils/installer/install.ps1 -UseBasicParsing | iex"
```

Edit `.../Cmder/config/user_profile.cmd`:

```
set "XDG_CACHE_HOME=C:\Users\YourName\AppData\Local\Temp%XDG_CACHE_HOME%"
set "XDG_RUNTIME_DIR=C:\Users\YourName\AppData\Local\Temp%XDG_RUNTIME_DIR%"
set "LUNARVIM_BASE_DIR=C:\Users\YourName\AppData\Roaming\lunarvim\lvim%LUNARVIM_BASE_DIR%"
set "LUNARVIM_CACHE_DIR=C:\Users\YourName\AppData\Local\Temp\lvim%LUNARVIM_CACHE_DIR%"
set "LUNARVIM_CONFIG_DIR=C:\Users\YourName\AppData\Local\lvim%LUNARVIM_CONFIG_DIR%"
set "LUNARVIM_RUNTIME_DIR=C:\Users\YourName\AppData\Roaming\lunarvim%LUNARVIM_RUNTIME_DIR%"
```

```sh
lvim
```

```sh
scoop install zig
scoop install gcc
scoop install llvm
```

Edit `init.lua`:

```lua
require "nvim-treesitter.install".compilers = { "zig" }
```

```sh
checkhealth
```

```sh
pip install neovim
pnpm add -g neovim
scoop install ripgrep perl
```

```sh
pip install git+https://github.com/luarocks/hererocks
cd D:/lib
hererocks lua53 -l5.3 -rlatest
source lua53/bin/activate
lua -v
luarocks install luacheck
luacheck --version
deactivate-lua
lua53/bin/lua -v
```

```sh
scoop install neovide
```

Create `lv.cmd`:

```sh
neovide --neovim-bin C:\Users\YourName\AppData\Local\bob\v0.9.0\nvim-win64\bin --geometry 100x30 --notabs --frame none -- -u "%LUNARVIM_BASE_DIR%\init.lua" %*
```

Edit `init.lua`:

```lua
local alpha = function()
  return string.format("%x", math.floor(255 * vim.g.transparency or 0.75))
end

if vim.g.neovide then
    vim.api.nvim_command("language en_US.UTF-8")
    vim.o.guifont = "MonaspiceAr NFP + Sarasa Gothic SC + WFM Sans SC:h9"
    vim.g.neovide_padding_top = 0
    vim.g.neovide_padding_bottom = 0
    vim.g.neovide_padding_right = 0
    vim.g.neovide_padding_left = 0
    vim.g.neovide_transparency = 0.75
    vim.g.transparency = 0.75
    vim.g.neovide_background_color = "#0f1117" .. alpha()
    vim.g.neovide_floating_blur_amount_x = 1.0
    vim.g.neovide_floating_blur_amount_y = 1.0
    vim.g.neovide_cursor_animation_length = 0
    vim.g.neovide_cursor_trail_size = 0
    vim.g.neovide_remember_window_size = true
    vim.g.neovide_hide_mouse_when_typing = true
end
```

↪ https://vi.stackexchange.com/questions/36426/how-do-i-change-my-language-in-my-init-lua-neovim  
↪ https://github.com/nvim-treesitter/nvim-treesitter/issues/2135  
↪ https://github.com/LunarVim/Neovim-from-scratch/issues/274#issuecomment-1364584526

### lvim

↪ https://www.reddit.com/r/neovim/comments/166fpfb/ppa_not_working_with_lazynvim/
↪ https://github.com/neovim/neovim/wiki/Building-Neovim#build-prerequisites

```sh
make distclean
make deps
```

↪ https://github.com/neovim/neovim/wiki/Building-Neovim#building.org/zh-Hans/docs/installation

```sh
pip install trash-cli
```

↪ https://www.lunarvim.org/docs/configuration/plugins/user-plugins

`lvim` → `c` → `4d` → ... `:wq` → `lvim` →  `/peek.nvim` → `I`

`:` `Tab` `peek`

```sh
curl -fsSL https://deno.land/x/install/install.sh | sh
```

### wim

```sh
vim .vimrc
```

```
" SirVer/ultisnips
```

↪ https://stackoverflow.com/questions/4618151/how-to-reference-source-a-custom-vimrc-file