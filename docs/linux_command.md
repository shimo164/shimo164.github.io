**Linuxコマンドメモもくじ**

- [grepコマンド](#grepコマンド)
- [findコマンド](#findコマンド)
    - [更新時間で探す](#更新時間で探す)
    - [findで/var/.. を検索するときの注意](#findでvar-を検索するときの注意)
- [locate検索](#locate検索)
- [lsコマンド](#lsコマンド)
- [awkコマンド](#awkコマンド)
  - [オプション](#オプション)
  - [Tips](#tips)
  - [例](#例)
- [Tipsいろいろ](#tipsいろいろ)
    - [コマンド中でランダム数を入れる](#コマンド中でランダム数を入れる)
    - [$RANDOM でランダム数を出せる](#random-でランダム数を出せる)
    - [rmでディレクトリを削除するときは、`rm -rf direname` で丸ごと削除](#rmでディレクトリを削除するときはrm--rf-direname-で丸ごと削除)
    - [`nautilus .` で今いるディレクトリを開く](#nautilus--で今いるディレクトリを開く)
    - [`mkdir -p`で深いディレクトリを同時に作る](#mkdir--pで深いディレクトリを同時に作る)
    - [lsof -iでポート使用状況を確認する](#lsof--iでポート使用状況を確認する)
    - [`cd -` で直前にいたディレクトリに戻る](#cd---で直前にいたディレクトリに戻る)
    - [whichコマンドで実行ファイルのフルパスを出力](#whichコマンドで実行ファイルのフルパスを出力)
    - [readlinkでファイルのfull pathを出力](#readlinkでファイルのfull-pathを出力)
- [シェルスクリプト本より](#シェルスクリプト本より)
    - [パス名展開](#パス名展開)
    - [パラメータ展開](#パラメータ展開)
    - [パターンを指定して切り出す](#パターンを指定して切り出す)
    - [置換してから展開する](#置換してから展開する)
    - [05コマンド置換](#05コマンド置換)
    - [算術式評価](#算術式評価)
    - [ファイルディスクリプタ](#ファイルディスクリプタ)
    - [リダイレクト](#リダイレクト)

# grepコマンド

| オプション | grep出力                  |
| :--------: | ------------------------- |
|     -r     | 配下ディレクトリを対象に  |
|     -v     | マッチしないものを出力    |
|     -n     | 行番号                    |
|     -H     | ファイル名あり            |
|     -h     | ファイル名なし            |
|     -l     | ファイル名のみ            |
|     -L     | マッチしないファイル名    |
|     -o     | 検索にマッチした部分だけ  |
|     -q     | 結果を出力しない          |
|     -s     | --no-messagesエラー無出力 |

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

# findコマンド

| オプション | Description            |
| :--------: | ---------------------- |
| -type f/d  | only File/Directory    |
| -maxdepth  | 検索対象の階層         |
|   -name    | ファイル名(wildcard可) |
|   -mtime   | 更新日時               |
|  -newermt  | この日より新しいもの   |


**NOTE**

- `/home/user/` -> /home/user/ 以下で
- `./` -> カレントディレクトリ以下で
- `grep`と組み合わせることが多い

---

filenameを探す(自動でワイルドカード、再帰)

```
find <directory> [-type f/d]| grep filename
```
---
カレントディレクトリからfilenameを探す(自動で再帰)
```
find ./ -type f -name filename
```

```
find ./ -type f | grep filename
```

---
directoryから階層1だけをfindの対象にする

```
find directory -maxdepth 1 | grep filename
```
---
patternという文字列を中に含むファイルを探す(その1)
- `-l` ファイル名だけを出力
- `-s` エラーメッセージをスキップ

```
find directory -type f | xargs grep -l -s pattern
```

---
patternという文字列を中に含むファイルを探す(その2)。Rと*があるとファイル名にスペースがあっても有効。
-  `-R` 再帰
- `-n` マッチした行を出力

```
find ./ -type f | xargs grep -R -n -s "pattern" *
```

---
### 更新時間で探す
`-mtime n` **last modified n*24 hours ago.**

| Command                            | Description                   |
| ---------------------------------- | ----------------------------- |
| `find . -mtime X`                  | x+1日前                      |
| `find . -mtime -X`                 | x+1日前から今まで             |
| `find . -mtime 0`                  | 0-24時間前                    |
| `find . -mtime 1`                  | 24-48時間前                   |
| `find . -mtime -1`                 | 0-48時間前                    |
| `find . -mtime -12 -name \*.py`    | 12-13日以内に更新した.pyファイル |
| `find . -mtime -12 | grep "\.py$"` | 同上                          |
| `find . -type f -newermt 2021-6-1 ! -newermt 2021-7-1` | 期間内  |
| `find . -maxdepth 1 -type f -newermt $(date --date="1 days ago" +%F)` | 昨日の0時以降に更新された |
|`find . -maxdepth 1 -type f -newermt $(date +%F)` | 今日の0時以降に更新された|

---
`-exec`コマンドと組合せて、マッチしたファイルを操作する

2020-09-01に更新したファイルが**見つかったら <dir_2/> に移動させる** [link](https://unix.stackexchange.com/questions/154818/how-to-integrate-mv-command-after-find-command)
```
find . -maxdepth 1  -newermt 2020-09-1 ! -newermt 2020-09-2 -exec mv -t <dir_2>/ {} +
```

### findで/var/.. を検索するときの注意
`/var/`の中を探すとき、`find ./ | grep var`ではできない。正しくは`find /var/...` とする。

`find ./` は `/home/` を検索するという意味。`/var/`はrootのサブディレクトリで`/home/`と同列。

# locate検索

| Command             | Description                     |
| ------------------- | ------------------------------- |
| `locate aaa`        | aaaがpathに入っているものを探す |
| `locate aaa bbb`    | aaa OR bbb                      |
| `locate -A aaa bbb` | aaa AND bbb                     |
| `locate -b aaa`     | basenameのみ検索                |

# lsコマンド

| Command              | Description                   | Note                              |
| -------------------- | ----------------------------- | --------------------------------- |
| `ls -d`              | ディレクトリの中を出力しない  |                                   |
| `ls -d */`           | ディレクトリのみ出力          |                                   |
| `ls -l | grep ^d`    | ディレクトリのみ出力          | -lの出力でディレクトリはdで始まる |
| `ls -l | grep -v ^d` | ファイルだけを出力            | -v:上記の反 対                    |
| `ls -d .?*`          | .で始まるファイル             | regex ? => match exactly          |
| `$ ls -l !(xxx)`     | ファイル名にxxxを**含まない** |                                   |
| ls \| wc -l          | ファイル数をカウント          | [link][ref1]                      |

[ref1]:https://unix.stackexchange.com/questions/1125/how-can-i-get-a-count-of-files-in-a-directory-using-the-command-line

# awkコマンド
shellコマンドの中で、出力を抽出したりする
```
awk [オプション] [コマンド] [ファイル……]
```

## オプション

| オプション | 意味                                         |
| ---------- | -------------------------------------------- |
| -f         | awkスクリプトが書かれたファイルを指定する    |
| -F         | 区切り文字を指定する（デフォルトは空白文字） |
| -v         | 変数名=値を定義する                          |

## Tips
- 行ごとに実行される
- awk '...' を awk "..." とすると間違い。全て出力してしまう。
- awk を aws とタイプしてしまう間違い(AWSのCLIコマンド)。

## 例
ls -la の出力で、最初($1)と最後($NF)だけをprint
```
$ ls -la | awk '{print $1, $NF}'
```

ls -la の出力で、サイズ($5)が1000バイトより大きいものだけを出力

```
$ ls -la | awk '$5 >= 1000 { print }'
```

「BEGIN {アクション}」: 処理を開始する前に実行する

ls -la の出力の各行の前に、整数をインクリメントで出力
```
$ ls -la | awk 'BEGIN{i=0}{print ++i,$0}'
```

ファイルに対して実行する
```
$ awk 'BEGIN{i=0}{print ++i,$0}' /etc/shells
```

「END {アクション}」: 最終行まで終えた後に実行したい処理がある場合

参考: https://atmarkit.itmedia.co.jp/ait/articles/1706/09/news013.html


# Tipsいろいろ

### コマンド中でランダム数を入れる

例: コマンドで指定するファイル名にランダム数を入れたい。

```
echo $RANDOM  # 0 to 32767

echo $(($RANDOM % 129))  # reminder: 0 to 128

touch file_$(($RANDOM % 1000)).txt  # file_num.txt 0 to 999
```

### $RANDOM でランダム数を出せる
[in the range 0 - 32767](https://tldp.org/LDP/abs/html/randomvar.html)

### rmでディレクトリを削除するときは、`rm -rf direname` で丸ごと削除
### `nautilus .` で今いるディレクトリを開く
### `mkdir -p`で深いディレクトリを同時に作る
`mkdir -p dir1/dir2` オプション-p(--parents)を使うと、親ディレクトリdir1がなくても同時に作成する。

このオプションがないと、エラーになる。`mkdir: cannot create directory‘dir1/dir2’: No such file or directory`

### lsof -iでポート使用状況を確認する
使用している全てのアドレスを表示。lsof: list open files , -i: Internet address

```
lsof -i
```

特定のポート(ex.5000)を見たい場合は

```
$ lsof -i :5000`
```

### `cd -` で直前にいたディレクトリに戻る
### whichコマンドで実行ファイルのフルパスを出力
```
which python
/home/user/anaconda3/bin/python

which git
/usr/bin/git
```
### readlinkでファイルのfull pathを出力
`readlink -f filename.txt`

# シェルスクリプト本より
### パス名展開
| 記号    | 意味                          |
| ------- | ----------------------------- |
| ?       | 任意の1文字                   |
| *       | 任意の文字列                  |
| []      | []に含まれる、いずれか1文字   |
| [!],[^] | []に含まれない、いずれか1文字 |

ブレース展開

{}で指定した複数コマンドを順に実行する。
注意: {}の中はコンマ区切りにするが、コンマの後にスペースを入れないこと

```
$ echo {foo,bar,baz}.txt
foo.txt bar.txt baz.txt

8から11まで
$ echo file-{8..11}.txt
file-8.txt file-9.txt file-10.txt file-11.txt

8から11まで、2ずつ増加
$ echo file-{8..11..2}.txt

文字も順に変えられる
$ echo file-{c..f}.txt
```

### パラメータ展開
`$HOME`は`${HOME}`とも書ける。

`:-`, `:=`, `:+`を使うと、変数が指定されているか、空白かなどによって返す値を変えることができる。

| 記法            | 変数が指定されている | 未指定・空白 | :省略            | 備考            |
| --------------- | -------------------- | ------------ | ---------------- | --------------- |
| `${変数:-Hoge}` | 指定された値         | Hoge         | 空白を値とみなす | -               |
| `${変数:=Hoge}` | 指定された値         | Hoge         | 空白を値とみなす | 変数=Hogeを代入 |
| `${変数:+Hoge}` | Hoge                 | 空白         | 空白を値とみなす | -               |

| 記法            | 出力                                     |
| --------------- | ---------------------------------------- |
| `${変数:?Hoge}` | エラーメッセージをHoge部分で指定できる。 |

`${#変数:n}`| 変数のn文字目以降

`${変数: -n}`| 変数の末尾からn文字目以降。マイナスの前にスペース。

`${変数:n:m}`| 変数のn文字目から**m文字(数)**

`${変数:n:-m}`| 変数のn文字目から**末尾m文字目まで**

`${#変数}`| 変数の文字数

※文字は(0始まり)
※`変数`を`配列[@]`にすると配列の要素を指定できる

### パターンを指定して切り出す
### 置換してから展開する
### 05コマンド置換
(command)とすると、()の中のcommandが実行され、標準出力が文字列で展開される

$ touch $(date +%Y-%m-%d).txt

とすると2021-01-19.txtができる。

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

stdoutとstderr出力を同じファイルにリダイレクトする

この2つは同じ。

`echo hello > file.txt 2>&1`

`echo hello &> file.txt`

この書き方はできない

`echo hello 2>&1 > file.txt `

`/dev/null` にリダイレクトすると消える

`ll`コマンドはlsのエイリアスになっている(.bashrcで設定)。

```
ll is aliased to `ls -alF'
```
