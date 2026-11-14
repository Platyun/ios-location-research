# 研究與發布版: 整合總結

Customized by Platyun · 2026-11-15

## 結論

**同一台 iPhone 16 Pro 先在 iOS 27.0.1 實測通過, 之後把系統更新到 iOS 27.2.1 再複測, 仍然成功, 且使用者明確確認沒有重開整機.** (U-06, U-01, U-02)

- 「失效後不重開整機, 讓原本的定位工具重新套用位置」在這台裝置的兩個系統版本上都有成功回報, 經歷一次系統更新後仍可重現.
- 還不能確定成功是由送出請求造成的 (沒有不送請求的對照), 也沒有證實 locationd 重新啟動, 基地台快取被清除, 或其他裝置與版本適用.
- 成功維持多久不列為門檻. 這是使用者對本次目標的界定, 不代表長期穩定性已測過.

## 1. 來源與版本

| 來源 | 固定版本 | 本文用途 |
| --- | --- | --- |
| [itool-fix (研究起點)](https://github.com/Platyun/itool-fix/tree/5ede53336972543a4c79cb5540029ed837b82146) | 5ede533 | 早期距離限制研究, 研究交接, 探測 App 與首次真機紀錄, 工程驗證回報. |
| [發布版 repo](https://github.com/Platyun/reset.loacation.by.Platyun/tree/5790b4f0d18257b6f293ab005b12d94c62efec08) | 5790b4f | 發布程式, 操作手冊, 側載說明與下載包. |
| 使用者直接補充 | 2026-11-15 收到 | U-01~U-06, 見第 3 節. |

發布版遠端在基線之後多一個提交 [3402ad0](https://github.com/Platyun/reset.loacation.by.Platyun/commit/3402ad02bcf3bb979f5c68b8cf664ed6e2110c5f), 只把 README 測試環境由「iOS 27.0.1」改為「iOS 27.0.1, 27.2.1」, 其餘檔案不變. 本文的發布版判讀仍以 5790b4f 為準, 凡受此提交影響的地方另行註明. itool-fix 遠端沒有比 5ede533 更新的提交.

本 repo 的舊版文件 ([8c3651f](https://github.com/Platyun/ios-location-research/tree/8c3651f6c719876d15dfb9274e5840f6a57a48fd)) 是被審查的整理稿, 不能當作獨立證據引用.

## 2. 研究起點與問題演變

| 日期 | 階段 | 依據 |
| --- | --- | --- |
| 2026-11-10 | 使用 iTools BT 2.5 外接定位配件時, iOS 18 的設備可遠距離選點, iOS 27 的設備約 20 km 外失敗. 研究先查 Shadowrocket 配置, DNS, TLS pinning 與定位來源. | [itool-fix 早期研究](https://github.com/Platyun/itool-fix/blob/5ede53336972543a4c79cb5540029ed837b82146/docs/ios27-itools-20km-research.md?plain=1#L3) |
| 2026-11-10 | 使用者對照: 在 iOS 27 設備上, 關閉 Shadowrocket, DIRECT, REJECT, bypass-system 結果都一樣, 所以單靠配置不能解釋版本差異. | [同文 A-1](https://github.com/Platyun/itool-fix/blob/5ede53336972543a4c79cb5540029ed837b82146/docs/ios27-itools-20km-research.md?plain=1#L14) |
| 2026-11-11 前 | 套用阻斷配置並重開整機後, 遠距離選點曾成功; 之後失效時, 不重開就無法重新套用. 舊 iOS 只要關再開定位服務. 問題因此改成「能否不重開整機就重新套用」, 也不再限定 BT 2.5. | [研究交接 L15–L19](https://github.com/Platyun/itool-fix/blob/5ede53336972543a4c79cb5540029ed837b82146/Platyun-Location-Service-Probe/HANDOFF.md?plain=1#L15) |
| 2026-11-11 | 舊版 runtime header 有 `shutdownDaemon` 類別方法, 做成探測 App: 先檢查方法與回傳簽章, 再由使用者手動送出. | [研究交接 L25](https://github.com/Platyun/itool-fix/blob/5ede53336972543a4c79cb5540029ed837b82146/Platyun-Location-Service-Probe/HANDOFF.md?plain=1#L25) |
| 2026-11-14 | iOS 27.0.1 首次真機: 方法存在, 呼叫返回 YES, 之後使用者回報重新套用成功. | E-01 |
| 2026-11-14 | 發布版 ResetLocation: 確認框, 10 秒冷卻, 小工具, 未簽名 IPA. | 第 5 節 |
| 2026-11-15 收到 | 同一台手機更新到 iOS 27.2.1 後複測成功, 沒有重開整機. | U-01, U-02, U-06 |

早期的「約 20 km」「約 30 秒回跳」是使用者觀察到的數值, 沒有證據顯示它們是系統固定常數; 後來遠距離選點成功, 也說明它不是無法跨越的限制. 早期研究對「限制由系統層還是控制 App 層施加」沒有下結論, 這個問題在研究目標改變後擱置, 沒有被回答.

## 3. 證據紀錄

### 3.1 使用者直接補充

摘錄已脫敏. 日期是收到補充的日期, 不代表實際測試的精確時間; 內容是使用者自述, 不是第三方驗證.

| ID | 直接摘錄 | 支持的範圍 |
| --- | --- | --- |
| U-01 | 「iOS 27.2.1」「iPhone 16 Pro」「實測成功」 | 裝置, 系統版本與成功回報. |
| U-02 | 「沒重開 維持多久應該不是重點」 | 明確未重開; 持續時間不列為門檻. |
| U-03 | 「要去根據研究 repo 以及 發布版 repo 都做總結」 | 總結範圍包含兩個 repo. |
| U-04 | 「可以的話用 HTML 比較好」「md 也一份可以在 github 直接讀」 | HTML 與 Markdown 兩種格式. |
| U-05 | 「記得 HTML 放 README 比較好點」 | README 要有 HTML 入口. |
| U-06 | 「27.0.1 實測通過後 更新成 27.2.1 複測成功」 | 兩筆是同一台裝置的先後測試: 先測, 更新系統, 再複測. |

### 3.2 事件與已知欄位

| 欄位 | E-01: iOS 27.0.1 | E-02: iOS 27.2.1 |
| --- | --- | --- |
| 來源 | [研究交接 L31–L55](https://github.com/Platyun/itool-fix/blob/5ede53336972543a4c79cb5540029ed837b82146/Platyun-Location-Service-Probe/HANDOFF.md?plain=1#L31), 是 App 匯出紀錄的轉錄; 原附件不在本 repo. | U-01, U-02, U-06. |
| 裝置 | iPhone 16 Pro (匯出紀錄沒有機型欄位, 機型由使用者後續補充). | iPhone 16 Pro, 與 E-01 是同一台 (U-06). |
| 安裝產物 | 使用者用個人 Team 從 Xcode 安裝的探測 App; 確切 build 未記錄. | 未知. 不知道更新系統後是否重新安裝, 也不知道是哪個 build 或發布版 IPA. |
| 送出路徑 | App 按鈕 (當時還沒有小工具). | 未知 (App 或小工具). |
| 實際操作 | 方法存在檢查 → 手動呼叫一次 → 依原流程重新套用. | 使用者稱為「複測」, 即重複同一測試; 逐步順序未記錄. |
| 原始輸出 | 方法存在, 回傳型別 `B`, 大小 1; 正常返回 YES, 原始值 1. | 未提供. **不能沿用 E-01 的 YES.** |
| 按下前是否已失效 | 未記錄. | 未記錄. |
| 是否重開整機 | 依「不重開」的測試指示進行; 使用者沒有逐項確認. | 明確沒有重開 (U-02). |
| 結果 | 使用者回報「成功了」. | 使用者回報「複測成功」. |
| 持續時間 | 不列為門檻. | 不列為門檻. |

### 3.3 工程與封裝紀錄

| ID | 材料 | 能支持什麼 | 不能支持什麼 |
| --- | --- | --- | --- |
| E-03 | [既有工程驗證回報](https://github.com/Platyun/itool-fix/blob/5ede53336972543a4c79cb5540029ed837b82146/handoff/codex-to-claude.md?plain=1#L23): 共用 gate 的 40 項假後端測試, 未簽名 App+小工具建置成功. | 節流邏輯與建置當時通過. | 私有介面效果, 小工具背景執行或 iOS 程序查詢的真機結果. 本次沒有重跑. |
| E-04 | 本次重跑: 發布版 4 筆 SHA-256 全部吻合; 4 份 IPA 在兩個 repo 位元相同. | 下載檔符合校驗檔, 兩端產物一致. | 能用任何側載工具安裝, 或安裝後的行為. |

## 4. 結果分層

| 層級 | 問題 | 目前判斷 |
| --- | --- | --- |
| 1 | 不重開整機能不能重新套用? | **可以, 有兩筆成功回報**, 跨一次系統更新 (E-01, E-02). |
| 2 | 是從已確認的失效狀態恢復嗎? | 未記錄. 兩筆都沒寫下按下前是否已失效. 不影響第 1 層. |
| 3 | 是送出請求造成恢復嗎? | 未確認. 沒有「同一失效狀態下不送請求」的對照. |
| 4 | 機制是 locationd 重啟或基地台快取清除嗎? | 未證實. 沒有程序 ID 變化, 系統日誌或快取內容的證據. |

兩筆來自同一台裝置, 不能當成兩個獨立樣本, 也不能計算成功率. 但它們確實說明: 這套做法在 iOS 27.0.1 有效, 更新到 27.2.1 之後仍然有效, 沒有因系統更新而失效.

## 5. 發布版能力與驗證狀態

發布版 [後端](https://github.com/Platyun/reset.loacation.by.Platyun/blob/5790b4f0d18257b6f293ab005b12d94c62efec08/PlatyunLocationProbe/PLShutdownBackend.m?plain=1#L8) 先檢查方法與回傳簽章, 送出前再驗證一次, 再由 runtime 呼叫. 紀錄只寫正常返回, 零/非零或例外, 不讀座標, 不自動判定定位成功.

| 功能 | 已實作 | 工程檢查 | 真機回報 | 尚未知道 |
| --- | --- | --- | --- | --- |
| App 送出請求 | 是 | 建置成功 (E-03) | E-01 (27.0.1, 探測 App) | E-02 用的是不是發布版 |
| 確認框, 10 秒冷卻 | 是 | 假後端測試 (E-03) | 無專門紀錄 | 真機倒數畫面 |
| 小工具背景送出, 不開 App | 是 ([intent](https://github.com/Platyun/reset.loacation.by.Platyun/blob/5790b4f0d18257b6f293ab005b12d94c62efec08/Shared/ReapplyLocationIntent.swift?plain=1#L10)) | 建置成功, gate 測試 | 無 | 小工具程序能否呼叫, 是否真的不開 App |
| App 與小工具共用冷卻 | 是 | 40 項假後端測試 | 無 | iOS 上的檔案鎖與程序查詢 |
| 未簽名 IPA (兩種) | 是 | 校驗碼吻合 (E-04) | 無 | Sideloadly/AltStore 安裝, 小工具重新簽名 |
| locationd 重啟/快取清除 | 不適用 | 不適用 | 無 | 全部 |

App 成功不能推定小工具也成功; 最新成功回報沒有指明安裝方式, 所以不能用來勾選側載驗證.

## 6. 假說與區辨方法

| 假說 | 若成立應看到 | 看到什麼會削弱它 | 狀態 |
| --- | --- | --- | --- |
| H1: 送出請求促成重新套用 | 同一失效狀態下, 只走原流程失敗; 加送一次請求後成功. | 不送請求, 只走原流程也一樣成功. | 可測, 未測. |
| H2: 關再開定位, 重連配件或等待就足夠 | 不送請求也能恢復. | 只做這些持續失敗, 加請求才成功. | 可測, 未測. 舊 iOS 確實只需關開定位. |
| H3: 請求讓 locationd 重新啟動 | Mac 主控台看到裝置 locationd 退出/啟動或 PID 改變. | 送出後 PID 不變, 仍重新套用成功. | 可測, 未測. |
| 研究方向: 基地台快取 | 目前在未越獄 iOS 上沒有可直接觀察快取內容的方法. | — | 只是研究方向, 不是可測假說. |
| 研究方向: 約 20 km 由系統層或控制 App 層施加 | 早期研究提出藍牙封包擷取等鑑別實驗. | — | 目標改變後擱置. |

想要確認 H1~H3 時, 在自然發生的失效狀態下做: 先只走原流程並記錄結果, 失敗後再送一次請求; 同時開 Mac 主控台過濾 locationd. 不需要為了這份總結要求使用者重測.

## 7. 發布流程如何理解

[發布手冊](https://github.com/Platyun/reset.loacation.by.Platyun/blob/5790b4f0d18257b6f293ab005b12d94c62efec08/docs/manual/操作手冊.md?plain=1#L43)寫的是作者自己的操作順序: 連接 Shadowrocket → 連接兩個藍牙 → 關閉定位 → ResetLocation → 開啟定位. 這是已成功的流程, 但沒有逐一證明每一步都必要; 別人的配件數量或網路環境不同時, 不能直接當成必要條件.

## 8. 已知文件差異

| 原 repo 位置 | 問題 | 待修方式 |
| --- | --- | --- |
| 研究 App README [L23](https://github.com/Platyun/itool-fix/blob/5ede53336972543a4c79cb5540029ed837b82146/Platyun-Location-Service-Probe/README.md?plain=1#L23) 與 [L69](https://github.com/Platyun/itool-fix/blob/5ede53336972543a4c79cb5540029ed837b82146/Platyun-Location-Service-Probe/README.md?plain=1#L69) | 一處說強制結束會重置冷卻, 一處說不會. 兩者其實分別對應「無 App Groups 建置」 (冷卻只存在程序內) 與「有 App Groups」 (冷卻存到共用檔), 但文中沒寫適用模式. | 兩句各標明適用的建置模式. |
| 研究 App README L7 與 L11 | L7 已有方法存在與返回紀錄, L11 仍寫尚未確認 iOS 27 是否保留此介面. | L11 改為歷史敘述. |
| 發布 README [L23](https://github.com/Platyun/reset.loacation.by.Platyun/blob/5790b4f0d18257b6f293ab005b12d94c62efec08/README.md?plain=1#L23) | 功能清單寫小工具「直接在背景送出, 不開 App」; 「仍需真機確認」的說明在 L37 另一節. | 把驗證狀態寫在功能旁. |
| 發布手冊 [L104](https://github.com/Platyun/reset.loacation.by.Platyun/blob/5790b4f0d18257b6f293ab005b12d94c62efec08/docs/manual/操作手冊.md?plain=1#L104) | 「處理中或冷卻中顯示倒數」; 程式中處理中顯示「正在送出請求…」, 只有冷卻顯示倒數. | 兩種狀態分列. |
| 下載 ZIP 內 README | 4 個相對連結 (兩份 IPA, 側載說明, 校驗檔) 指向 ZIP 內不存在的檔案. 本次解壓重跑確認. | 改為 ZIP 內有效的連結或線上固定版本連結. |
| 研究 README 測試環境 | 只寫 iOS 27.0.1; 發布 README 已在 3402ad0 補上 27.2.1. | 補上 27.2.1, 並註明是同一台更新後複測. |

## 9. 可直接引用的總結

「本研究起於 iOS 27 上外接定位配件約 20 km 的選點限制, 後來改為研究失效後能否不重開整機重新套用位置. 研究做出手動送出定位服務請求的實驗 App. 使用者以同一台 iPhone 16 Pro 在 iOS 27.0.1 實測通過, 更新到 iOS 27.2.1 後複測仍成功, 且沒有重開整機. 依使用者回報, 此操作在該裝置上可行, 並在系統更新後仍可重現; 但還沒有確認成功是否由送出請求造成, 也沒有證實 locationd 重啟或基地台快取清除. 小工具, 跨程序冷卻與側載安裝的驗證狀態要分開看, 不能由 App 成功一併推定.」
