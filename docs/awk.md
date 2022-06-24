# awk

shell コマンドの中で、出力を抽出

```
awk [オプション] [コマンド] [ファイル……]
```

http://www.kt.rim.or.jp/~kbk/gawk/gawk_toc.html


## 入力
- 行ごとに、区切り(デフォルトは空白)でフィールドに分けられる
  - ex. "This is mine." で $1はThis $2はis $3はmine. $0は全文
- パイプでも、ファイルでも入力可能
- 複数行のばあい、各行ごとに処理される
- コマンド部分はシングルクォート

```
# '{print}'で全部出力。{print $0}と同じ
echo "This is mine." | awk '{print}'
This is mine.

echo "This is mine." | awk '{print $0}'
This is mine.

# $1で1番目
echo "This is mine." | awk '{print $1}'
This

# $2で2番目
echo "This is mine." | awk '{print $2}'
is

# $3で3番目
echo "This is mine." | awk '{print $3}'
mine.

# 1,2を続けて出力
echo "This is mine." | awk '{print $1 $2}'

# 1,2を続けて出力。カンマを入れるとスペースが入る
echo "This is mine." | awk '{print $1, $2}'

# 2と3の間に文字列を入れている
echo "This is mine." | awk '{print $1, $2, "not", $3}'

# ダブルクォートにするとawkが失敗して全部出力される
echo "This is mine." | awk "{print $2}"
This is mine.

# ファイルから読む
echo 'This is mine.' > file.txt
awk '{print $1}' file.txt
This

# 複数行から読む
echo $'This is mine.\nThat is yours.' | awk '{print $1}'
This
That

# 同上
echo $'This is mine.\nThat is yours.' > file.txt
awk '{print $1}' file.txt
This
That
```

TODO 再構成する
- フィールド、前後の入れ替え、フィールドの値書き換え
- フィールドの判定方法。
- if文
- 指定行だけにawk


## オプション

| オプション | 意味                                         |
| ---------- | -------------------------------------------- |
| -f         | awk スクリプトが書かれたファイルを指定する   |
| -F         | 区切り文字を指定する（デフォルトは空白文字） |
| -v         | 変数名=値を定義する                          |


### -f: awk スクリプトが書かれたファイルを指定する

```
echo '{print $1}' > script
echo 'This is mine.' | awk -f script
This
```

### -F: 区切り文字を指定する

```
# i で区切る。-Fiとつなげて書いてもよい。
echo "This is mine." | awk -F i '{print $1, $2}'
Th s

# $2,$3はスペースも含むことに注意
echo "This is mine." | awk -F i '{print $3}'
s m

```

### -v: 変数名=値を定義する

awkコマンドの中に変数を使いたいとき。-v var="$hoge" とする。ダブルクォートなので注意
```
hello="Hello World!"
echo "This is mine." | awk -v h="$hello" '{print h, $0}'
Hello World! This is mine.
```



## Tips

- 行ごとに実行される
- awk '...' を awk "..." とすると間違い。全て出力してしまう。
- awk を aws とタイプしてしまう間違い(AWS の CLI コマンド)。
- $0 が全項目、$1は1番目、、、$NF は最後

## 例

ls -la の出力で、最初($1)と最後($NF)だけを print

```
ls -la | awk '{print $1, $NF}'
```

---

4 行目と 7 行目(||)、4 行目から 7 行目(&&)を抽出

```
ls -la | awk 'NR==4 || NR==7'
ls -la | awk 'NR>=4 && NR<=7'
```

---

ls -la の出力で、サイズ($5)が 1000 バイトより大きいものだけを出力

```
ls -la | awk '$5 >= 1000 { print }'
```

---

ls -la の出力の各行の前に、整数をインクリメントで出力

```
ls -la | awk 'BEGIN{i=0}{print ++i,$0}'
```

---

ファイルに対して実行する

```
awk 'BEGIN{i=0}{print ++i,$0}' /etc/shells
```

---

「BEGIN {アクション}」: 処理を開始する前に実行する

「END {アクション}」: 最終行まで終えた後に実行したい処理がある場合

参考: https://atmarkit.itmedia.co.jp/ait/articles/1706/09/news013.html

---

日付を time 変数として作って awk の各行の前につける。code という文字の入ったプロセスを抜き出している。[c]ode としているのは grep code コマンド自体を回避するため。

```
ps aux | grep '[c]ode' | awk -v time="$(date +"%F %H:%M:%S")""    " '{print time, $0}'
```

---

ある文字列(change this one)を含むファイルを find して、ある文字列を置き換える(yes we can/we did)。change this one のところに yes we can を入れてもよい。 [参考](https://stackoverflow.com/questions/13838973/using-find-grep-and-sed-together)

```
find ./ -type f -exec grep -i "change this one" -l {} \; -exec sed -i 's/yes we can/we did/g' {} \;
```
