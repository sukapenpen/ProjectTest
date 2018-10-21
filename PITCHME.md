# モデルを作るところまでのまとめ


---
## ①必要なもののインストール

### 下のをコピペして実行する（Shift + Enter）
```
!pip install gensim
!apt install aptitude
!aptitude install mecab libmecab-dev mecab-ipadic-utf8 git make curl xz-utils file -y
!pip install mecab-python3
```

---
## ②テキストファイルのアップロード

### この[我輩は猫である]()をダウンロードしてアップしてください！！！
```
from google.colab import files
uploaded = files.upload()
```

---
## ③
