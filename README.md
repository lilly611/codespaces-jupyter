# Kaggriculture Agent

Kaggle の農業シミュレーション環境 **Kaggriculture** で動かす、メロン栽培エージェントの実験リポジトリです。観測データから農場の状態と市場価格を読み取り、植付け・水やり・収穫・種の購入・メロンの売却を自動で判断します。

## 構成

```text
.
├── notebooks/
│   ├── kaggriculture-getting-started.ipynb  # ゲームルールとスターター実装
│   ├── agent.ipynb                          # 改良版エージェントの対戦比較
│   └── melon_maxxer.py                      # エージェント本体
├── data/                                    # データ置き場
├── requirements.txt
└── .devcontainer/devcontainer.json
```

## セットアップ

Python 3.11 を想定しています。Codespaces では、コンテナ作成時に依存関係がインストールされます。ローカル環境では次を実行してください。

```bash
python -m pip install -r requirements.txt
python -m pip install "kaggle-environments>=1.32.2"
```

VS Code で Jupyter 拡張機能を有効にし、Python カーネルとしてセットアップした環境を選択します。

## 使い方

1. `notebooks/kaggriculture-getting-started.ipynb` を開き、ゲームの観測形式とアクションを確認します。
2. ノートブックを上から実行して、`melon_maxxer` をランダムエージェントと対戦させます。
3. `notebooks/melon_maxxer.py` を編集し、戦略を改善します。
4. `notebooks/agent.ipynb` を実行して、スターター版と改良版のスコアを比較します。

コマンドラインからエージェントを読み込んで対戦する場合は、リポジトリのルートで次のように実行できます。

```bash
python - <<'PY'
from kaggle_environments import make
from notebooks.melon_maxxer import melon_maxxer

env = make("kaggriculture", debug=True)
env.run([melon_maxxer, "random"])
print([state.reward for state in env.steps[-1]])
PY
```

## 現在の戦略

- メロンの種を切らしたとき、購入資金があれば 1 個購入する
- 空いているタイルへ移動してメロンを植える
- メロンを毎日水やりし、収穫可能になったら収穫する
- 市場価格が `SELL_THRESHOLD` 以上のときだけ、倉庫のメロンを売る
- 改良版では、雇った farm hand に水やりと収穫を分担させる

スターター実装はメロンだけを扱い、土地の拡張・肥料・複数作物への切り替えには対応していません。`agent.ipynb` では、これらを改善するための比較実験を行えます。

## 参考

- [Kaggriculture](https://www.kaggle.com/competitions/kaggriculture-gdm-internal)
- [Kaggle Environments](https://github.com/Kaggle/kaggle-environments)
