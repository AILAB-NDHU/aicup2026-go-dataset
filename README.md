# 2026 AI CUP 圍棋競賽 — 訓練資料集

100 萬盤匿名圍棋棋譜，依 10 級棋力分檔。

## 下載

資料檔請由 [Releases](../../releases) 取得。

```bash
tar xf aicup2026_go_training.tar.xz
```

```
aicup2026_go_training.tar.xz   214 MB   解壓後約 1.3 GB
SHA-256  bcc9c1354137da1ebe9edbd47331d651998914cdbe91798af08b87976be1b69a
```

## 內容

```
training/
├── train_D.csv    train_C.csv    train_B.csv    train_A.csv
├── train_1D.csv   train_2D.csv   train_3D.csv
└── train_4D.csv   train_5D.csv   train_6D.csv
```

10 個等級各一檔，每檔 100,000 列，共 1,000,000 盤。等級由弱到強為
`D`、`C`、`B`、`A`、`1D` 至 `6D`。

## 欄位

`player_id, game_id, rank, color, sgf_content`

| 欄位 | 說明 |
| --- | --- |
| `player_id` | 玩家代號，同一玩家在所有列一致 |
| `game_id` | 棋譜編號 |
| `rank` | 該玩家的棋力等級，等於檔名的等級 |
| `color` | 該玩家在這盤的執色，`B` 或 `W` |
| `sgf_content` | 單行 SGF，只含棋盤大小與落子 |

樣本見 [`sample_train_A.csv`](sample_train_A.csv)。

`sgf_content` 內含逗號，請以標準 CSV 解析器讀取：

```python
import pandas as pd
df = pd.read_csv("training/train_A.csv")
```

## 使用範圍

本資料集僅供 2026 AI CUP 競賽使用。棋譜已匿名化，不得用於識別真實使用者。
