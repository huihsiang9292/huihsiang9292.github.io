---
title: "Timeshift 教學"
date: 2025-06-17
lastmod: 2025-06-17
categories:
  - Linux
---

本教學以 CLI 操作為主，適用於無 GUI 的 Server 環境。適合想定期快照整個系統、快速還原崩潰環境的你。

# 為什麼選擇 Timeshift？

1. 快速備份整個作業系統（通常是 / 根目錄）
2. 自動排程快照，定期保留歷史版本
3. 發生錯誤時快速還原，不需重裝系統

# 安裝

```bash
sudo apt install timeshift -y
```

# 模式

使用 `df -T /` 可查看你的根分割區是否為 btrfs，若不是，請使用 rsync 模式。

| 模式      | 適用                 | 說明                           |
| ------- | ------------------ | ---------------------------- |
| rsync |  大多數人             | 利用檔案層級備份，可用在任何檔案系統           |
| btrfs |  有使用 Btrfs 分割區的人 | 使用 Btrfs 原生 snapshot，更快更節省空間 |

# 備份命令

## 手動快照

```bash
sudo timeshift --create --comments "第一次備份"
```

* --create：建立快照
* --comments：備註資訊
* --tags D：標記為每日（Daily）快照

## 快照列表

```bash
sudo timeshift --list
```

## 還原快照

```bash
# 選擇列表
sudo timeshift --restore
# 直接指定
sudo timeshift --restore --snapshot '快照名稱'
```

## 清理舊快照

```bash
# 單一快照
sudo timeshift --delete --snapshot '快照名稱'
# 所有標記為每日的快照
sudo timeshift --delete-all --tags D
```

## 標記

| 類型（Tag） | 參數代碼       | 建議用途            | 說明             |
| ------- | ---------- | --------------- | -------------- |
| Boot    | --tags B | 每次開機自動備份（不建議過多） | 系統每開一次機會觸發一次備份 |
| Hourly  | --tags H | 每小時備份一次         | 適合變動頻繁的開發機     |
| Daily   | --tags D | 每日備份            | 常見的排程頻率        |
| Weekly  | --tags W | 每週備份            | 比較節省空間，適合穩定環境  |
| Monthly | --tags M | 每月備份            | 保留長期歷史         |
| Manual  | --tags O | 手動建立            | 自行用指令建立的快照     |

# 設定檔

Timeshift 的主要設定檔位於 `/etc/timeshift/timeshift.json`，儲存了所有與快照類型、排程、保留數量、使用者界面偏好等相關設定。

## 參數說明

| 參數                     | 類型 / 範例           | 說明                              |
| ---------------------- | ----------------- | ------------------------------- |
| backup_device_uuid   | string            | 儲存快照的磁碟分割區 UUID                 |
| snapshot_device_uuid | string            | 與 backup_device_uuid 相同，大多可忽略 |
| snapper_type         | "RSYNC" 或 "BTRFS" | 使用的快照技術                         |
| do_first_run         | true / false      | 是否為首次啟動時使用的引導設定                 |
| schedule_backup      | true / false      | 啟用排程快照（必須為 true）                |
| schedule_hourly      | true / false      | 啟用每小時快照排程                       |
| schedule_daily       | true / false      | 啟用每日快照排程                        |
| schedule_weekly      | true / false      | 啟用每週快照排程                        |
| schedule_monthly     | true / false      | 啟用每月快照排程                        |
| schedule_boot        | true / false      | 啟用每次開機後備份                       |
| count_hourly         | 整數                | 每小時快照保留數量                       |
| count_daily          | 整數                | 每日快照保留數量                        |
| count_weekly         | 整數                | 每週快照保留數量                        |
| count_monthly        | 整數                | 每月快照保留數量                        |
| count_boot           | 整數                | 開機後快照保留數量                       |
| exclude              | 陣列                | 要排除的目錄（不會備份）                    |
| exclude-apps         | true / false      | 是否排除已安裝的應用程式列表（如 apt 或 flatpak） |

## 注意事項

修改後請確保語法正確（可用 jq 檢查）：

```bash
jq . /etc/timeshift.json
```

無須手動啟用排程器（如 cron），Timeshift 本身會透過 systemd 排程執行且修改此設定不會立即生效於已建立的快照，僅影響未來行為。
