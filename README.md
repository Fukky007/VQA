# Visual Question Answering（VQA）


## 環境構築
### Conda
```bash
conda create -n dl_competition python=3.10
pip install -r requirements.txt
```
### Docker
- Dockerイメージのcudaバージョンについては，ご自身が利用するGPUに合わせて変更してください．
```bash
docker build -t <イメージ名> .
docker run -it -v $PWD:/workspace -w /workspace <イメージ名> bash
```

## ベースラインモデルを動かす
### データのダウンロード
- [こちら](https://drive.google.com/drive/folders/1QTcWMATZ_iGsHnxq6-3aXa7D5VZAzs5T?usp=sharing)から各データをダウンロードしてください．
  - train.json: 訓練データのjsonファイル．画像のパス，質問文，回答文がjson形式でまとめられている．
  - valid.json: テストデータのjsonファイル．画像のパス，質問文がjson形式でまとめられている．
  - train.zip: 訓練データの画像ファイル．
  - valid.zip: テストデータの画像ファイル．
- ダウンロード後，train.zipとvalid.zipを解凍し，各データをdataディレクトリ下に置いてください．
### 訓練・提出ファイル作成
```bash
python3 main.py
```
- `main.py`と同様のディレクトリ内に，学習したモデルのパラメータ`model.pth`とテストデータに対する予測結果`submission.npy`ファイルが作成されます．

## VizWiz(2023 edition) dataset [[link](https://www.kaggle.com/datasets/nqa112/vizwiz-2023-edition)] の詳細
- 24842枚の画像があり，訓練データには各画像に対して1つの質問と10人の回答者による回答．
  - 10人の回答はすべて同じであるとは限らない．
- 24842のデータの内，80%(19873)が訓練データ，20%(4969)がテストデータとして与えられる．
  - テストデータに対する回答は正解ラベルとし，訓練時には与えられない．
  - データ提供元ではtrainとvalに分かれているが，データの配布前に運営の方でtrainとvalをランダムに分け直している．

## タスクの詳細
- 評価は[VQA](https://visualqa.org/index.html)(Visual Question Answering)に基づき，以下の式で計算される．
$$\text{Acc}(ans) = min(\frac{humans \ that \ said \ ans}{3}, 1)$$
- 1つのデータに対し，10人の回答の内9人の回答を選択し上記の式で性能を評価した，10パターンのAccの平均をそのデータに対するAccとする．
- 予測結果と正解ラベルを比較する前に，回答をlowercaseにする，冠詞は削除するなどの前処理を行う（[詳細](https://visualqa.org/evaluation.html)）．
