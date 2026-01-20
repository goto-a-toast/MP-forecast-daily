# 時系列予測モデル比較ツール / Time Series Forecasting Tool

このリポジトリには、機械学習を使った時系列予測のための2つのノートブックが含まれています：

## 📔 ノートブック一覧

### 1. 汎用版（推奨）：任意の時系列データに対応
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/goto-a-toast/MP-forecast-daily/blob/main/timeseries_prediction_generic.ipynb)

**ファイル名**: `timeseries_prediction_generic.ipynb`

**特徴**:
- ✅ 任意の列名に対応（設定で指定可能）
- ✅ どんな時系列データでも使える（売上、需要、PV数など）
- ✅ ダミー変数なしでも動作
- ✅ 英語インターフェース（グラフも英語表記）
- ✅ 初心者にも使いやすい設定セクション付き

**おすすめ用途**: 初めて使う方、様々な時系列データに適用したい方

---

### 2. MP専用版：限界利益（MP）予測に最適化
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/goto-a-toast/MP-forecast-daily/blob/main/mp_prediction_model_comparison.ipynb)

**ファイル名**: `mp_prediction_model_comparison.ipynb`

**特徴**:
- MP（限界利益）列が前提
- 特定のデータフォーマットに最適化
- 日本語の説明とコメント

**おすすめ用途**: MPデータ専用、既存のデータフォーマットに合わせる場合

---

## 📊 比較するモデル

1. **線形回帰** (Linear Regression)
2. **Ridge回帰** (L2正則化)
3. **Lasso回帰** (L1正則化)
4. **ランダムフォレスト** (Random Forest)
5. **勾配ブースティング** (Gradient Boosting)
6. **XGBoost**
7. **LightGBM**
8. **Prophet**（時系列特化）

## 🚀 汎用版の使い方（推奨）

### ステップ1: Colabで開く

1. 上の**汎用版**の「Open in Colab」バッジをクリック
2. Google Colabでノートブックが開きます

### ステップ2: 設定を編集

**重要**: セル2の設定セクションで、あなたのデータに合わせて以下を設定してください：

```python
# あなたのCSVファイルの列名を指定
DATE_COLUMN = 'date'           # 日付列の名前
TARGET_COLUMN = 'sales'        # 予測したい列の名前（例：sales, revenue, pvなど）

# 除外する列（特徴量として使わない列）
EXCLUDE_COLUMNS = ['id', 'notes']  # 不要な列を指定

# テスト期間
TEST_DAYS = 180                # テスト用に使う日数（デフォルト：180日）

# 表示設定
TARGET_DISPLAY_NAME = 'Sales'  # グラフでの表示名
TARGET_SCALE = 1000            # 値を割る（1000なら千単位、1000000なら百万単位）
TARGET_UNIT = 'Thousand'       # 単位の表示名
```

### ステップ3: データをアップロード

3. セル3を実行してCSVファイルをアップロード
4. メニューから「ランタイム」→「すべてのセルを実行」を選択

これだけで、自動的にモデルの学習・評価・予測が実行されます！

---

## 🚀 MP専用版の使い方

### Google Colabで実行する場合（推奨）

1. 上の「Open in Colab」バッジをクリック
2. ノートブックが開いたら、メニューから「ランタイム」→「すべてのセルを実行」を選択
3. データファイルのアップロードプロンプトが表示されたら、CSVファイルをアップロード
4. 自動的にモデルの学習・評価・予測が実行されます

### ローカル環境で実行する場合

```bash
# リポジトリのクローン
git clone https://github.com/goto-a-toast/MP-forecast-daily.git
cd MP-forecast-daily

# 必要なライブラリのインストール
pip install pandas numpy matplotlib scikit-learn xgboost lightgbm prophet jupyter

# Jupyter Notebookの起動
jupyter notebook mp_prediction_model_comparison.ipynb
```

## 📁 データフォーマット

### 汎用版の場合

最小限必要なのは以下の2列だけです：

| 列名 | 説明 | 必須 |
|------|------|------|
| 日付列 | 日付（YYYY-MM-DD形式、列名は設定で指定） | ✅ |
| 目的変数列 | 予測したい値（列名は設定で指定） | ✅（学習データのみ） |
| その他の列 | 追加の特徴量（あれば自動的に使用） | オプション |

**データ例**:
```csv
date,sales,region,is_holiday
2023-01-01,100000,Tokyo,1
2023-01-02,120000,Tokyo,0
2023-01-03,115000,Tokyo,0
```

時系列特徴量（月、曜日、季節性など）は**自動生成**されるため、ダミー変数がなくても動作します。

---

### MP専用版の場合

入力CSVファイルは以下の形式である必要があります：

| 列名 | 説明 | 必須 |
|------|------|------|
| `date` | 日付（YYYY-MM-DD形式） | ✅ |
| `day` | 曜日 | ✅ |
| `MP` | 限界利益（数値、カンマ区切り可） | ✅（学習データのみ） |
| その他のダミー変数 | 年、月、曜日、祝日フラグなど | 推奨 |

### データ例

```csv
date,day,MP,year_2023,year_2024,month_1,month_2,holiday flg
2023-01-01,日,"1,234,567",1,0,1,0,1
2023-01-02,月,"2,345,678",1,0,1,0,0
2023-01-03,火,"3,456,789",1,0,1,0,0
```

**注意:**
- MPが空欄の行は予測対象として扱われます
- MPに値がある行は学習データとして使用されます

## 📈 出力結果

ノートブックを実行すると、以下の結果が得られます：

1. **モデル比較表**: 各モデルのMAE、RMSE、MAPE、R²スコア
2. **精度比較グラフ**: MAPEとR²スコアの可視化
3. **予測vs実績グラフ**: テスト期間における予測精度の確認
4. **特徴量重要度**: 予測に影響する要因の分析
5. **将来予測結果**: 2025-2026年のMP予測値（CSV出力）

## 🔧 必要なライブラリ

- pandas
- numpy
- matplotlib
- scikit-learn
- xgboost
- lightgbm
- prophet

**Google Colabの場合**: これらのライブラリは自動的にインストールされます。

## 📊 評価指標

- **MAE** (Mean Absolute Error): 平均絶対誤差
- **RMSE** (Root Mean Squared Error): 二乗平均平方根誤差
- **MAPE** (Mean Absolute Percentage Error): 平均絶対パーセント誤差
- **R²** (R-squared): 決定係数

**MAPE**が小さいほど、**R²**が1に近いほど精度が高いモデルです。

## 🛠️ カスタマイズ

### テスト期間の変更

```python
# cell-13で変更可能
test_days = 180  # デフォルトは180日（約6ヶ月）
```

### モデルのハイパーパラメータ調整

```python
# cell-16でモデルのパラメータを調整可能
'7. LightGBM': lgb.LGBMRegressor(
    n_estimators=100,    # ツリーの数
    max_depth=5,         # 木の深さ
    learning_rate=0.1,   # 学習率
    random_state=42
)
```

## 🎯 適用可能なユースケース

汎用版は以下のような時系列データに適用できます：

### ビジネス
- 📊 売上予測（日次/週次/月次）
- 📦 需要予測・在庫管理
- 💰 収益予測
- 🛒 ECサイトの注文数予測

### Webサービス
- 👥 ユーザー数・PV数予測
- 🎯 コンバージョン予測
- 📱 アプリのDAU/MAU予測

### エネルギー・インフラ
- ⚡ 電力需要予測
- 🚗 交通量予測
- 🏭 設備稼働率予測

### その他
- 🌡️ 気象データ（気温、降水量など）
- 📈 金融データ（株価、為替など）
- 🔧 IoTセンサーデータ

**条件**: 日次の数値データで、過去のパターンから将来を予測できる時系列データ

---

## 📝 バグ修正履歴

- **2026-01-20**: グラフの文字化け問題を修正
  - 日本語フォント設定を削除し、英語表記に変更
  - すべての環境で正しく表示されるように改善
- **2026-01-20**: ファイルアップロード時のIndexError対策を追加（cell-6）
  - ファイルが選択されなかった場合のエラーハンドリングを実装
  - ユーザーフレンドリーなエラーメッセージを表示
- **2026-01-20**: 汎用版ノートブックを追加
  - 任意の時系列データに対応できる汎用版を作成
  - 設定で列名やパラメータを簡単にカスタマイズ可能

## 📄 ライセンス

このプロジェクトはオープンソースです。自由に使用・改変してください。

## 🤝 貢献

バグ報告や機能追加の提案は、GitHubのIssuesでお願いします。

## 📧 お問い合わせ

質問や提案がある場合は、GitHubのIssuesまたはPull Requestsでお知らせください。
