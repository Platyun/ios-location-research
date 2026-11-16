# iOS 定位服務研究

Customized by Platyun

## 使用前請先了解

本研究與相關工具使用的 `CLLocationManager shutdownDaemon` 是非公開 API，不是 Apple 提供給一般 App 使用的官方公開 API，不保證在不同裝置或系統版本上可用，行為也可能隨系統更新改變。

本專案透過 GitHub 等非官方管道分享，並非 Apple 官方發布或認可的工具；在 GitHub 上架不代表經過 Apple 的安全審核。下載、簽名、安裝或使用前，請自行評估風險。

若有疑慮，建議將整個 repo 交給 AI 協助檢視，了解程式行為、權限需求與可能風險；若 AI 無法直接讀取 repo，可提供完整專案檔案。AI 的檢視結果僅供參考，不代表安全保證，也不能保證下載的安裝包與程式碼一致。

**這是讓 iOS 27 在使用 4G / 5G 行動數據時, 也能重新套用定位的方法, 不必為此改接 Wi-Fi, 也不必重開整支手機.** 工具本身見 [ResetLocation](https://github.com/Platyun/reset.location.by.Platyun).

研究未越獄 iPhone 能否不重開整機, 讓原本的定位工具重新套用位置. 本 repo 只保存脫敏研究文件, 不包含程式碼, 配置, 安裝包, 圖片或開發交接紀錄.

## 閱讀入口

| 文件 | HTML 閱讀版 | GitHub 直接閱讀 |
| --- | --- | --- |
| **研究主文：不重開 iPhone，能不能重新套用定位？** | [開啟 HTML](docs/cross-repo-summary.html) | [閱讀 Markdown](docs/cross-repo-summary.md) |

HTML 檔在 GitHub 上會顯示原始碼, 請下載後用瀏覽器開啟; 不需網路. 這些 HTML 沒有部署成網站. Markdown 可直接在 GitHub 閱讀, 兩種格式內容相同.

## 結論

**同一台 iPhone 16 Pro 先在 iOS 27.0.1 實測通過, 更新到 iOS 27.2.1 後複測仍成功, 且沒有重開整機. 另一台 iPhone 18 Pro Max 在 iOS 27.2 也實測成功，並已確認沒有重開整機。**

這表示「不重開整機重新套用位置」在不只一台裝置上可行, 並在系統更新後仍可重現. 目前還沒有確認成功是否由送出請求造成, 也沒有證實 locationd 重啟或基地台快取清除. 成功維持多久不列為本次門檻.

## 研究起點

研究起於 [itool-fix](https://github.com/Platyun/itool-fix) 的早期研究 (2026-11-10): 使用 iTools BT 2.5 時, iOS 27 約 20 km 外選點失敗, iOS 18 則正常. 之後問題轉為「失效後能否不重開整機就重新套用」, 並做出探測 App 與發布版 ResetLocation. 完整研究過程與證據見整合總結。

## 歷史基線文件

以下是 2026-11-15 第一版整理稿, 保留供追溯, 不再單獨維護. 內容有出入時以整合總結為準.

- [個案研究稿](docs/case-study.md)
- [研究結論](docs/conclusion.md), [實測證據](docs/evidence.md), [研究脈絡](docs/research.md), [可選後續驗證](docs/verification.md), [來源與整理方式](docs/sources.md)

更新日期: 2026-11-15.
