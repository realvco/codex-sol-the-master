# 變更紀錄

<p align="center"><a href="./CHANGELOG.zh-TW.md" style="font-size: 20px; text-decoration: none;">繁體中文</a> · <a href="./CHANGELOG.md" style="display: inline-block; font-size: 24px; text-decoration: none; border: 1px solid #0969da; border-radius: 6px; padding: 4px 10px; color: #0969da;">English</a></p>

## 目前公開契約

繁中頁面目前列出兩個精確暗號：

- `有請高手處理` → `sol_high`
- `恭請高高手處理` → `sol_xhigh`

這兩個暗號都只授權單一完整任務，不會永久切換主代理。任務完成、失敗、受阻或
停止後，控制權回到原本主代理；下一項任務必須再次輸入完整暗號。

## 文件狀態

- `SOL_DELEGATION_SETUP_REQUEST.md` 是最初需求的原始快照，保持不變以供稽核。
- `README.md` 是繁體中文主入口；`README.en.md` 是英文主入口。
- `docs/CONFIGURATION.zh-TW.md` 與 `docs/CONFIGURATION.md` 是雙語設定說明。
- `docs/TESTING.zh-TW.md` 與 `docs/TESTING.md` 是雙語測試說明。

## 速度與驗證原則

`high` 與 `xhigh` 代表推理深度，不代表 Fast。若客戶端沒有顯示 service-tier
證據，文件與測試回報必須標示未觀測，不得把推定寫成已驗證。
