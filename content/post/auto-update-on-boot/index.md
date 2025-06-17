---
title: "自動更新 Ubuntu 並使用 Discord 通知"
date: 2025-06-17
lastmod: 2025-06-17
categories:
  - Linux
  - Ubuntu Server
  - Discord
---

# 前言

在日常的 Ubuntu Server 維運中，保持套件更新是確保安全與穩定的基本工作。不過，頻繁手動登入更新麻煩又容易遺漏。

這篇教學將示範如何使用 Shell 腳本自動執行更新，並將執行結果透過 Discord Webhook 即時推播到你指定的頻道。

# Discord Webhook

1. 新增 Discord 伺服器
2. 進入伺服器設定，並在左側選單中點選 `整合`
3. 點選 `Webhook` 並新增
4. 命名為 `auto-update-bot`（自訂名稱）
5. 複製產生的 `Webhook URL`
6. 貼到腳本中的 `DISCORD_WEBHOOK_URL` 欄位

# Shell 腳本

## 概覽

| 功能               | 說明                                                   |
|--------------------|--------------------------------------------------------|
| 自動更新套件       | 執行 `apt update && upgrade` 並清除緩存                |
| 日誌記錄           | 輸出更新紀錄到 `/var/log/auto-update.log`             |
| Discord 通知       | 傳送通知訊息至 Discord 頻道                            |
| 避免重複執行       | 每次只執行一次更新，避免重複更新                        |
| 判斷是否需要重啟   | 根據 `/var/run/reboot-required` 自動通知                |

## 內容

路徑：`/usr/local/bin/auto-update.sh`  

```bash
#!/bin/bash

# === 設定 ===
LOG_FILE="/var/log/auto-update.log"      # 記錄更新過程的日誌檔案路徑
DISCORD_WEBHOOK_URL=""  # Discord Webhook URL，請替換成你自己的

# === Discord 通知函式 ===
# 這個函式會將訊息發送到指定的 Discord webhook
function notify_discord() {
    local message="$1"  # 取得要發送的訊息內容
    curl -s -H "Content-Type: application/json" \  # 設定 HTTP 標頭為 JSON
         -X POST \                                 # 使用 POST 方法
         -d "{\"content\": \"$message\"}" \        # 將訊息內容包裝成 JSON 格式
         "$DISCORD_WEBHOOK_URL" > /dev/null        # 發送到 Discord，並忽略輸出
}

# === 開始更新 ===
# 在日誌中記錄開始訊息，並通知 Discord
echo "[$(date)] 🚀 開始自動更新" | tee -a "$LOG_FILE"
notify_discord "🚀 [$(hostname)] 開始自動更新 Ubuntu 套件"

# === 套件更新流程 ===
# 執行 apt 指令進行系統套件更新，並將所有輸出寫入日誌檔案
{
    apt update             # 取得最新的套件清單
    apt upgrade -y         # 自動安裝所有可升級的套件
    apt autoremove -y      # 移除不再需要的套件
    apt clean              # 清除下載的套件快取
} >> "$LOG_FILE" 2>&1      # 將標準輸出與錯誤輸出都寫入日誌

# === 是否需要重啟 ===
# 檢查 /var/run/reboot-required 檔案是否存在，判斷系統是否需要重啟
if [ -f /var/run/reboot-required ]; then
    echo "[$(date)] ⚠️ 系統需要重新啟動" | tee -a "$LOG_FILE"
    notify_discord "⚠️ [$(hostname)] 更新完成，但系統需要重開機"
else
    echo "[$(date)] ✅ 不需重啟。" | tee -a "$LOG_FILE"
    notify_discord "✅ [$(hostname)] 更新完成，不需重啟"
fi

# === 完成通知 ===
echo "[$(date)] 🏁 更新完成" | tee -a "$LOG_FILE"
notify_discord "🏁 [$(hostname)] 自動更新流程完成"
```

## 給予執行權限

```bash
sudo chmod +x /usr/local/bin/auto-update.sh
```

## 開機自動執行

建立 service：

```bash
systemd Service：/etc/systemd/system/auto-update.service
```

內容如下:

```bash
[Unit]
Description=Ubuntu 開機自動更新               # 服務描述，顯示於 systemctl status
After=network-online.target                  # 等待網路連線完成後再啟動本服務
Wants=network-online.target                  # 本服務需要網路連線

[Service]
Type=oneshot                                # 只執行一次，執行完畢即結束
ExecStartPre=/bin/sleep 30                  # 在執行主程式前先暫停 30 秒，確保網路穩定
ExecStart=/usr/local/bin/auto-update.sh     # 執行自動更新腳本
StandardOutput=journal                      # 將標準輸出寫入 systemd journal
StandardError=journal                       # 將錯誤輸出寫入 systemd journal
RemainAfterExit=true                        # 執行完畢後仍保持為 active 狀態

[Install]
WantedBy=multi-user.target                  # 在多使用者模式下啟動（一般開機流程會進入此
```

啟用服務

```bash
# 重新載入 systemd 的服務設定檔，讓 systemd 讀取最新的 .service 檔案
sudo systemctl daemon-reload

# 啟用 auto-update.service，讓它在每次開機時自動執行
sudo systemctl enable auto-update.service
```

下次開機就會自動執行一次。你也可以手動觸發：

```bash
sudo systemctl start auto-update.service
```
