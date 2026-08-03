<table align="center">
  <tr>
    <td align="center"><a href="./README.md" style="font-size: 20px; text-decoration: none;">繁體中文</a></td>
    <td align="center"><a href="./README.en.md" style="display: inline-block; font-size: 24px; text-decoration: none; border: 1px solid #0969da; border-radius: 6px; padding: 4px 10px; color: #0969da;">English</a></td>
  </tr>
</table>

<h1 align="center">Codex Sol 委派設定</h1>

本專案整理一套「明確指令、單次委派」的兩層 Codex Sol 設定。

| 精確指令 | 自訂 Agent | 模型 | 推理強度 |
| --- | --- | --- | --- |
| `有請高手處理` | `sol_high` | `gpt-5.6-sol` | `high` |
| `恭請高高手處理` | `sol_xhigh` | `gpt-5.6-sol` | `xhigh` |

指令只作用於目前這一項任務，不會永久改變主 Agent 的模型、推理強度或速度
設定。辨識必須採用逐字精確比對；不得使用模糊比對、子字串比對、標點正規化
或語意相似度。兩個中文指令不可互相誤觸發。

## 核心設計

這套設定把日常使用的 Codex 主代理，與明確召請的 Sol 子代理分開。主代理的
模型、推理強度與速度仍由 Codex App 當前設定決定；Sol 暗號只授權目前這一項
任務，不會永久切換整個對話。

### 兩個 Sol 層級

- `sol_high`：使用 `gpt-5.6-sol` 與 `high` 推理強度，適合根因分析、定向修復、
  測試補強與一般跨檔案實作。
- `sol_xhigh`：使用 `gpt-5.6-sol` 與 `xhigh` 推理強度，適合架構取捨、資料遷移、
  高風險變更、複雜依賴與需要額外驗證的整體任務。

`high` 與 `xhigh` 是推理深度，不等於服務速度。兩個 Sol Agent 都不得主動要求
Fast；如果客戶端沒有提供可觀測的 service-tier 證據，文件與回報必須標示
「未觀測到」，不能把推定寫成已驗證。

### 一次委派的生命週期

1. 使用者以完整暗號作為主動指令，並在同一則訊息提供任務，或先有明確未完成
   任務、再單獨輸入暗號。
2. 主代理建立指定的實際自訂 Agent，傳遞任務、限制、目前進度與驗收條件。
3. 子代理完整執行並回報工作、檔案、檢查結果、模型／推理強度與可觀測限制。
4. 子代理完成、失敗、受阻、被停止或 Thread 關閉後，這次授權立即結束，控制權
   回到原本主代理。
5. 下一項任務必須再次輸入完整暗號；困難度、檔案數量或主代理的不確定性都不會
   自動啟動 Sol。

### 精確匹配與優先級

兩個中文暗號都採完整字串、逐字精確匹配。引用、文件內容、翻譯要求、否定句、
只輸入部分片語，以及「請深入分析」等一般請求都不會觸發。

如果同一項任務同時指定兩個層級，預設只使用優先級較高的 `sol_xhigh`，避免兩個
Agent 重複做同一件事。只有在使用者清楚劃分兩個互不重疊的子任務時，才可分別
委派；邊界不清時仍以 `sol_xhigh` 處理完整任務。

### 適用範圍與安全邊界

這個專案是文件與設定範例，不是安裝器，也不會自動修改 Codex。正式安裝前應
確認 Codex 版本與 Agent TOML schema、備份 `$CODEX_HOME`、檢查既有 Agent 與
路由區塊，並只套用明確授權的檔案。不得把 API key、Token、Cookie、私有 log、
個人絕對路徑或整份私人 Codex Home 提交到公開 repository。

## 專案內容

- `SOL_DELEGATION_SETUP_REQUEST.md`：保留原始設定需求；它是歷史快照。
- [`docs/CONFIGURATION.zh-TW.md`](docs/CONFIGURATION.zh-TW.md)：繁中檔案配置與安全安裝說明。
- [`docs/CONFIGURATION.md`](docs/CONFIGURATION.md)：英文版設定與安全安裝說明。
- [`docs/TESTING.zh-TW.md`](docs/TESTING.zh-TW.md)：繁中正向、負向及回到預設測試。
- [`docs/TESTING.md`](docs/TESTING.md)：英文版正向、負向及回到預設設定的測試。
- [`docs/CHANGELOG.zh-TW.md`](docs/CHANGELOG.zh-TW.md)／[`docs/CHANGELOG.md`](docs/CHANGELOG.md)：雙語變更紀錄。
- `examples/`：去除個人路徑的範例；不是目前電腦上的啟用設定。
- `AGENTS.md`、`CONTENT-SPEC.md`、`CHECKLIST.md`：專案規則、範圍及發布檢查。

這是一個文件與設定範例專案，不是 Codex plugin，也不會自動修改使用者的
Codex 安裝。

## 快速測試

1. 閱讀[繁中設定說明](docs/CONFIGURATION.zh-TW.md)或 [英文版設定說明](docs/CONFIGURATION.md)。
2. 修改前先備份個人 Codex Home。
3. 確認目前 Codex 版本與支援的 Agent schema 後，才安裝範例設定。
4. 閱讀[繁中測試說明](docs/TESTING.zh-TW.md)或 [英文版測試說明](docs/TESTING.md)，
   再開一個新的 Codex task，使用精確指令搭配無害的唯讀工作。

繁中指令範例：

```text
有請高手處理。請讀取 README.md，列出本專案的三個主要檔案，不要修改任何檔案。
```

子代理回報完成後，主代理應接收結果並恢復控制；下一項任務必須再次輸入完整暗號。
測試時要確認實際子 Agent 名稱、模型與推理強度。若客戶端沒有提供 service-tier
遙測，就明確標示「未觀測到」，不要推測或宣稱已驗證。

## 公開 repository 安全

不要提交 API key、token、私有 log、個人絕對路徑或個人 Codex home 內容。
詳見 [SECURITY.md](SECURITY.md)。

## 授權

本文件與設定範例採用 MIT 授權。詳見 [LICENSE](LICENSE)。
