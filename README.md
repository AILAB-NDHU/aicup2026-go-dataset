# 2026 AI CUP 圍棋競賽 — 訓練資料集

100 萬盤匿名圍棋棋譜，依 10 級棋力分檔，執黑／執白各半。

資料檔案體積過大，不直接放在 repo 內，改由 [Releases](../../releases) 發佈。

## 下載

| 檔案 | 大小 | 內容 |
| --- | ---: | --- |
| `aicup2026_go_training.tar.xz` | 214 MB | 10 個等級的訓練檔（解壓後約 1.3 GB） |

```bash
tar xf aicup2026_go_training.tar.xz     # 解出 training/
```

SHA-256

```
8d6e5e2cfccf41513671d2c72f62808cbdc74806cd870f16191b9d001d47915c
```

## 內容

```
training/
├── train_D.csv   train_C.csv   train_B.csv   train_A.csv      級位（弱 → 強）
├── train_1D.csv  train_2D.csv  train_3D.csv                   段位
├── train_4D.csv  train_5D.csv  train_6D.csv
└── README.md
```

每檔 100,000 列，共 1,000,000 盤。

## 欄位

`player_id, game_id, rank, color, sgf_content`

| 欄位 | 說明 |
| --- | --- |
| `player_id` | 玩家的匿名代號，同一玩家在所有列一致，無法回推真實身分 |
| `game_id` | 該檔內的棋譜流水號 |
| `rank` | 該玩家的棋力等級（大寫）；同一玩家所有列相同，且等於檔名的等級 |
| `color` | 該玩家在這盤的實際執色，`B` 或 `W` |
| `sgf_content` | 匿名化並單行化的 SGF；只保留棋盤大小與落子 |

樣本見 [`sample_train_A.csv`](sample_train_A.csv)（前 20 列）。

`sgf_content` 內含逗號，請以標準 CSV 解析器讀取，不要用 `split(",")`：

```python
import pandas as pd
df = pd.read_csv("training/train_A.csv")
```

## 10 級制

由弱到強：

| 等級 | 對應 | | 等級 | 對應 |
| --- | --- | --- | --- | --- |
| `D` | 10k–12k | | `1D` – `6D` | 1 段 – 6 段 |
| `C` | 7k–9k | | | |
| `B` | 4k–6k | | | |
| `A` | 1k–3k | | | |

級位跨到段位不是斷點：`A` 與 `1D` 相鄰。

`rank` 是該玩家**所有對局的眾數等級**，不是單局等級；同一玩家的每一列都相同。

## 棋譜格式

每盤只保留重建棋局所需的內容，所有可識別資訊皆已移除：

```
(;SZ[19];B[pd];W[pq];B[cd];W[cp];…)
```

保留的屬性僅 `SZ`（棋盤大小）與 `B` / `W` / `AB` / `AW` / `AE`（落子與盤面設定）。
棋手姓名、段位、勝負、日期、註解等一律不含。

每盤棋譜都經過合法性驗證：19 路、主分支可完整重播（不含自殺、打劫、疊子）、
手數至少 100，且已排除機器人與系統帳號。

## 授權與使用

本資料集僅供 2026 AI CUP 競賽使用。棋譜已匿名化，不得用於回推或識別真實使用者。
