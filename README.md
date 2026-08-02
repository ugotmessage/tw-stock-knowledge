# tw-stock-knowledge

台股概念、產業鏈、產品、應用與題材標籤的結構化知識庫。

本專案的主要目標不是提供買賣建議，而是讓人類、程式與 AI Agent 能以一致格式查詢：

- 一檔股票有哪些概念與標籤
- 該公司位於哪個產業鏈環節
- 題材屬於直接受惠、間接受惠或市場聯想
- 各標籤的依據、來源與更新時間
- 哪些股票共享相同產品、應用或事件題材

## 快速開始

股票主資料放在 `stocks/`，每檔股票一個 YAML 檔，檔名使用四位數股票代號：

```text
stocks/2330.yaml
stocks/2408.yaml
```

最小可用格式：

```yaml
schema_version: "1.0"
code: "2408"
name: 南亞科
market: TWSE
summary: 台灣主要 DRAM 製造商，營運與 DRAM 報價及產能利用率高度相關。
tags:
  industry: [半導體, 記憶體]
  product: [DRAM, DDR4, DDR5]
  application: [PC, 伺服器, 消費電子]
  theme: [記憶體漲價, 庫存回補]
  characteristic: [景氣循環, 高營運槓桿]
  risk: [DRAM 報價下跌, 資本支出壓力]
updated_at: "2026-08-02"
```

完整欄位與規則請見 [`docs/SCHEMA.md`](docs/SCHEMA.md)。

## 目錄結構

```text
tw-stock-knowledge/
├── README.md
├── AGENTS.md
├── CONTRIBUTING.md
├── docs/
│   ├── SCHEMA.md
│   ├── USAGE.md
│   └── TAGS.md
├── stocks/
│   └── 2408.yaml
└── schemas/
    └── stock.schema.json
```

## Agent 取用原則

Agent 進入此 repository 後，應先讀取：

1. `AGENTS.md`
2. `docs/SCHEMA.md`
3. `docs/TAGS.md`
4. 目標股票的 `stocks/<code>.yaml`

查詢時應優先使用結構化欄位，不要只依賴 `summary`。新增或更新資料時，必須保留股票代號為字串、附上來源、更新日期，並區分直接與間接受惠。

詳細流程請見 [`docs/USAGE.md`](docs/USAGE.md)。

## 設計原則

- **一股多標籤**：同一檔股票可以同時屬於多個產業、產品與應用。
- **證據優先**：題材應盡量附官方或可信來源，不把市場傳聞寫成已確認事實。
- **區分關聯強度**：直接供應、間接受惠、市場聯想不可混為一談。
- **可機器讀取**：股票資料以 YAML 為主，欄位名稱固定。
- **可持續更新**：時間敏感內容必須標記 `updated_at`。
- **不含投資建議**：本庫描述公司與題材關係，不預測股價。

## 使用範例

尋找所有具有 `AI伺服器` 應用標籤的股票：

```python
from pathlib import Path
import yaml

matches = []
for path in Path("stocks").glob("*.yaml"):
    stock = yaml.safe_load(path.read_text(encoding="utf-8"))
    if "AI伺服器" in stock.get("tags", {}).get("application", []):
        matches.append((stock["code"], stock["name"]))

print(matches)
```

## 狀態

目前處於初始建置階段。第一階段以建立股票標籤、公司定位、供應鏈關係與資料來源為主。