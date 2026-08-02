# 貢獻指南

## 新增股票

1. 複製現有 YAML 範例。
2. 以四位數股票代號命名，例如 `stocks/2330.yaml`。
3. 確認 `code` 與檔名一致。
4. 依 `docs/SCHEMA.md` 填寫欄位。
5. 使用 `docs/TAGS.md` 的標準名稱。
6. 對重要敘述加入來源。
7. 更新 `updated_at`。

## 修改股票

- 保留既有、仍然正確的資料。
- 時間敏感資訊若已失效，應刪除或改寫，不要只追加新說法。
- 修改標籤時，檢查是否影響其他股票的一致性。
- 若改動欄位結構，必須同步更新：
  - `schemas/stock.schema.json`
  - `docs/SCHEMA.md`
  - `AGENTS.md`（若 Agent 行為受影響）

## 品質檢查

提交前確認：

- YAML 語法正確。
- 股票代號使用字串。
- 日期格式為 `YYYY-MM-DD`。
- 無重複標籤。
- 標籤沒有放錯分類。
- `theme_relations` 有清楚原因。
- 客戶、訂單與受惠關係有可信來源。
- 不包含買賣建議、目標價或未驗證傳聞。

## Commit 建議

```text
Add 2408 Nanya Technology profile
Update AI server tags for 2382
Normalize memory industry tags
```

## Pull Request 說明

PR 應說明：

- 新增或修改哪些股票
- 新增哪些標籤或關聯
- 主要資料來源
- 是否有低信心或待確認內容
