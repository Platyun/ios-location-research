# 來源與整理方式

Customized by Platyun

> **歷史基線**：這是 2026-11-15 第一版整理稿（[8c3651f](https://github.com/Platyun/ios-location-research/tree/8c3651f6c719876d15dfb9274e5840f6a57a48fd)），保留供追溯。現行內容以[整合總結](cross-repo-summary.md)為準。複審只更正兩處事實：研究起點是 itool-fix 的早期距離限制研究；iOS 27.2.1 是同一台手機在 27.0.1 實測通過後更新系統再做的複測。

## 研究來源

本次依 Platyun 的既有私人研究 repo 整理，來源版本為 5ede53336972543a4c79cb5540029ed837b82146，主要文件為最新研究交接、[早期距離與網路配置研究報告](https://github.com/Platyun/itool-fix/blob/5ede53336972543a4c79cb5540029ed837b82146/docs/ios27-itools-20km-research.md)（研究起點，2026-11-10）、探測實驗說明。再加入使用者於 2026-11-15 補充的回報：同一台 iPhone 16 Pro 在 iOS 27.0.1 實測通過後更新到 iOS 27.2.1，複測成功且未重開。

原始交接與開發歷程未搬入新 repo。新回報獨立列項，沒有回填成舊版匯出紀錄。本文是依證據重新編排的研究整理，不宣稱逐字完整保存原稿。

## 外部資料

| 來源 | 支持的範圍 | 不支持的推論 |
| --- | --- | --- |
| [舊版 CLLocationManager runtime header](https://github.com/nst/iOS-Runtime-Headers/blob/master/Frameworks/CoreLocation.framework/CLLocationManager.h) | 舊版介面調查線索。 | 不能單靠 header 證明目前系統支援或呼叫效果。 |
| [iOS 13 對應 header](https://github.com/xybp888/iOS-Header/blob/master/13.0/Frameworks/CoreLocation.framework/CLLocationManager.h) | 原研究引用的歷史介面資料。 | 不代表新系統的權限或實作相同。 |
| [wloc](https://github.com/Yu9191/wloc) | 維護者對其網路定位修改流程的相容性及快取說明。 | 不直接證明本裝置的底層原因。 |
| [wloc issue 113](https://github.com/Yu9191/wloc/issues/113) | 原研究引用的 MITM 與代理觀察。 | 不推廣為所有系統版本、所有路徑的保證。 |
| [wloc issue 130](https://github.com/Yu9191/wloc/issues/130) | 原研究記錄的社群報告。 | 沒有在本研究裝置重現，不列為已證實事實。 |
| [Apple 定位來源資訊](https://developer.apple.com/documentation/corelocation/cllocationsourceinformation) | 定位來源資訊的官方參考。 | 有來源標記不等於有固定距離限制。 |
| [Apple includeAllNetworks](https://developer.apple.com/documentation/networkextension/nevpnprotocol/includeallnetworks) | 網路延伸路由行為的官方參考。 | 不證明本次定位流量一定繞過 VPN。 |
| [GeoShim](https://github.com/regulad/GeoShim) | 原研究參照的越獄環境方案。 | 不能推定普通簽名 App 有相同權限。 |
| [APOLLO 歷史定位資料模組](https://github.com/mac4n6/APOLLO/blob/master/modules/locationd_cacheencryptedAB_ltecelllocationlocal.txt) | 歷史定位資料庫研究線索。 | 不代表現代 iOS 的資料庫內容或清理方法已確認。 |

2026-11-15 整理時重新開啟舊版 runtime header 與 wloc repo 作來源核對；Apple 頁面僅回傳需 JavaScript 提示，沒有據此新增技術結論。其餘連結保留為原研究引用，未於此次逐項重新核實。廠商支援版本與商業評測內容未作為本次結論依據。

## 脫敏與交付範圍

只保留 Markdown 研究文件；移除本機絕對路徑、帳號 Email、Team ID、附件識別碼、精確測試時間、實際選點地名及協作通知。保留作者署名、裝置型號、系統版本與研究日期，因為它們是使用者明確提供且與研究結論相關的資訊。

不包含 App 原始碼、測試程式、腳本、配置、IPA、ZIP、Xcode 個人狀態或舊 Git 歷史。新提交使用作者 Platyun 與不含個人 Email 的提交身分。
