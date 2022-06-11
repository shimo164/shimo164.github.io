- [shell script の基本](#shell-script-の基本)
  - [変数](#変数)
  - [sh の実行](#sh-の実行)
  - [環境変数](#環境変数)
  - [sleep](#sleep)
  - [配列](#配列)
    - [grepから配列にする例](#grepから配列にする例)
  - [条件判定](#条件判定)
  - [数値を 2 回判定](#数値を-2-回判定)
  - [文字列が同一かを判定](#文字列が同一かを判定)
  - [文字列が空白であるかを判定](#文字列が空白であるかを判定)
  - [文字列が含まれているか判定](#文字列が含まれているか判定)
  - [sed で文字を消す](#sed-で文字を消す)
  - [if 文](#if-文)
  - [case 文](#case-文)
  - [while 文](#while-文)
  - [for 文](#for-文)
  - [キー入力](#キー入力)
  - [ファイル Read](#ファイル-read)
    - [文字列リストから配列にする](#文字列リストから配列にする)
    - [最後が改行で終わっていないファイルを対策する](#最後が改行で終わっていないファイルを対策する)
  - [ファイル Write](#ファイル-write)
  - [関数を作る](#関数を作る)
    - [関数で返り値を作る](#関数で返り値を作る)
    - [関数で arg を使う](#関数で-arg-を使う)
  - [try catch はない](#try-catch-はない)
- [例: ループで日付順に処理](#例-ループで日付順に処理)
- [例 実行している file の basename を出力](#例-実行している-file-の-basename-を出力)

参考) 新しいシェルプログラミングの教科書 三宅 英明

# shell script の基本

- shebang `#!/bin/bash` を 1 行目に
- バックスラッシュ`\`で複数行に分けて書ける
- セミコロン`;`で区切ると、複数行をまとめて 1 行で書ける
- インデント、空白行は無視される(コマンド途中の改行は NG)
- Unix のコマンドを呼び出せる
- コメントは `#` で始める。1 行ずつしかコメントできない

## 変数

- 変数名の先頭にハイフン、数字は使えない
- 大文字小文字は[慣例に従う](https://stackoverflow.com/questions/673055/correct-bash-and-shell-script-variable-capitalization)
- 代入は `変数名=値`とする。※スペースを入れない
- 値にスペースを含む場合は`file='Document files'` のようにシングルクォートで囲む
- 変数の参照は`$`をつけて`$file` とする
- 値を設定せずに`$file`を呼び出した場合、空文字列が返る。※エラーにはならない
- {}で変数部分を明示的に指定。`$item=pen; echo I have a ${item}s`.
- コマンド実行結果を変数に入れるときは`変数名=$(コマンド)`とする。例:`tmp=$(man ls | head)`
- 配列はスペースで区切る。index は 0 始まり
  - `a=(1 2 3)`
  - コンマ区切りは文字扱いになるので NG
- 配列の呼び出しは{}でくくる
  - `echo ${a[0]}`
- 日付呼び出し。`$date`は日本語になるのでフォーマットするには
  - `echo $(date +%Y-%m-%d\ %H:%M:%S)` -> 2021-12-17 08:41:07
- `eval $(command)`でコマンド結果の文字列を実行する
  - 例:`eval $(cat tmp.txt | awk 'NR==1')` で tmp.txt の 1 行目のコマンドを実行する

## sh の実行

`bash hello.sh` bash コマンドで実行すると、実行権限がなくても実行できる

`./hello.sh`

1 行目に shebang を書いておく。sh ファイルに実行権限をつけておく。これは相対パスだが絶対パスでもよい。

## 環境変数

コマンドの直前で環境変数を定義すると、その時限りで有効になる ↓

```
CONFIG_FILE=/home/hoge.txt ./config.sh
```

## sleep

`sleep 1`: 1 秒

`sleep 0.1`: 0.1 秒

## 配列

```bash
a=(2 4 6) # indexは0始まり

echo $a  # 0 番目になるので注意
# 2

echo ${a[1]} # indexで指定するときは{}が必要
# 4

echo $a[1] # {}がないと、$a と "[1]"の連結になるので注意
# 2[1]

echo ${a[@]}  # @ で全ての要素。
# 2 4 6

echo $a[@]  # {}がないとき
# 2[@]

echo ${#a[@]}  # #a[@] は要素数
# 3

a[2]=10 # 代入
echo ${a[@]}
# 2 4 10

a+=(20 30) # 要素追加
echo ${a[@]}
# 2 4 10 20 30

echo `date` # Unixの日時表示

d=(`date`)
echo ${d[3]}  # その3番目の要素(曜日)

```

### grepから配列にする例

tmp.txt

```txt
    "local/services/aaa",
    "global/services/bbb",
    "global/services/ccc",
```

bash test.sh
```bash
SELECTION=( $(cat tmp.txt | grep global) )
# SELECTION="$(cat tmp.txt | grep global)"  # NG

echo ${SELECTION[@]}
# echo "${SELECTION[@]}"  # OK

IFS=', '  read -ra my_array <<<${SELECTION[@]}

echo ---

echo "${my_array[@]}"

for item in "${my_array[@]}"; do
	echo ===
	echo $item
	base=$(basename $item | tr -d \")
	echo $base
done
```

出力
```
"global/services/bbb", "global/services/ccc",
---
"global/services/bbb" "global/services/ccc"
"global/services/ccc"
===
"global/services/bbb"
bbb
===
"global/services/ccc"
ccc
```


## 条件判定

**注意。判定に使うカッコ[ ] で前後にスペースを入れること。`: command not found`というエラーが出る。**

```sh
test 1 -eq 1; echo $?
test 1 -eq 1 -a 2 -eq 2; echo $?

test で判定した結果がtrueなら0

# 数値の判定

-ne not equal
-gt greater than
-ge greater than or equal
-lt less than
-le less than or equal


# 文字列の判定

=
!=

# ファイルの判定

-nt (newer than)
-ot
-e exsist
-d directory

# 論理演算子

-a (and)
-o (or)
!
```

## 数値を 2 回判定

```sh
if [[ $cnt -eq 0 ]] || [[ $cnt -eq 1 ]]; then
```

## 文字列が同一かを判定

```sh
if [[ "$T" == "$S" ]]; then
	echo "Identical string!"
else
	echo "No!"
fi
```

## 文字列が空白であるかを判定

NOTE: "$var" , not $var (no quatation)

```sh
if [ -z "$var" ]; then
  echo "\$var is empty"
else
  echo "\$var is NOT empty"
fi
```

https://www.cyberciti.biz/faq/unix-linux-bash-script-check-if-variable-is-empty/

## 文字列が含まれているか判定

```sh
STR='This is a test.'
SUB='test'
if [[ "$STR" == *"$SUB"* ]]; then
	echo "Found -> "$SUB
fi
```

## sed で文字を消す

```sh
name='[ "aaaa", "bbbb" ]'
echo $name
# " を消している。
name=$(echo $name | sed -E 's/"//g')
echo $name
# [ ] も消している 。縦棒で「または」バックスラッシュでエスケープ
name=$(echo $name | sed -E 's/"|\[|\]//g')

```

## if 文

`if .. elif .. else .. fi`

```sh
x=70
if [ $x -gt 60 ]; then # [  ] の前後は文字列がくるとエラー
  echo "ok!"
elif [ $x -gt 40 ]; then
  :  # 何もしないときはコロン
else
  echo "ng!..."
fi
```

ファイルが存在しなければ exit

```sh
if [ ! -f cdk.json ]; then
  printf "\n--app is required either in command-line, in cdk.json or in ~/.cdk.json.\n"
  exit
fi
```

ファイルが空かどうか

```

if ! [ -s "/file" ];then
    exit
fi
or shorter with less obvious syntax

test -s "/file" && exit
test -s tests if a file exists and has non-zero size.
```

`[[ condition1 && condition2 ]]` というように、 カッコ[]を二重にすると`-a, -o`の代わりに`&&, ||`が使える。

## case 文

- case .. esac
- ;; で処理終わり
- \*) がその他の場合

```sh

signal="red"
case $signal in
  "red")
    echo "stop!"
    ;;
  "yellow")
    echo "caution!"
    ;;
  "green")
    echo "go!"
    ;;
  *)
    echo "..."
    ;;
esac
```

## while 文

- while : .. do .. done
- while の直後は要スペース

```sh
i=0
while :
do
  i=`expr $i + 1`

  if [ $i -eq 3 ]; then
    continue
  fi

  if [ $i -gt 10 ]; then
    break
  fi

  echo $i
done
```

## for 文

- for .. do .. done

```sh
for i in 1 2 3 4 5
do
  echo $i
done

`seq 1 5` とかくと連続数になる
```

## キー入力

key インプットをコンソール上に出力

```sh
while :
do
  read key
  echo "you passed $key"
  if [ $key = "end" ]; then
    break
  fi
done

```

## ファイル Read

Read

https://stackoverflow.com/questions/7427262/how-to-read-a-file-into-a-variable-in-shell

https://linuxhint.com/read_file_line_by_line_bash/

### 文字列リストから配列にする

```sh
IFS=', ' read -ra my_array <<<${LIST[@]}

for item in "${my_array[@]}"; do
  echo $item
done
```

入力の記号も文字として読むので注意

```sh
LIST='["item1", "item2"]'
["item1"
"item2"]

LIST='"item1", "item2"'
["item1"
"item2"]

LIST='item1, item2'
item1
item2
```

### 最後が改行で終わっていないファイルを対策する

```
# test.txt
test1
test2
test3  # <- End with this line.
```

```sh
#!/bin/bash
filename='test.txt'

while read line; do
	echo "$line"
done <$filename

# read all lines
while IFS= read line || [[ -n $line ]]; do
	echo "$line"
done <$filename
```

## ファイル Write

echo "test" >> test.txt

※ >> で追記にすること。> でリダイレクトすると上書きされる

## 関数を作る

注:先に宣言しないと使えない
注:呼び出すときは()いらない

```sh

hello() {
  echo "hello $1 and $2"
  local i=5
  echo $i
}

hello Mike Tom
echo $i  # empty
```

### 関数で返り値を作る

return は数値を返す。変数を返すときは echo を使う。

関数内の複数の echo をまとめて 1 つとして受けとることになる。

```sh
#!/bin/bash
func_test() {

	echo "hi!"

	result="my result"

	echo $result
}

echo "hello"

from_func=$(func_test)

echo $from_func

# Output:
# hello
# hi! my result
```

### 関数で arg を使う

$1, $2,...を使う

```sh
func_arg() {
	echo $1
	echo $2
	echo $1+$2        # string
	echo $(($1 + $2)) # int
	echo $4           # empty
	echo $*           # all
}

func_arg 10 20

# Output:
# 10
# 20
# 10+20
# 30
#
# 10 20
```

変数は$1,$2 という書き方しかできないので、
置き換えると楽かもしれない

```sh
func_arg() {
	this=$1
	that=$2
	# $a ,,,

}

func_arg $this $that
```

## try catch はない

ないそうです。

こうやると無理やり書ける？

```sh
func(){

}

func

if $? != 0
  # catch...

```

# 例: ループで日付順に処理

- 日付を＋ 1 日してループ
- $d は GNU の Timestamp の処理を使っている。
  - d=$(date -d "$d+1day" +%Y%m%d) # yyyymmdd. Can set format.
  - d=$(date -I -d "$d + 1 day") # %y-%m-%d, yyyy-mm-dd
- (https://stackoverflow.com/questions/28226229/how-to-loop-through-dates-using-bash)

- echo するだけ

```
#!/bin/bash

d=20200801
while [ "$d" != 20210101 ]; do
  echo $d
  # touch $d".txt"
  # mkdir $d/

  d=$(date -d "$d+1day" +%Y%m%d)
done

# d=$(date -I -d "$d + 1 day")  # %y-%m-%d
```

- 日付をつけたファイルがあってフォルダを作成して移動する
- フォルダ自身を選択しないように"a"をフォルダ名の前につけている

```
#!/bin/bash

d=20201201
while [ "$d" != 20210101 ]; do
  mkdir "a"$d/
  mv $d* "a"$d/
  d=$(date -d "$d+1day" +%Y%m%d)
done
```

# 例 実行している file の basename を出力

$0 は args の 1 番目。つまりそのファイル名になる。

```
#!/bin/bash
me=$(basename "$0")

echo $0
echo $me
```
