**Linux コマンドメモもくじ**

- [grep](#grep)
- [find](#find)
    - [更新時間で探す](#更新時間で探す)
    - [find で/var/.. を検索するときの注意](#find-でvar-を検索するときの注意)
- [locate](#locate)
- [ls](#ls)
- [sed](#sed)
  - [書式](#書式)
  - [オプション](#オプション)
- [tree](#tree)
- [awk](#awk)
  - [オプション](#オプション-1)
  - [Tips](#tips)
  - [例](#例)

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
