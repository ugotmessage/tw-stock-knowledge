# 取用指南

## 給 AI Agent

處理本 repository 的推薦流程：

1. 讀取根目錄 `AGENTS.md`。
2. 讀取 `docs/SCHEMA.md` 與 `docs/TAGS.md`。
3. 根據股票代號讀取 `stocks/<code>.yaml`。
4. 查概念股時，遍歷 `stocks/*.yaml` 的標籤與 `theme_relations`。
5. 回答時說明關聯原因、強度、來源與更新日期。

### 查單一股票

使用股票代號定位，不要只靠名稱搜尋：

```text
stocks/2408.yaml
```

### 查某個概念

依優先順序比對：

1. `theme_relations[].theme`
2. `tags.application`
3. `tags.product`
4. `tags.technology`
5. `tags.theme`
6. `tags.industry`

`theme_relations` 有原因與關聯強度，應優先用於解釋。

### Agent 回答範本

```text
2408 南亞科
- 符合標籤：DRAM、記憶體漲價
- 關聯類型：direct
- 原因：公司直接生產 DRAM，產品報價變動會影響營運。
- 信心：high
- 資料更新：2026-08-02
- 來源：公司法說資料
```

## 給 Python 程式

安裝 PyYAML：

```bash
pip install pyyaml
```

讀取單檔：

```python
from pathlib import Path
import yaml

path = Path("stocks/2408.yaml")
stock = yaml.safe_load(path.read_text(encoding="utf-8"))
print(stock["code"], stock["name"])
```

依標籤查詢：

```python
from pathlib import Path
import yaml


def load_stocks():
    for path in Path("stocks").glob("*.yaml"):
        yield yaml.safe_load(path.read_text(encoding="utf-8"))


def find_by_tag(category: str, tag: str):
    return [
        stock
        for stock in load_stocks()
        if tag in stock.get("tags", {}).get(category, [])
    ]

for stock in find_by_tag("application", "AI伺服器"):
    print(stock["code"], stock["name"])
```

依題材關聯查詢：

```python
def find_by_theme(theme: str, relation_type: str | None = None):
    results = []
    for stock in load_stocks():
        for relation in stock.get("theme_relations", []):
            if relation.get("theme") != theme:
                continue
            if relation_type and relation.get("relation_type") != relation_type:
                continue
            results.append({"stock": stock, "relation": relation})
    return results
```

## 給其他系統

- YAML 是主要維護格式。
- 匯入資料庫時，以 `code` 作為股票主鍵之一，並搭配市場欄位。
- 不要把標籤陣列合併成單一逗號字串後再覆寫原始檔。
- 建立向量索引時，可使用 `summary`、標籤與 `theme_relations.reason`，但查詢結果仍應回到原始 YAML 驗證。

## 時效性

本庫不是即時資料源。對「目前、最新、今年」等問題，Agent 應先檢查 `updated_at` 與來源日期；資料過舊時要明確告知並重新查證。