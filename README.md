![Version](https://img.shields.io/static/v1?label=emacs30ubuntu22&message=0.2&color=brightcolor)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)


# Compiling Emacs 30.0.5 on Ubuntu 22.04 LTS with tree-sitter support

## Quick start with emacs 29.4 in two lines of commands

If you want treesitter installed in emacs29.4, I recommend the following procedure if you have snap enabled on Ubuntu.
You will obtain a binary of Emacs that has treesitter plus many other C programs included.

```bash
sudo snap install emacs --classic
# This installs emacs 29.4 from Alex Murray.
# ~/e29fewpackages is where I install my init.el file.
emacs --init-directory ~/e29fewpackages --debug-init
```
This worked in Ubuntu 24.04 on January 3rd, 2025.


## Original post
By building Emacs29 and greater with tree-sitter, you can use the package combobulate to use the concrete syntax tree to perform vastly more efficient edits of code.
You will need this capability to fix all of the wrong code returned by ChatGPT.

Tree-sitter is a C-library that must be installed before compiling Emacs from source code.
Please note that as of early February 2023, emacs-plus@29 in home brew does not ship with support for tree-sitter, nor does the latest version downloaded from Emacs on Mac OSX.

I was trying to install Emacs 29.0.5, but as of Monday, February 6, 2023, the emacs29 branch of the git repo was loaded with Emacs 30.0.5. This error should be fixed soon, but the user should be aware.

I did this installation in a fresh virtual machine using VirtualBox VM.
I tried to install from the iso file on an external harddrive, but my account failed to land in the sudouser list.
I was unable to use `sudo` to install software.
To fix this problem, I repeated the virtual machine creation with the iso file on a USB flash drive.

I followed the instructions [here](https://www.techrepublic.com/article/how-to-enable-copy-and-paste-in-virtualbox/) to enable the copying and pasting of commands between the host and guest operating systems.

```bash
sudo apt update
sudo apt upgrade
sudo apt-get install build-essential
sudo apt-get install libgtk-3-dev libtiff5-dev libgif-dev libjpeg-dev libpng-dev libxpm-dev libncurses-dev libtiff-dev
sudo apt-get install texinfo gnutls-dev imagemagick
sudo apt-get -y install libtree-sitter-dev
sudo apt install git
sudo apt install sqlite3
sudo apt install libgtk-3-dev libwebkit2gtk-4.0-dev
sudo apt install libc6-dev libjpeg62-dev libpng-dev xaw3dg-dev zlib1g-dev libice-dev libsm-dev libx11-dev libxext-dev
sudo apt install libxi-dev libxmu-dev libxmuu-dev libxrandr-dev libxt-dev libxtst-dev libxv-dev
sudo apt install libattr1-dev
sudo apt install autoconf
mkdir software
cd software
git clone git://git.sv.gnu.org/emacs emacs30
cd emacs30
git checkout -b emacs29
./autogen.sh
mkdir build
cd build
../configure --with-gnutls=ifavailable --with-cairo --with-tree-sitter --program-suffix=30
make -j 4
sudo make -j 4 install
```

## Check for support for tree-sitter

Now enter `emacs30` to fire up the GUI and enter `C-h v`, and then enter the variable `system-configuration-features.`
You should have the following returned. Note TREE_SITTER before X11:

```bash
"CAIRO DBUS FREETYPE GIF GLIB GMP GNUTLS GSETTINGS HARFBUZZ JPEG LIBSELINUX LIBXML2 MODULES NOTIFY INOTIFY PDUMPER PNG SECCOMP SOUND SQLITE3 THREADS TIFF TOOLKIT_SCROLL_BARS TREE_SITTER X11 XAW3D XDBE XIM XINPUT2 XPM LUCID ZLIB"
```

## Install texlive
I then installed `texlive` to support using latex and org-mode in Emacs.

```bash
sudo apt install texlive-latex-base texlive-latex-recommended texlive-latex-recommended-doc texlive-science texlive-science-doc 
sudo apt install texlive-fonts-recommended texlive-fonts-recommended-doc texlive-luatex texlive-xetex
```

## Make alias to emacs30

Now, make a profile for emacs30 by storing it in the directory "~/latex-emacs30".
I am taking advantage of the `--init-directory` flag available since version 29.

```bash
cd
mkdir latex-emacs30
touch .bashAliases
emacs30 .bashAliases # add the following line
alias e30ld='/usr/local/bin/emacs30 --init-directory ~/latex-emacs30 --debug-init' 
C-x C-s
C-x C-c
emacs30 .bashrc # add the following line
source ~/.bashAliases
C-x C-s
C-x C-c
source .bashrc # now enter the alias to fire Emacs
e30ld
```

## Notes

- I tried to compile with "--with-imagemagick" but the required libraries were not found.
