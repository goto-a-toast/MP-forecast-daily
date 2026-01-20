# MP（限界利益）予測モデル比較実験

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/goto-a-toast/MP-forecast-daily/blob/main/mp_prediction_model_comparison.ipynb)

複数の機械学習モデルを使用して、MP（限界利益）の日次予測を行い、最も精度の高いモデルを特定するためのJupyter Notebookです。

## 📊 比較するモデル

1. **線形回帰** (Linear Regression)
2. **Ridge回帰** (L2正則化)
3. **Lasso回帰** (L1正則化)
4. **ランダムフォレスト** (Random Forest)
5. **勾配ブースティング** (Gradient Boosting)
6. **XGBoost**
7. **LightGBM**
8. **Prophet**（時系列特化）

## 🚀 使い方

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

## 📝 バグ修正履歴

- **2026-01-20**: ファイルアップロード時のIndexError対策を追加（cell-6）
  - ファイルが選択されなかった場合のエラーハンドリングを実装
  - ユーザーフレンドリーなエラーメッセージを表示

## 📄 ライセンス

このプロジェクトはオープンソースです。自由に使用・改変してください。

## 🤝 貢献

バグ報告や機能追加の提案は、GitHubのIssuesでお願いします。

## 📧 お問い合わせ

質問や提案がある場合は、GitHubのIssuesまたはPull Requestsでお知らせください。
