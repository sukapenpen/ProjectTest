# モデルを作るところまでのまとめ


---
## ①必要なもののインストール

このページ以降、下の欄のコードをコピペして実行する（Shift + Enter）
```
!pip install gensim
!apt install aptitude
!aptitude install mecab libmecab-dev mecab-ipadic-utf8 git make curl xz-utils file -y
!pip install mecab-python3
```

---
## ②テキストファイルのダウンロードとアップロード

[我輩は猫である](https://drive.google.com/drive/folders/1ckgg3mUbs7WkBbEbm53GIgClRGjEdspH?usp=sharing)を（大学のアカウントでログインしてから）ダウンロードしてから下のコードを実行してアップしてください！！！

```
from google.colab import files
uploaded = files.upload()
```

---
## ③ファイルを読み込んで配列に格納

配列に1文ずつ格納していきます

```
doc = []

f = open('wagahai.txt')

for line in f:
    doc.append(line)

f.close()
```

---
## ④形態素解析をする関数を定義する

```
import sys
from io import StringIO
import MeCab
import pandas as pd
def tokenize(sentence):
    try:
        mecab = MeCab.Tagger("-Ochasen")
        data = mecab.parse(sentence)
        data = StringIO(data.decode('utf-8'))
        data = pd.read_csv(data, sep='\t', header=None)       
        #名詞のみを抽出
        data = data.loc[(data[3].str.find("名詞") >= 0)]
        texts = list(data.iloc[:, 0])
        return texts
    except:
        return []

```

---
