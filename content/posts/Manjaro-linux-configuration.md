---
author: Aucker AN
title: Manjaro Linux Configuration
date: 2020-04-19T20:15:41+08:00
# draft: true
# author: "Aucker AN"
---

## Mirrors
After the installation of Manjaro, configure the mirror
* `sudo pacman-mirrors -g` rank the mirrors
* `sudo pacman-mirrors -c China -m rank` change the Chinese mirror
* add `archlinuxcn` blocks in `/etc/pacman.conf` file.

```shell
[archlinuxcn]
SigLevel = Optional TrustedOnly
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```
* the optional installation is `archlinuxcn-keyring` so that import GPG key, in case of the verification failure:
    `sudo pacman -S archlinuxcn-keyring`
* sync and update the system  
    ```shell
    sudo pacman -S manjaro-keyring
    sudo pacman -Syyu
    ```
## Input methods
fcitx(Free Chinese Input Toy for X)
```
sudo pacman -S fcitx-googlepinyin
sudo pacman -S fcitx-im
sudo pacman -S fcitx-configtool # GUI config tool
sudo pacman -S fcitx-skin-material
```
solve the input method switch problem: add the file `~/.xprofile`:
```
export GTK_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
```
then reboot.

## pacman 
```
sudo pacman -S [name]  # install
sudo pacman -R [name]  # delete the single software, butthe relationship remains
sudo pacman -Rns [name]  # delte the software & dependencies
sudo pacman -Ss [name]  # find a software
sudo pacman -Sc  # clear & download new data
sudo pacman -Qs  # search the exist packages
```

## ZSH
sudo pacman -S zsh
(sudo) chsh -s /bin/zsh
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
> now manjaro KDE has the pre-install zsh environment, hooray!!!
