## モデルを作るまでのまとめ


---
### ①必要なもののインストール

このページ以降、下の欄のコードをコピペして実行する（Shift + Enter）
```
!pip install gensim
!apt install aptitude
!aptitude install mecab libmecab-dev mecab-ipadic-utf8 git make curl xz-utils file -y
!pip install mecab-python3
```

---
### ②テキストファイルのダウンロードとアップロード

[ここ](https://drive.google.com/drive/folders/1ckgg3mUbs7WkBbEbm53GIgClRGjEdspH?usp=sharing)から『nekodearu.txt』を（大学のアカウントでログインして）ダウンロードしてから下のコードを実行してアップしてください！！！

```
from google.colab import files
uploaded = files.upload()
```

---
### ③ファイルを読み込んで配列に格納

配列に1文ずつ格納していきます

```
doc = []

#""の中身はさっきのファイル名
file = open("nekodearu.txt")

for line in file:
    doc.append(line)

file.close()
```

---
### ④形態素解析をする関数を定義する

今回は名詞を学習対象にする（下の欄、ちょっと隠れてるからちゃんと全部コピペしてね！）

```
import sys
from io import StringIO
import MeCab
import pandas as pd
def WordsAnalysis(sentence):
    try:
        mecab = MeCab.Tagger("-Ochasen")
        data = mecab.parse(sentence)
        data = StringIO(data)
        data = pd.read_csv(data, sep="\t", header=None)       
        #名詞のみを抽出する
        data = data.loc[(data[3].str.find("名詞") >= 0)]
        texts = list(data.iloc[:, 0])
        return texts
    except:
        return []

```

---
### ⑤④で定義した関数を実行する

```
nounsData=[]
for sentence in doc:
    nounsData.append(WordsAnalysis(sentence))
```

---
### ⑥学習を行う

```
from gensim.models import word2vec
#Word2Vecモデルの学習
#sizeは特徴量の数、min_count未満の登場数の単語を無視、ある単語とその前後window数の単語に関係性を持たせる、iter回数分繰り返し計算
model = word2vec.Word2Vec(tokeData, size=100, min_count=5,
                            window=5, iter=3)
#モデルの保存、""内はモデルのファイル名
model.save("neko.nouns.model")
```

---
### 疑問
1.このモデルデータ（学習結果）をどうやって確認するんだろう（どうせなら類似単語の検索とかしてみたい）
2.このモデルデータの精度をあげるにはどうすればいいんだろう
3.感情のファイルを使って学習するにはどうすればいいのだろう（みんながまとめてくれたファイルを使いたい）
