# Installing Emacs 30.0.5 on Ubuntu 22.04 LTS

By building Emacs29 and greater with tree-sitter, you can use the package combobulate  to use the concrete syntax tree to perform vastly more efficient edits of code.
You are going to need this capability to fix all of that wrong code returned by ChatGPT.

Tree-sitter is an C-library that has to be installed in advance of compiling Emacs from source code.
Note as of early February 2023, emacs-plus@29 in home brew does not ship with support for tree-sitter nor does the latest version downloaded from Emacs on Mac OSX.

I was trying to install Emacs 29.0.5, but as of Monday February 6, 2023, the emacs29 branch of the git repo was loaded with Emacs 30.0.5. This error should be fixed soon, but user be aware.

I did this installation in a fresh virtual machine using VirtualBox VM.
I tried to install from the iso file on an external harddrive but I my account failed to be in the sudouser list.
To fix this problem, I installed again with the iso file on a USB flash drive.

I followed the instruction [here](https://www.techrepublic.com/article/how-to-enable-copy-and-paste-in-virtualbox/) to enable the copying and pasting of commands between the host and guest operating systems.

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
sudo apt install libc6-dev libjpeg62-dev libncurses5-dev libpng-dev xaw3dg-dev zlib1g-dev libice-dev libsm-dev libx11-dev libxext-dev   sudo apt install  libxi-dev libxmu-dev libxmuu-dev libxpm-dev libxrandr-dev libxt-dev libxtst-dev libxv-dev
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

Now enter "emacs30" to fire up the GUI and enter "C-h v"  and then enter the variable "system-configuration-features".
You should have the following returned. Note TREE_SITTER before X11:

```bash
"CAIRO DBUS FREETYPE GIF GLIB GMP GNUTLS GSETTINGS HARFBUZZ JPEG LIBSELINUX LIBXML2 MODULES NOTIFY INOTIFY PDUMPER PNG SECCOMP SOUND SQLITE3 THREADS TIFF TOOLKIT_SCROLL_BARS TREE_SITTER X11 XAW3D XDBE XIM XINPUT2 XPM LUCID ZLIB"
```

## Install texlive
I then installed texlive to support the use of latex and org-mode in Emacs.

```bash
sudo apt install texlive-latex-base texlive-latex-recommended texlive-latex-recommended-doc texlive-science texlive-science-doc 
sudo apt install texlive-fonts-recommended texlive-fonts-recommended-doc texlive-luatex texlive-xetex
```

## Notes

- I tried to compile with "--with-imagemagick" but the required libraries were not found.
