---
title: Arch Linux
date: 2024-10-11
categories:
  - Arch
  - i3-WM
---

# 系統

## Arch Linux

驗證啟動模式是否為 UEFI，若命令結果為 64、32，表示是以各自位元的 UEFI 啟動

```bash
cat /sys/firmware/efi/fw_platform_size
```

確認是否有網路

```bash
ping google.com
```

輔助安裝腳本

```bash
archinstall
```

## 軟體清單

- openssh
- greetd-tuigreet
- xorg-server
- xorg-xinit
- xorg-xrandr
- i3-wm
- polybar
- feh
- wezterm
- ttf-nerd-fonts-symbol-mono
- neovim

## 軟體配置

### openssh

啟動 ssh server

```bash
sudo systemctl start sshd
```

### greetd-tuigreet

開啟配置文件

```bash
sudo nvim /etc/greetd/config.toml
```

配置 tuigreet、自啟 i3-wm、邊框顏色為亮黃色、輸入顏色為亮紅色

```bash
[default_session]
command = "tuigreet --cmd startx --remember --theme 'border=LightYellow;input=LightRed'"
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

### polybar

複製預設配置文件

```bash
mkdir ~/.config/polybar
sudo cp /etc/polybar/config.ini ~/.config/polybar/config.ini
```

找出自己的螢幕名稱並修改`fallback-value`

```bash
xrandr | grep "connected"
nvim ~/.config/polybar/config.ini

[bar/example]
monitor = ${env:MONITOR:fallback-value}
```

開啟 i3-wm 配置文件、刪除預設 bar 、新增自啟 polybar

```bash
nvim ~/.config/i3/config

bar {
    i3bar_command i3bar
}

exec_always --no-startup-id ~/.config/polybar/launch.sh
```

建立啟動文件並給予執行權限，修改 `bar` 為 `[bar/example]` 中的名稱

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
polybar bar 2>&1 | tee -a /tmp/polybar1.log & disown
echo "Bars launched..."

chmod +x ~/.config/polybar/launch.sh
```
