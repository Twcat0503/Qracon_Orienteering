# Qracon Orienteering｜QRACON 2026 量子冒險者

把 QRACON 2026 大會變成一場個人探索遊戲：參加者登入後成為「量子冒險者」，在知識地圖上參與議程、完成任務、累積 QXP、升級，最後帶走一枚可感應回顧成果的 NFC 徽章。

| | |
| --- | --- |
| 活動 | 2026-12-13（日）· 中央研究院南部院區（臺南）· 預估 500 人 |
| 距活動 | **D-33**（2026-11-10 起算） |
| 文件狀態 | 需求已整理、計畫已排、19 項待決（其中 8 項阻擋開發） |
| 程式狀態 | 尚未開工（本 repo 目前只有文件） |

---

## 文件索引

| 文件 | 內容 | 讀者 |
| --- | --- | --- |
| [`docs/00-requirements.md`](docs/00-requirements.md) | 整理後的需求：體驗流程、等級、地圖、計分、交流版、獎勵、營運、驗收條件 | 所有人 |
| [`docs/01-open-questions.md`](docs/01-open-questions.md) | 19 個待決問題 + 建議 + 決策者 + 期限；決策日誌 | 總召、組長 |
| [`docs/02-development-plan.md`](docs/02-development-plan.md) | 範圍（MoSCoW）、5 個 Sprint、關鍵路徑、風險表、崩潰計畫、本週待辦 | 工程、活動 |
| [`docs/03-architecture-and-data-model.md`](docs/03-architecture-and-data-model.md) | 架構、完整 DDL、計分引擎規格、API、AI 審核、徽章 token、壓測 | 工程 |
| [`docs/04-figma-board-review.md`](docs/04-figma-board-review.md) | Figma 設計板（2026-11-03 精簡版）頁面清單與規格落差、5 項待確認矛盾 | 設計、工程、總召 |
| [`docs/source-hackmd.md`](docs/source-hackmd.md) | 原始 HackMD 快照（未編輯，僅供追溯） | 所有人 |

---

## 三個最重要的判斷

**1. 真正的瓶頸是採購與審核，不是寫程式。**
NFC 徽章必須在 **11/20 下單**（設計稿 11/18 定稿），否則 12/13 拿不到。重點彙整的人工審核若照 500 人 × 30% 投稿 × 15 分鐘計算，需要 37.5 人時，現場只有約 16 人時 → 必須改成「AI 判分為主、人工只審邊界」。

**2. SSS 的 400 分幾乎等於「每場都寫心得」。**
以 11/10 議程草案（16 場可計分）估算，認真型參加者約 270 分、全力型約 410 分。建議把門檻做成設定值，12/01 議程凍結後用實際場次重算，必要時下修至 350。

**3. 地圖不能用「手畫 SVG + 硬連線」。**
原文自陳「議程一多版面會亂」。改成資料驅動：三區域 → 標籤群 → 議程節點，預設只展開一層。這樣議程異動不用重畫。

---

## 需要你現在決定的三件事

| 決策 | 為什麼卡 |
| --- | --- |
| **登入方式**（建議：報到代碼 + Google 備援） | 決定帳號模型與報到動線，最高風險項 |
| **彙整審核量能**（建議：AI 為主、人工只審邊界） | 決定 SSS 能不能在現場結算、配件怎麼發 |
| **網域**（建議：`quest.2026-qracon.entangletech.tw`） | 決定部署、Cookie、徽章 URL 長度 |

完整清單見 [`docs/01-open-questions.md`](docs/01-open-questions.md)。

---

## 本週（11/10–11/16）七件事

- [ ] 回覆 Q-1（網域）、Q-5（登入）、Q-6（審核）
- [ ] 建 repo、CI、部署可連 DB 的空站
- [ ] 從主站 `src/data/sessions.ts` 匯出 `agenda.json`，補每場 1–3 個標籤
- [ ] 徽章需求單送設計組，壓 11/18 定稿、11/20 下單
- [ ] 地圖 v0（Figma／紙上）：三區域 → 標籤 → 議程，驗證可讀性
- [ ] 定義 KKTIX 名單匯出欄位與報到代碼規則
- [ ] 建立活動日事故聯絡表與決策權限

---

## 技術基線（建議）

Astro 7 SSR + 原生 CSS/TS islands，部署 Cloudflare Workers + D1 + KV，AI 審用 Workers AI（備援 Gemini）。與主站 `qracon_web_2026` 共用設計 token 與 `agenda.json`，不共用程式碼、不動主站。

理由與替代方案見 [`docs/03-architecture-and-data-model.md`](docs/03-architecture-and-data-model.md) §1。
