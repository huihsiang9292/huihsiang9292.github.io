---
title: Arch Linux
date: 2024-10-11
lastmod: 2024-10-11
categories:
  - Linux
  - Arch
---

# 系統

## Arch Linux

驗證啟動模式是否為 UEFI，若命令結果為 64、32，表示是以 64、32 位元的 UEFI 啟動

```bash
cat /sys/firmware/efi/fw_platform_size
```

確認是否有網路

```bash
ping google.com
```

輔助安裝腳本

```bash
# archinstall --config <url>
archinstall
```

## 軟體清單

- openssh
- tailscale
- greetd-tuigreet
- xorg-server
- xorg-xinit
- xorg-xrandr
- i3-wm
- rofi
- ranger
- polybar
- picom
- feh
- polkit-kde-agent
- wezterm
- neovim
- firefox
- timeshift
- ttf-cascadia-code
- adobe-source-han-serif-tw-fonts

## 軟體配置

### openssh

開機自啟、啟動 ssh server

```bash
sudo systemctl enable sshd
sudo systemctl start sshd
```

### tailscale

開機自啟 tailscale、連結到帳戶

```bash
sudo systemctl enable --now tailscaled
sudo tailscale up
tailscale ip -4
```

### greetd-tuigreet

開啟配置文件、自啟 i3-wm、邊框顏色為亮黃色、輸入顏色為亮紅色、重啟

```bash
sudo nvim /etc/greetd/config.toml

[default_session]
command = "tuigreet --cmd startx --remember --theme 'border=LightYellow;input=LightRed'"

sudo systemctl restart greetd
```

### xorg-xinit

複製預設配置文件並開啟

```bash
sudo cp /etc/X11/xinit/xinitrc ~/.xinitrc
sudo nvim ~/.xinitrc
```

刪除底部預設自啟項並新增 i3-wm

```bash
exec i3
```

啟動 X11、查找螢幕名稱

```bash
startx
xrandr | grep connected
```

### rofi

開啟 i3-wm 配置文件、修改`dmenu`為`rofi -show drun`

```bash
nvim ~/.config/i3/config

bindsym $mod+d exec --no-startup-id rofi -show drun
```

### ranger

複製預設配置文件

```bash
ranger --copy-config=all
```

### polybar

複製預設配置文件

```bash
mkdir ~/.config/polybar
sudo cp /etc/polybar/config.ini ~/.config/polybar/config.ini
```

修改`fallback-value`為查找到的螢幕名稱

```bash
sudo nvim ~/.config/polybar/config.ini

[bar/example]
monitor = ${env:MONITOR:fallback-value}
```

建立啟動文件並給予執行權限，修改`bar`為`[bar/example]`中的名稱

```bash
touch ~/.config/polybar/launch.sh
nvim ~/.config/polybar/launch.sh

#!/usr/bin/env bash
# Terminate already running bar instances
# If all your bars have ipc enabled, you can use
polybar-msg cmd quit
# Otherwise you can use the nuclear option:
# killall -q polybar
# Launch bar1 and bar2
echo "---" | tee -a /tmp/polybar.log
polybar bar 2>&1 | tee -a /tmp/polybar.log & disown
echo "Bars launched..."

chmod +x ~/.config/polybar/launch.sh
```

開啟 i3-wm 配置文件、刪除預設 bar 、新增自啟 polybar

```bash
nvim ~/.config/i3/config

bar {
    i3bar_command i3bar
}

exec_always --no-startup-id ~/.config/polybar/launch.sh &
```

### picom

複製預設配置文件

```bash
mkdir ~/.config/picom
cp /etc/xdg/picom.conf ~/.config/picom/picom.conf
```

開啟 i3-wm 配置文件、新增自啟 picom

```bash
nvim ~/.config/i3/config

exec_always --no-startup-id picom -b --config ~/.config/picom/picom.conf &
```

### feh

設定桌布

```bash
feh --bg-fill /path/to/image.file
```

設定隨機桌布，資料夾內只能有圖檔

```bash
feh --bg-fill --randomize /path/to/image/folder/*
```

開啟 i3-wm 配置文件、新增自啟 feh

```bash
nvim ~/.config/i3/config

exec_always ~/.fehbg &
```

## 美化

### i3-WM

配置文件

```bash

```
