**Linux コマンドメモもくじ**

- [grep](#grep)
- [find](#find)
  - [更新時間で探す](#更新時間で探す)
  - [find で/var/.. を検索するときの注意](#find-でvar-を検索するときの注意)
- [locate](#locate)
- [ls](#ls)
- [sed](#sed)
- [tree](#tree)
- [awk](#awk)
  - [オプション](#オプション)
  - [Tips](#tips)
  - [例](#例)
- [Tips いろいろ](#tips-いろいろ)
  - [文字列の連結](#文字列の連結)
  - [コマンド中でランダム数を入れる](#コマンド中でランダム数を入れる)
  - [$RANDOM でランダム数を出せる](#random-でランダム数を出せる)
  - [rm でディレクトリを削除するときは、`rm -rf direname` で丸ごと削除](#rm-でディレクトリを削除するときはrm--rf-direname-で丸ごと削除)
  - [`nautilus .` で今いるディレクトリを開く](#nautilus--で今いるディレクトリを開く)
  - [`mkdir -p`で深いディレクトリを同時に作る](#mkdir--pで深いディレクトリを同時に作る)
  - [lsof -i でポート使用状況を確認する](#lsof--i-でポート使用状況を確認する)
  - [`cd -` で直前にいたディレクトリに戻る](#cd---で直前にいたディレクトリに戻る)
  - [which コマンドで実行ファイルのフルパスを出力](#which-コマンドで実行ファイルのフルパスを出力)
  - [readlink でファイルの full path を出力](#readlink-でファイルの-full-path-を出力)
  - [unzip -l で、解凍せずに中を見る](#unzip--l-で解凍せずに中を見る)
- [シェルスクリプト本より](#シェルスクリプト本より)
  - [パス名展開](#パス名展開)
  - [パラメータ展開](#パラメータ展開)
  - [パターンを指定して切り出す](#パターンを指定して切り出す)
  - [置換してから展開する](#置換してから展開する)
  - [コマンド置換](#コマンド置換)
  - [算術式評価](#算術式評価)
  - [ファイルディスクリプタ](#ファイルディスクリプタ)
  - [リダイレクト](#リダイレクト)

# grep

| オプション | grep 出力                  |
| :--------: | -------------------------- |
|     -r     | 配下ディレクトリを対象に   |
|     -v     | マッチしないものを出力     |
|     -n     | 行番号                     |
|     -H     | ファイル名あり             |
|     -h     | ファイル名なし             |
|     -l     | ファイル名のみ             |
|     -L     | マッチしないファイル名     |
|     -o     | 検索にマッチした部分だけ   |
|     -q     | 結果を出力しない           |
|     -s     | --no-messages エラー無出力 |

```
# ファイル名にfooとbarを含む。grepのANDをパイプでつなぐ。
ls -l | grep foo | grep bar

# ファイル名にfooを含まない。マッチしないものを出力ときは `-v` をつける。
ls -l | grep -v foo

# directory配下でtimeを含むファイル名だけを出力
grep -rl time directory

# directory配下でt..eを含む行番号とマッチした部分を出力
grep -rno t..e directory

# grepした結果を消す
find ... | grep ... | xargs rm

# grepで正規表現の利用(例:filenameで終わる)
... |grep filename$
```

その他 エラーを出力しない
[参考](https://stackoverflow.com/questions/6426363/how-can-i-have-grep-not-print-out-no-such-file-or-directory-errors)

```
grep pattern * -s -R -n
```

# find

| オプション | Description             |
| :--------: | ----------------------- |
| -type f/d  | only File/Directory     |
| -maxdepth  | 検索対象の階層          |
|   -name    | ファイル名(wildcard 可) |
|   -mtime   | 更新日時                |
|  -newermt  | この日より新しいもの    |

**NOTE**

- `/home/user/` -> /home/user/ 以下で
- `./` -> カレントディレクトリ以下で
- `grep`と組み合わせることが多い

---

filename を探す(自動でワイルドカード、再帰)

```
find <directory> [-type f/d]| grep filename
```

---

カレントディレクトリから filename を探す(自動で再帰)

```
find ./ -type f -name filename
```

```
find ./ -type f | grep filename
```

---

directory から階層 1 だけを find の対象にする

```
find directory -maxdepth 1 | grep filename
```

---

pattern という文字列を中に含むファイルを探す(その 1)

- `-l` ファイル名だけを出力
- `-s` エラーメッセージをスキップ

```
find directory -type f | xargs grep -l -s pattern
```

---

pattern という文字列を中に含むファイルを探す(その 2)。R と\*があるとファイル名にスペースがあっても有効。

- `-R` 再帰
- `-n` マッチした行を出力

```
find ./ -type f | xargs grep -R -n -s "pattern" *
```

---

### 更新時間で探す

`-mtime n` **last modified n\*24 hours ago.**

| Command                                                               | Description                        |
| --------------------------------------------------------------------- | ---------------------------------- | ---- |
| `find . -mtime X`                                                     | x+1 日前                           |
| `find . -mtime -X`                                                    | x+1 日前から今まで                 |
| `find . -mtime 0`                                                     | 0-24 時間前                        |
| `find . -mtime 1`                                                     | 24-48 時間前                       |
| `find . -mtime -1`                                                    | 0-48 時間前                        |
| `find . -mtime -12 -name \*.py`                                       | 12-13 日以内に更新した.py ファイル |
| `find . -mtime -12                                                    | grep "\.py$"`                      | 同上 |
| `find . -type f -newermt 2021-6-1 ! -newermt 2021-7-1`                | 期間内                             |
| `find . -maxdepth 1 -type f -newermt $(date --date="1 days ago" +%F)` | 昨日の 0 時以降に更新された        |
| `find . -maxdepth 1 -type f -newermt $(date +%F)`                     | 今日の 0 時以降に更新された        |

---

`-exec`コマンドと組合せて、マッチしたファイルを操作する

2020-09-01 に更新したファイルが**見つかったら <dir_2/> に移動させる** [link](https://unix.stackexchange.com/questions/154818/how-to-integrate-mv-command-after-find-command)

```
find . -maxdepth 1  -newermt 2020-09-1 ! -newermt 2020-09-2 -exec mv -t <dir_2>/ {} +
```

### find で/var/.. を検索するときの注意

`/var/`の中を探すとき、`find ./ | grep var`ではできない。正しくは`find /var/...` とする。

`find ./` は `/home/` を検索するという意味。`/var/`は root のサブディレクトリで`/home/`と同列。

# locate

| Command             | Description                        |
| ------------------- | ---------------------------------- |
| `locate aaa`        | aaa が path に入っているものを探す |
| `locate aaa bbb`    | aaa OR bbb                         |
| `locate -A aaa bbb` | aaa AND bbb                        |
| `locate -b aaa`     | basename のみ検索                  |

# ls

| Command          | Description                     | Note                     |
| ---------------- | ------------------------------- | ------------------------ | ------------------------------------ |
| `ls -d`          | ディレクトリの中を出力しない    |                          |
| `ls -d */`       | ディレクトリのみ出力            |                          |
| `ls -l           | grep ^d`                        | ディレクトリのみ出力     | -l の出力でディレクトリは d で始まる |
| `ls -l           | grep -v ^d`                     | ファイルだけを出力       | -v:上記の反 対                       |
| `ls -d .?*`      | .で始まるファイル               | regex ? => match exactly |
| `$ ls -l !(xxx)` | ファイル名に xxx を**含まない** |                          |
| ls \| wc -l      | ファイル数をカウント            | [link][ref1]             |

[ref1]: https://unix.stackexchange.com/questions/1125/how-can-i-get-a-count-of-files-in-a-directory-using-the-command-line

# sed

```sh

# 基本形
sed 's/aaa/bbb/g'

g をつけると何回でも置換。つけないと1回だけ置換。
-E, -r, --regexp-extended 拡張正規表現を使う
-i 編集してファイルを上書き保存する

# echoをパイプする # gがないので1つ目だけ
echo test | sed -E 's/t//'
Out:est

# 変数を上書きする Delete "#"、space
S=$(echo $S | sed -E 's/#\s//g')

# fileを対象にする
sed -E 's/aaa/bbb/g' tmp.txt

# 別fileに保存
sed -E 's/aaa/bbb/g' tmp.txt > tmp2.txt

# 同じfileに上書き
sed -Ei 's/aaa/bbb/g' tmp.txt

# 2行目だけを置換する
sed '2s/aaa/bbb/g' file

# 2から4行目だけを置換する
sed '2,4s/aaa/bbb/g' file

# \ でエスケープして/ を置換する
echo //path// | sed  's/\/\//\//g'

# 区切り文字を他の記号(ここでは%)にすれば/ をエスケープしなくてよい
echo //path// | sed  's%//%/%g'

```

# tree

ディレクトリの中身を表示

| Command      | Description            | Note          |
| ------------ | ---------------------- | ------------- |
| `-a`         | 全ファイル出力         |               |
| `-d`         | ディレクトリのみ出力   |               |
| `-d`         | ディレクトリのみ出力   |               |
| `-L level`   | level 層まで出力       |               |
| `-P pattern` | マッチするものを出力   | --ignore-case |
| `-I pattern` | マッチしないものを出力 | --ignore-case |

```
# そのまま
tree

# patternという文字列をファイル名に含む。
# *が前後に必要。^$は省略可能
# 検索ヒットしたフォルダ内のフォルダも出力される
tree -P '*pattern*'

# ...を含まない
tree -I 'example*|bin|lib'

```

# awk

shell コマンドの中で、出力を抽出

```
awk [オプション] [コマンド] [ファイル……]
```

## オプション

| オプション | 意味                                         |
| ---------- | -------------------------------------------- |
| -f         | awk スクリプトが書かれたファイルを指定する   |
| -F         | 区切り文字を指定する（デフォルトは空白文字） |
| -v         | 変数名=値を定義する                          |

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

# Tips いろいろ

### 文字列の連結

連結するには、文字列を続く。スペースは不要

```
echo "hoge1""hoge2"  # hoge1hoge2

a="hoge3"
echo $a"hoge4"  # hoge3hoge4
```

### コマンド中でランダム数を入れる

例: コマンドで指定するファイル名にランダム数を入れたい。

```
echo $RANDOM  # 0 to 32767

echo $(($RANDOM % 129))  # reminder: 0 to 128

touch file_$(($RANDOM % 1000)).txt  # file_num.txt 0 to 999
```

### $RANDOM でランダム数を出せる

[in the range 0 - 32767](https://tldp.org/LDP/abs/html/randomvar.html)

### rm でディレクトリを削除するときは、`rm -rf direname` で丸ごと削除

### `nautilus .` で今いるディレクトリを開く

### `mkdir -p`で深いディレクトリを同時に作る

`mkdir -p dir1/dir2` オプション-p(--parents)を使うと、親ディレクトリ dir1 がなくても同時に作成する。

このオプションがないと、エラーになる。`mkdir: cannot create directory‘dir1/dir2’: No such file or directory`

### lsof -i でポート使用状況を確認する

使用している全てのアドレスを表示。lsof: list open files , -i: Internet address

```
lsof -i
```

特定のポート(ex.5000)を見たい場合は

```
lsof -i :5000`
```

### `cd -` で直前にいたディレクトリに戻る

### which コマンドで実行ファイルのフルパスを出力

```
which python
/home/user/anaconda3/bin/python

which git
/usr/bin/git
```

### readlink でファイルの full path を出力

`readlink -f filename.txt`

### unzip -l で、解凍せずに中を見る

`unzip -l zipfile.zip`

# シェルスクリプト本より

### パス名展開

| 記号    | 意味                            |
| ------- | ------------------------------- |
| ?       | 任意の 1 文字                   |
| \*      | 任意の文字列                    |
| []      | []に含まれる、いずれか 1 文字   |
| [!],[^] | []に含まれない、いずれか 1 文字 |

ブレース展開

{}で指定した複数コマンドを順に実行する。
注意: {}の中はコンマ区切りにするが、コンマの後にスペースを入れないこと

```
echo {foo,bar,baz}.txt
foo.txt bar.txt baz.txt

8から11まで
echo file-{8..11}.txt
file-8.txt file-9.txt file-10.txt file-11.txt

8から11まで、2ずつ増加
echo file-{8..11..2}.txt

文字も順に変えられる
echo file-{c..f}.txt
```

### パラメータ展開

`$HOME`は`${HOME}`とも書ける。

`:-`, `:=`, `:+`を使うと、変数が指定されているか、空白かなどによって返す値を変えることができる。

| 記法            | 変数が指定されている | 未指定・空白 | :省略            | 備考             |
| --------------- | -------------------- | ------------ | ---------------- | ---------------- |
| `${変数:-Hoge}` | 指定された値         | Hoge         | 空白を値とみなす | -                |
| `${変数:=Hoge}` | 指定された値         | Hoge         | 空白を値とみなす | 変数=Hoge を代入 |
| `${変数:+Hoge}` | Hoge                 | 空白         | 空白を値とみなす | -                |

| 記法            | 出力                                       |
| --------------- | ------------------------------------------ |
| `${変数:?Hoge}` | エラーメッセージを Hoge 部分で指定できる。 |

`${#変数:n}`| 変数の n 文字目以降

`${変数: -n}`| 変数の末尾から n 文字目以降。マイナスの前にスペース。

`${変数:n:m}`| 変数の n 文字目から**m 文字(数)**

`${変数:n:-m}`| 変数の n 文字目から**末尾 m 文字目まで**

`${#変数}`| 変数の文字数

※文字は(0 始まり)
※`変数`を`配列[@]`にすると配列の要素を指定できる

### パターンを指定して切り出す

### 置換してから展開する

### コマンド置換

(command)とすると、()の中の command が実行され、標準出力が文字列で展開される

touch $(date +%Y-%m-%d).txt

とすると 2021-01-19.txt ができる。

$()の代わりに ``とバッククォートで囲んでもいいが、やりにくい

### 算術式評価

### ファイルディスクリプタ

プロセスから開かれたファイルに割り当てられる番号。プロセスからファイルを参照する際の識別子。

| name   | number |
| ------ | :----: |
| stdin  |   0    |
| stdout |   1    |
| stderr |   2    |

### リダイレクト

`>` (上書き)と、`>>` (追記)を選べる。

stdout と stderr 出力を同じファイルにリダイレクトする

この 2 つは同じ。

`echo hello > file.txt 2>&1`

`echo hello &> file.txt`

この書き方はできない

`echo hello 2>&1 > file.txt `

`/dev/null` にリダイレクトすると消える

`ll`コマンドは ls のエイリアスになっている(.bashrc で設定)。

```
ll is aliased to `ls -alF'
```
