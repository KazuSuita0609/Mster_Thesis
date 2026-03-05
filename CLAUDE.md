# 修士論文プロジェクト — Claude への指示

## 研究背景（卒業論文から継承）

卒業論文では，d4PDF 5kmグリッド地域気候モデルデータを用いて以下を行った：

- **対象：** 淀川流域（緯度：34.3–35.6°N，経度：135.3–136.4°E）
- **手法：** 降雨イベント抽出（枚方地点24h降水量 ≥ 20mm）＋クラスタリング（K-means, SOM, Autoencoder, DEC, CAE）
- **比較：** 現在気候（1950–2010年，12アンサンブルメンバー）と +4K 将来気候シナリオ
- **統計：** カイ二乗検定・二項検定によるパターン出現頻度の変化検証
- **主要な知見：** 将来気候下で一部の降水パターン（南部集中型）の出現頻度が増加する傾向が示唆された

過去の研究コードは `../ProgramCode_for_graduation_thesis/` に保存されている。

---

## 修士論文の研究テーマ

<!-- TODO: 研究テーマ・目的が決まったらここを更新する -->

**テーマ：** （未定）

**研究目的：**
- （未定）

**先行研究との差分：**
- （未定）

---

## 技術スタック

### 言語・環境
- Python 3.x（主要言語）
- Jupyter Notebook（探索的解析）

### 主要ライブラリ
```
xarray        # NetCDFデータの読み書き・操作
numpy, pandas # 数値計算・データ整形
scikit-learn  # K-means, PCA, t-SNE等
minisom       # Self-Organizing Map
tensorflow / keras  # Autoencoder, DEC
matplotlib, cartopy # 可視化・地図描画
netCDF4       # NetCDFの低レベルアクセス
```

### データソース
- d4PDF 5km グリッドデータ（DS2022）
  - NetCDF形式（.nc），時間解像度：1時間
  - 詳細：https://diasjp.net/ds2022/manual_chapter1.html#main1

---

## コード規約

- **再利用優先：** 卒論コード（`../ProgramCode_for_graduation_thesis/`）の関数・パイプラインを積極的に活用する
- **データパス：** ハードコードは避け，`config.py` や環境変数（`DATA_DIR` 等）で管理する
- **ファイル命名：** `snake_case` を使用する
- **NetCDF操作：** `xarray.open_dataset()` を基本とし，メモリ効率のため `chunks` パラメータを活用する
- **可視化：** 地図表示は `cartopy` を使用し，流域界は別途シェープファイルで重畳する

---

## 推奨プロジェクト構造

```
Mster_Thesis/
├── CLAUDE.md          # このファイル（Claude への指示）
├── README.md          # プロジェクト概要（研究方向確定後に記載）
├── config.py          # データパス・パラメータ設定（作成予定）
├── data/              # 前処理済みデータ（GitHubにはpush不要）
├── src/               # 解析スクリプト
│   ├── preprocessing/ # データ前処理
│   ├── analysis/      # メイン解析
│   └── visualization/ # 可視化
├── notebooks/         # Jupyter探索用
└── results/           # 出力図・CSV
```

---

## 重要な注意事項

- d4PDF データは大容量（GB単位）のため，処理時は `xarray` の lazy loading を使用すること
- 現在気候と将来気候のイベント数はそれぞれ約 **7,200件**（12メンバー × 61年）を基準とする
- クラスター番号はクラスタリングの際に便宜的につけられたものであり，番号とパターンに対応関係はない（可視化時に注意）
- 統計検定を行う際は卒論と同じ検定手法（カイ二乗検定・二項検定）との整合性を確認すること
