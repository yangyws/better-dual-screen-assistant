<p align="center">
  <img src="docs/assets/showcase.png" alt="Better Dual Screen Assistant 展示" />
</p>

# Better Dual Screen Assistant

**Better Dual Screen Assistant**（`com.stormpanda.betterdualscreenassistant`）是一款可由使用者自行安裝的無縫替代與功能強化套件，專為 **AYN Thor**（`kalama` / Snapdragon 8 Gen 2，Android 13）與 **Retroid Pocket Duo**（`sun` / `CQ7790S`，Android 16）雙螢幕掌機打造，用來升級並取代系統內建的 `DualScreenAssistant`（`com.odin.dualscreen.assistant` / `com.retroidpocket.dualscreen.assistant`）。

本專案與各裝置的原廠助手維持 1:1 的視覺與功能一致性，並在此基礎上全面擴充：支援在所有主題間自由切換 **AYN Smooth** 與 **Retroid Pixel** 介面風格、手把操作優先的**上螢幕隨行設定應用程式（Companion Settings App）**、**24 個可自訂與重新排序的快速設定磚**、**硬體加速雙螢幕與遊戲內錄音錄影**、**精準逐任務遊戲 FPS 計數器**、**螢幕任務互換**、**類比搖桿 LED 亮度控制**、**Megingiard 主題色盤與即時同步**、**Android 13–16 Monet 系統主題注入**、**App 內更新檢查與 Obtainium 整合**，以及具備 Direct Boot 支援、持續性服務監控與解除安裝自動復原機制的**亞毫秒級 `UID 0` Binder 輔助背景常駐程式（Daemon）**。

<p>
  <a href="https://apps.obtainium.imranr.dev/redirect?r=obtainium://add/https://github.com/stormpanda/better-dual-screen-assistant">
    <img src="https://raw.githubusercontent.com/ImranR98/Obtainium/main/assets/graphics/badge_obtainium.png" alt="透過 Obtainium 取得" height="54" />
  </a>
</p>

---

## 支援裝置

| 裝置 | SoC 與韌體 | 上螢幕 | 下螢幕 | 取代的原廠套件 | 預設介面風格 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **[AYN Thor](docs/devices/AYN_THOR.md)** | Snapdragon 8 Gen 2 (`kalama`)，Android 13 (`SDK 33`) | `displayId = 0`（`1920 × 1080`，`16:9` 120 Hz AMOLED） | `displayId = 4`（`1240 × 1080`，120 Hz AMOLED，OLED 圖層熄滅） | `com.odin.dualscreen.assistant`（實體 **AYN** 鍵） | **AYN Smooth** |
| **[Retroid Pocket Duo](docs/devices/RETROID_POCKET_DUO.md)** | Moorechip `CQ7790S` (`sun` / Adreno 722)，Android 16 (`SDK 36`) | `displayId = 0`（`1920 × 1080`，`16:9` 120 Hz） | `displayId = 2`（`1280 × 960`，`4:3` 120 Hz，`bl_power` 背光熄滅） | `com.retroidpocket.dualscreen.assistant`（實體 **Dual Screen** 鍵） | **Retroid Pixel** |

- **跨裝置介面風格**：**AYN Smooth** 與 **Retroid Pixel**（包含 50+ 個自訂像素風向量圖示、倒角像素幾何造型、內建像素字型，以及選用的**提升 Retroid 可讀性**強化選項）皆可在**兩款**掌機的所有主題中自由使用。
- **自動硬體適配**：`DeviceProfile` 會自動適配顯示器 ID、SurfaceFlinger 實體連接埠、核心層 `panel{0,1}-backlight/bl_power` 與 OLED 熄滅機制、`IOverviewProxy` / `ILauncherProxy` 下螢幕最近使用的應用程式、原廠按鍵映射器 IPC（`com.odin.mapping` / `com.rp.mapping`）、控制器樣式標籤（*Odin* 與 *Retro*），以及特定裝置專屬功能（例如 AYN Thor 的類比搖桿 RGB *LED 燈效*與 *Home 鍵鎖定*）。

---

## 文件索引

完整的功能與技術文件請參閱 [`docs/`](docs/)：

| 文件 | 涵蓋範圍 |
| :--- | :--- |
| **[安全性與隱私權概覽](docs/SECURITY.md)** | 執行摘要、與原廠 `DualScreenAssistant` 的安全性與隱私權逐項比較、`PServerBinder` 與 `PrivilegedHelperMain` 說明、權限明細，以及解除安裝自動復原機制保證。 |
| **[功能規格書](docs/FUNCTIONAL_SPECIFICATION.md)** | 完整使用者指南，涵蓋實體助手按鍵狀態機、下螢幕懸浮覆蓋分頁（**模式**與**設定**）、全部 **24 個快速設定磚**、彈出對話框、即時硬體遙測數據，以及上螢幕**隨行設定應用程式**（`MainActivity`）。 |
| **[技術架構](docs/TECHNICAL_ARCHITECTURE.md)** | 深入解析三層式特權執行模型（`PrivilegedBridge` -> `PrivilegedHelperMain` `UID 0` 常駐程式 -> `PServerBinder`）、暫時性原廠 `DualScreenAssistant` 抑制機制、持續性服務監控與解除安裝復原看門狗，以及跨 AYN Thor（`displayId = 4`）與 Retroid Pocket Duo（`displayId = 2`）的多螢幕視窗不變量。 |
| **[系統整合與硬體參考](docs/SYSTEM_INTEGRATION_REFERENCE.md)** | 所有 `Settings.System`/`Global`/`Secure` 鍵值、`SharedPreferences`（`better_dsa_prefs`）、系統屬性（`sys.temp.disabled.touch`、`sys.light.curr.is.show`）、原廠 Binder 服務（`IFanControlProvider`、`IOverviewProxy`、`ILauncherProxy`、`IMappingService`）、sysfs 遙測路徑，以及 `FabricatedOverlay` Monet 鍵值的完整參考表。 |
| **[AYN Thor 硬體參考](docs/devices/AYN_THOR.md)** | 完整硬體規格、AMOLED 多螢幕（`displayId = 0` 與 `4`）、DDIC TE 時脈比、OLED 熄滅機制、溫控／風扇／RGB sysfs、Binder IPC，以及 `DeviceProfile.AYN_THOR` 規格。 |
| **[Retroid Pocket Duo 硬體參考](docs/devices/RETROID_POCKET_DUO.md)** | 完整硬體規格、Android 16（`SDK 36`）、`4:3` 副螢幕（`displayId = 2`）、`bl_power` 背光熄滅、`CQ7790S` 溫控／sysfs、`ILauncherProxy` / `com.rp.*` IPC，以及 `DeviceProfile.RETROID_POCKET_DUO` 規格。 |
| **[裝置硬體設定檔範本](docs/devices/DEVICE_PROFILE_TEMPLATE.md)** | 供未來支援新雙螢幕掌機時使用的標準化 10 節式硬體與韌體文件範本（附唯讀 ADB 探索指令）。 |
| **[UI 架構、自訂元件與主題化](docs/UI_AND_THEMING.md)** | 標準 `720dp` `OverlayViewport` + `TccLookSpec`（`AYN Smooth` 與 `Retroid Pixel`）規格、非 Material 自訂畫布元件（`HorizontalSlidingBar`、`HardwareRingGauge`、`FrameRatePanelCard`、`BatteryIconCanvas`）、全部 12 種 `AppThemeOption` 色盤，以及內嵌 `com.stormpanda.megingiard` 手把設計系統。 |
| **[建置、發布與開發者指南](docs/BUILD_AND_RELEASE.md)** | 開發者專用指南，包含金鑰庫簽章設定（`local.properties`）、Debug 建置（`scripts/debug.sh`）、Release Git-Flow 自動化（`scripts/release.sh`）、SHA-256 校驗（`scripts/generate_checksum.sh`）與 ADB 檢查指令。 |
| **[貢獻指南](CONTRIBUTING.md)** | 給貢獻者的架構規範、多螢幕與特權硬體不變量、程式碼慣例，以及授權條款遵循說明。 |
| **[AI 代理指南](AGENTS.md)** | 給參與本專案之 AI 程式設計代理的核心背景知識、架構不變量、硬體特性與工作流程規範。 |

---

## 核心功能

### 1. 下螢幕助手懸浮覆蓋層（AYN Thor 為 `displayId = 4` `1240 × 1080` • Retroid Pocket Duo 為 `displayId = 2` `1280 × 960`）
- **模式分頁（`Tab 0`）**：
  - **螢幕模式控制**：可在**雙螢幕顯示**（`0`）、**僅上螢幕**（`1`）與**僅下螢幕**（`2`）之間切換，並在熄滅螢幕時完整覆蓋狀態列／手勢列；另可選用一鍵**互換螢幕（Swap Screens）**按鈕，將上、下螢幕目前運作中的前景應用程式任務對調。
  - **亮度滑桿**：連動式**雙螢幕**、**上螢幕**、**下螢幕**亮度調整（拖曳時透過硬體 `setTemporaryBrightness` 達成零延遲調整，並與外部 SystemUI 快速設定滑桿同步），另提供選用的**類比搖桿 LED** 亮度滑桿（AYN Thor 支援 `0%–100%`）。
  - **音量滑桿**：連動式**雙螢幕**、**上螢幕**（`STREAM_MUSIC`）與**下螢幕**（`secondary_screen_volume_level`）音量調整。
- **設定分頁（`Tab 1`）**：
  - **幀率面板與精準遊戲 FPS**：一鍵切換 **`60 HZ MODE` / `120 HZ MODE`**，並搭配**精準遊戲 FPS 計數器**，直接量測前景遊戲／應用程式的真實畫面輸出（Android 14–16 使用 `ITaskFpsCallback`，並針對 AYN Thor 的 AMOLED DDIC TE 時脈校準逐圖層 `SurfaceFlinger` 時間戳記），亦可隨時切換回原廠全域 `SurfaceFlinger` 計數器。
  - **24 個可自訂快速設定磚**：採分頁式 `2 × 4` 網格排列（支援觸控滑動或 **`L1` / `R1`** 肩鍵翻頁，並可選擇開啟**記住上次開啟分頁**與**記住上次快速開關頁面**）：
    - **原廠廠商開關（最多 10 個 — 預設啟用）**：*按鍵焦點鎖定（Key Focus Lock）*（切換螢幕模式時保留 `HOME`/`BACK` 焦點鎖定）、*效能模式*、*風扇控制*（關閉／安靜／智慧／運動／自訂 `35%–100%`）、*清除記憶體*、*控制器樣式*（停用／Xbox／Odin 或 Retro）、*充電分離*、*震動*、*LED 燈效*（AYN Thor）、*強制橫向*與*主題模式*。
    - **Android 系統開關（4 個 — 選用）**：*Wi-Fi*、*藍牙*、*勿擾模式*與*飛航模式*。
    - **實用工具（最多 10 個 — 選用）**：*觸控鎖定*、*螢幕截圖*（上螢幕／下螢幕／雙螢幕同時擷取並儲存至 `/sdcard/Pictures/Screenshots`）、*螢幕錄影*（硬體 `MediaCodec` H.264 + `48 kHz` AAC 錄製，支援上螢幕、下螢幕或經 OpenGL ES GPU 合成的**雙螢幕畫面**，可選 `30`/`60 FPS`，並具備特權**遊戲內部音訊**迴路錄音 + **麥克風**混音，儲存至 `/sdcard/Movies/ScreenRecordings`）、*扳機模式*（類比／數位／雙模式）、*80% 充電上限*、*滑鼠模式*、*按鍵映射器*、*保持螢幕開啟*、*Home 鍵鎖定*（AYN Thor）與*清除所有任務*。
  - **即時硬體遙測面板**：即時顯示 **SoC 溫度**（支援動態溫度變色，可於設定中或直接點擊 `TEMP` 數值切換 **`°C` / `°F`**）、**風扇轉速百分比**（`IFanControlProvider`），以及 4 個 `270°` 動態圓形儀表環，分別顯示 **CPU 頻率**（可設定以**硬體最大值**或目前**效能模式上限**為基準）、**GPU 頻率**（`kgsl-3d0`）、**電池功耗**（`-20W..+25W`）與 **RAM 使用量**。
- **頂部狀態列、底部導覽列與特權視窗層級**：
  - 狀態列包含即時時鐘（點擊可清理記憶體）、錄影時的互動式**錄影計時／停止膠囊按鈕**、藍牙圖示、即時網路下載速度（`KB/S` / `MB/S`）、Wi-Fi 訊號格數，以及附帶充電閃電標誌的自訂電池圖示。
  - 底部導覽列包含**模式**、**任務**（透過 `IOverviewProxy` / `ILauncherProxy` 開啟下螢幕最近使用的應用程式）、**設定**，以及選用的**編輯**（在上螢幕開啟隨行設定應用程式的同時保持下螢幕覆蓋層開啟）。
  - 透過特權視窗類型 `2021` 進行繪製，確保 Android 的 `HIDE_NON_SYSTEM_OVERLAY_WINDOWS` 安全旗標（如檔案管理員、系統設定、權限對話框）絕不會隱藏助手或意外點亮已關閉的螢幕，同時能與原廠 `GameAssistant` 邊緣滑動手勢和平共存。

### 2. 上螢幕隨行設定應用程式（位於 `displayId = 0` 的 `MainActivity`）
- **手把操作優先的雙欄介面**：使用 **十字鍵（D-Pad）/ 左類比搖桿**瀏覽，以 **`L1` / `R1`** 切換分類，搭配 **`L2` 微調修飾鍵**精細調整滑桿，所有變更皆可即時在下螢幕（`displayId = 4` / `2`）預覽。程式本身會自動從最近使用的應用程式列表中排除，並在離開時自動結束以釋放背景 RAM。
- **視覺化快速開關排序**：精巧的互動式 `2 × 4` 分頁版面預覽，同時支援觸控點擊移動與手把抓取移動（按 `A` 抓取、D-Pad/`L1`/`R1` 即時移動、按 `B` 放下），並提供分類式的開關顯示／隱藏管理。
- **跨裝置介面風格、豐富主題與 Android 13–16 Monet 注入**：
  - **覆蓋層介面風格**：可在任意配色下自由選擇 `自動`、**AYN Smooth** 與 **Retroid Pixel**，並可開啟**提升 Retroid 可讀性**以放大設定磚、滑桿、遙測數值與對話框的像素字體大小。
  - **主題色盤**：提供**原廠樣式**（`跟隨系統`、`淺色`、`深色`）、**8 款內建自訂色盤**（`Dark`、`Dark OLED`、`Norse Forest`、`Mjölnir Steel`、`Valhalla Sunset`、`Aurora Borealis`、`Retro Phosphor`、`Royal Asgard`）搭配 **13 種強調色預設**與**自訂 HSV 檢色器**，或選擇**與 Megingiard 同步**（當裝置安裝有 Megingiard 時會自動顯示）。
  - **將主題色彩注入系統（Inject Theme Colors into OS）**：產生一組包含 221 個色彩的 Android 13–16 動態語意 Monet `FabricatedOverlay` 覆蓋層（保留完整快速設定強調色飽和度），將主題套用至全系統的 Android SystemUI、快速設定、通知中心、系統設定與 Material You 應用程式。
- **App 內更新、升職彩蛋與授權條款**：
  - **更新分頁**：每 24 小時自動檢查 GitHub Release（附 App 內更新橫幅提示）、支援手動**檢查更新**，以及一鍵透過 Deep-Link **新增至 Obtainium**。
  - **升職為管理員（Promote to Manager）**：一般設定中的趣味選項（*"Good job, assistant! You get a promotion!"*），開啟後會將桌面與介面上的 App 名稱變更為 **Better Dual Screen Manager**。
  - **App 內授權檢視與除錯診斷**：提供支援手把捲動的文件閱讀器以查閱本軟體授權條款與內建第三方字型／函式庫聲明；Debug 建置版本另提供**即時系統診斷**分頁，顯示即時狀態徽章並支援一鍵重新連線 `PrivilegedHelperMain (UID 0)`、`PServerBinder`、原廠 `DualScreenAssistant` 抑制狀態與 Megingiard IPC。

### 3. Direct Boot、安全無痕的原廠抑制與解除安裝自動復原
- **Direct Boot 支援**：由裝置加密儲存空間（`DirectBootPrefs`）支援，在 `LOCKED_BOOT_COMPLETED` 階段即自動啟動，確保開機後尚未解鎖螢幕前，實體按鍵、螢幕模式與使用者偏好設定就能立即生效。
- **零殘留原廠抑制**：僅在記憶體中暫停（`SIGSTOP`）常駐的原廠 `com.odin.dualscreen.assistant` / `com.retroidpocket.dualscreen.assistant` 行程，不會在磁碟上將其套件或 `TccServices` 設為停用狀態。
- **持續性監控與自動復原**：運作期間，`PrivilegedHelperMain`（`UID 0`）會持續監控並在遭到清除工作、清理 RAM 或 `force-stop` 時立即重新喚起 `AssistantForegroundService`。一旦 **Better Dual Screen Assistant** 遭到**解除安裝**、**停用（`pm disable`）**，或使用者取消勾選 **取代原廠助手（Replace Stock Assistant）**，看門狗機制會自動重新點亮雙螢幕（包含 Retroid Pocket Duo 的核心層 `bl_power`）、恢復螢幕亮度與觸控功能（`sys.temp.disabled.touch = "0,0"`）、移除已注入的系統主題覆蓋層，並喚醒／重啟原廠 `DualScreenAssistant`。

---

## 快速開始（建置與安裝）

請先確認您的 AYN Thor 或 Retroid Pocket Duo 已開啟 USB 偵錯並連線（`adb devices`）：

```bash
# 單一指令建置並安裝 Debug APK：
./scripts/debug.sh build-install

# 或建置並安裝經過混淆最佳化的 Release APK：
./scripts/release.sh build-install

# 透過 ADB 立即啟動前景服務：
adb shell am start-foreground-service \
  -n com.stormpanda.betterdualscreenassistant/.service.AssistantForegroundService
```

有關簽章金鑰庫設定、Release 分支自動化與疑難排解指令，請參閱 [`docs/BUILD_AND_RELEASE.md`](docs/BUILD_AND_RELEASE.md)。

---

## 授權條款

Better Dual Screen Assistant 為專有且原始碼開放檢視（Source-Available）的專案，採用自訂的 **Better Dual Screen Assistant Source-Available License (Version 1.0)** 授權。

- **原始碼與執行檔：** 您獲准檢視原始碼、進行編譯，並僅限於**個人非商業用途**執行本應用程式。
- **禁止事項：** 未經事前書面同意，嚴禁任何商業利用、散布修改後的原始碼／執行檔，或預載／搭售於商業硬體與裝置上。
- **開源轉換承諾：** 若本專案在無接任維護者的情況下永久停止積極開發，著作權人承諾將整個程式碼庫轉換為完全寬鬆、經 OSI 核准的開放原始碼授權條款（例如 MIT 或 Apache 2.0），並賦予完整的再散布權利。

完整條款與細則請參閱專案根目錄的 [LICENSE](LICENSE) 檔案；內建開源字型與函式庫之歸屬聲明請參閱 [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md)；提交貢獻前請先閱讀 [CONTRIBUTING.md](CONTRIBUTING.md)。
