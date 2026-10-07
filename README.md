# DrvGo

DrvGo 是自動安裝驅動程式 (Driver) 與應用程式 (App) 的工具：選擇放安裝檔的資料夾，DrvGo 會找出每個項目的最新版本，並依序自動安裝。

## 下載

到右側的 **[Releases](../../releases/latest)** 下載最新的 `DrvGo.exe`，不需要安裝，直接執行即可。

- 啟動時會跳出 **UAC (使用者帳戶控制)**，請按「是」，DrvGo 需要系統管理員權限才能安裝驅動程式。
- DrvGo 同時只能開一個，已經在執行時再開會提示「DrvGo is already running」。

## 主畫面

| 位置 | 操作 |
| --- | --- |
| **Select Directory** | 選擇放安裝檔的資料夾，DrvGo 會自動掃描並列出可安裝的項目 (下次開啟會記住這個資料夾) |
| **Import Config / Export Config** | 把清單匯出成 Excel (`.xlsx`) 或 CSV，或從檔案匯入清單 |
| **Revoke Trust** | 移除 DrvGo 先前為了安裝驅動程式而信任的發行者憑證 |
| 右上角 **Language** | 切換 English / 繁體中文 |
| 右上角 ☀ / 🌙 | 切換亮色 / 深色模式 (預設跟隨 Windows 設定) |
| 右上角 ⚙ | 開啟 **Settings** (見下方) |

### 清單

每一列是一個要安裝的項目，欄位依序為 **Path / Folder**、**Revision** (版本)、**Detected Type** (EXE / ZIP)、**Target File**、**Result** (結果)。

- 同一個項目有多個版本時，只列出最新的版本。
- **橘色列、標示 ★ newest created**：同一個位置有多個不同平台 (SWB) 的套件，DrvGo 只保留建立時間最新的一個，請確認是不是你要安裝的平台。
- **Result** 顏色：綠色 = 成功、紅色 = 失敗、黃色 = 已中斷、藍色 = 執行中。

## 安裝

| 按鈕 | 說明 |
| --- | --- |
| **Start Installation Queue** | 安裝清單中所有項目 (先前已安裝成功的項目會自動略過) |
| **Install Selected** | 只安裝選取的項目 (可按住 Ctrl / Shift 複選，也可在清單上按右鍵) |
| **Retry Failed** | 只重新安裝失敗的項目 |
| **Stop** | 中斷安裝 (目前項目會被終止，其餘項目不再執行) |

- 一次只安裝一個項目，進度條顯示整體進度。
- 同編號的驅動程式與應用程式 (例如 `06_..._Drv` 與 `06_..._App`)，會先安裝 Drv；Drv 沒有安裝成功時，對應的 App 不會安裝。
- 安裝過程中 DrvGo 會阻擋系統重新開機，請勿手動重開機。
- 開始安裝前，會先套用 **Settings → Environment** 中開啟的環境設定。

### 安裝完成

全部項目跑完後會跳出結果。若有項目需要重新開機才能完成：

- **Restart Now**：立即重新開機 (約 5 秒後)。按鈕為**綠色**表示全部安裝成功；**紅色**表示有項目失敗或被中斷，建議先看執行日誌再決定。
- **Later**：稍後再自行重新開機。

## 執行日誌與安裝紀錄

- **Execution Logs**：顯示每一步的執行過程、回傳碼與錯誤訊息。**Clear Logs** 只清除畫面上的內容。
- 每個項目的結果都會即時保存，即使中途當機或重新開機，下次開啟 DrvGo 仍會顯示上次的結果；中途被打斷的項目會顯示 **Interrupted last time**，可用 **Retry Failed** 重新安裝。
- **Clear Records**：清除所有項目的安裝紀錄，之後 **Start Installation Queue** 會重新安裝全部項目。

## Settings (⚙)

### Environment (環境設定)

每個項目都有 ON / OFF 開關 (預設全部 ON)。ON 的項目會在每次安裝前自動套用，**會永久改變系統設定**：

| 項目 | 說明 |
| --- | --- |
| Disable UAC | 關閉使用者帳戶控制 (重新開機後生效) |
| Disable 'Automatically restart' on system failure | 系統當機時不自動重新開機 |
| Enable Testsigning | 允許安裝測試簽章的驅動程式 (重新開機後生效；開啟安全開機 Secure Boot 時無法設定) |
| All power plans: turn off the display / put the computer to sleep = Never | 所有電源計畫都不關閉螢幕、不進入睡眠 |
| Enable Hibernate | 啟用休眠，並在電源選單、「關機或登出」中顯示「休眠」 |

- **Apply Now**：不必等到安裝，依目前的開關立即套用，完成後顯示成功 / 失敗的項目數，以及需要重新開機才會生效的項目。
- **OK** 儲存開關設定；**Cancel** 放棄變更。

### Open Log Folder

用檔案總管開啟 DrvGo 的記錄資料夾 (`%LOCALAPPDATA%\DrvGo\logs`)，路徑也顯示在按鈕下方：

| 檔案 | 內容 |
| --- | --- |
| `execution_YYYYMMDD_HHMMSS.log` | 每次安裝的完整執行記錄 |
| `error.log` | 所有失敗的項目 |
| `Appx_Installer.log` | UWP App 的安裝記錄 |

### About

顯示目前的版本。按 **Check for Updates** 會檢查這裡是否有新版；有新版時按「是」會直接下載新的 `DrvGo.exe`，下載完成後關閉 DrvGo，改用新的 exe 即可。

## 查詢版本

在命令列執行以下指令，會跳出視窗顯示版本號 (不需要系統管理員權限)：

```
DrvGo.exe -v
```

版本號格式為發佈日期加上字母，例如 `20261007.A`。
