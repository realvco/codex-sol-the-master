<h3 align="center">
  <a href="./README.md">繁體中文</a>
  &nbsp;&nbsp;&nbsp;·&nbsp;&nbsp;&nbsp;
  <a href="./README.en.md">English</a>
</h3>

<h1 align="center">Codex Sol 委派設定</h1>

本專案整理一套「明確指令、單次委派」的兩層 Codex Sol 設定。

| 精確指令 | 自訂 Agent | 模型 | 推理強度 |
| --- | --- | --- | --- |
| `有請高手處理` | `sol_high` | `gpt-5.6-sol` | `high` |
| `恭請高高手處理` | `sol_xhigh` | `gpt-5.6-sol` | `xhigh` |

指令只作用於目前這一項任務，不會永久改變主 Agent 的模型、推理強度或速度
設定。辨識必須採用逐字精確比對；不得使用模糊比對、子字串比對、標點正規化
或語意相似度。兩個指令不可互相誤觸發。

## 專案內容

- `SOL_DELEGATION_SETUP_REQUEST.md`：保留原始設定需求；它是歷史快照。
- `docs/CONFIGURATION.md`：檔案配置與安全安裝說明。
- `docs/TESTING.md`：正向、負向及回到預設設定的測試方式。
- `examples/`：去除個人路徑的範例；不是目前電腦上的啟用設定。
- `AGENTS.md`、`CONTENT-SPEC.md`、`CHECKLIST.md`：專案規則、範圍及發布檢查。

這是一個文件與設定範例專案，不是 Codex plugin，也不會自動修改使用者的
Codex 安裝。

## 快速測試

先閱讀 [docs/TESTING.md](docs/TESTING.md)，再開一個新的 Codex task，使用
精確指令搭配無害的唯讀工作，例如：

```text
有請高手處理。請讀取 README.md，列出本專案的三個主要檔案，不要修改任何檔案。
```

測試時要確認實際子 Agent 名稱、模型與推理強度。若客戶端沒有提供 service
tier 遙測，就明確標示「未觀測到」，不要推測或宣稱已驗證。

## 安全

不要提交 API key、token、私有 log、個人絕對路徑或個人 Codex home 內容。
詳見 [SECURITY.md](SECURITY.md)。
