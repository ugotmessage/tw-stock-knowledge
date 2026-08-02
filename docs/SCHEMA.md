# 股票資料格式

每檔股票使用一個 YAML 檔，路徑為 `stocks/<code>.yaml`。

## 必填欄位

| 欄位 | 型別 | 說明 |
|---|---|---|
| `schema_version` | string | 目前固定為 `"1.0"` |
| `code` | string | 四位數股票代號，保留前導零 |
| `name` | string | 公司簡稱 |
| `market` | string | `TWSE`、`TPEx` 或 `ESB` |
| `summary` | string | 一至三句公司定位 |
| `tags` | object | 分類後的標籤 |
| `updated_at` | string | `YYYY-MM-DD` |

## 建議欄位

```yaml
schema_version: "1.0"
code: "2408"
name: 南亞科
market: TWSE
industry_status: listed
summary: 台灣主要 DRAM 製造商。

company:
  website: https://example.com
  headquarters: 台灣

tags:
  industry: []
  product: []
  technology: []
  application: []
  theme: []
  supply_chain: []
  characteristic: []
  risk: []

theme_relations:
  - theme: 記憶體漲價
    relation_type: direct
    reason: DRAM 報價變動會直接影響產品售價與毛利。
    confidence: high
    source_ids: [src-001]

related_stocks:
  - code: "2344"
    name: 華邦電
    relation: 同屬記憶體族群，但產品組合不同。

sources:
  - id: src-001
    title: 公司年報或法說資料
    url: https://example.com
    publisher: 公司名稱
    published_at: "2026-01-01"
    accessed_at: "2026-08-02"
    source_type: company

notes: []
updated_at: "2026-08-02"
```

## 標籤分類

- `industry`：半導體、PCB、被動元件等產業分類。
- `product`：DRAM、MLCC、ABF 載板等實際產品。
- `technology`：CoWoS、先進封裝、液冷等技術。
- `application`：AI伺服器、車用、手機、工控等終端應用。
- `theme`：漲價、缺貨、轉單、國產替代等市場題材。
- `supply_chain`：上游材料、中游製造、下游組裝等角色。
- `characteristic`：景氣循環、高資本支出、高營運槓桿等特性。
- `risk`：價格下跌、客戶集中、匯率、良率等風險。

## 欄位規則

- 陣列不可放入空字串或重複值。
- 標籤使用繁體中文；通用技術縮寫可保留英文。
- `summary` 應描述穩定的公司定位，不放短期股價評論。
- 時間敏感的題材放入 `theme_relations`，並附來源及日期。
- `confidence` 僅使用 `high`、`medium`、`low`。
- `source_type` 建議使用 `company`、`exchange`、`government`、`industry`、`media`、`broker`、`community`。
- URL 應連到原始資料頁，而不是搜尋結果頁。

## 資料缺漏

未知欄位應省略或使用空陣列，不要猜測。不可使用 `N/A` 代替未知內容。