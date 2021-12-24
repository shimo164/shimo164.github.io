
- [shell scriptの基本](#shell-scriptの基本)
  - [変数](#変数)
  - [shの実行](#shの実行)
  - [環境変数](#環境変数)
  - [sleep](#sleep)
  - [配列](#配列)
  - [条件判定](#条件判定)
  - [if文](#if文)
  - [case文](#case文)
  - [while文](#while文)
  - [for文](#for文)
  - [キー入力](#キー入力)
  - [ファイルを読む](#ファイルを読む)
  - [関数を作る](#関数を作る)
  - [try catchはないらしい](#try-catchはないらしい)
- [例forループで日付を替えて処理をする](#例forループで日付を替えて処理をする)
- [例 実行しているfileのbasenameを出力](#例-実行しているfileのbasenameを出力)

シェルスクリプトの教科書より

# shell scriptの基本

- shebang `#!/bin/bash` を1行目に
- バックスラッシュ`\`で複数行に分けて書ける
- セミコロン`;`で区切ると、複数行をまとめて1行で書ける
- インデント、空白行は無視される(コマンド途中の改行はNG)
- Unixのコマンドを呼び出せる
- コメントは `#` で始める。1行ずつしかコメントできない

## 変数
- 変数名の先頭にハイフン、数字は使えない
- 代入は `変数名=値`とする。※スペースを入れない
- 値にスペースを含む場合は`file='Document files'`　のようにシングルクォートで囲む
- 変数の参照は`$`をつけて`$file` とする
- 値を設定せずに`$file`を呼び出した場合、空文字列が返る。※エラーにはならない
- {}で変数部分を明示的に指定。`$item=pen; echo I have a ${item}s`.
- コマンド実行結果を変数に入れるときは`変数名=$(コマンド)`とする。例:`tmp=$(man ls | head)`
- 配列はスペースで区切る。indexは0始まり
  - `a=(1 2 3)`
  - コンマ区切りは文字扱いになるのでNG
- 配列の呼び出しは{}でくくる
  - `echo ${a[0]}`
- 日付呼び出し。`$date`は日本語になるのでフォーマットするには
  -  `echo $(date +%Y-%m-%d\ %H:%M:%S)` -> 2021-12-17 08:41:07
-  `eval $(command)`でコマンド結果の文字列を実行する
   -  例:`eval $(cat tmp.txt | awk 'NR==1')` でtmp.txtの1行目のコマンドを実行する


## shの実行

`bash hello.sh` bashコマンドで実行すると、実行権限がなくても実行できる

`./hello.sh`

1行目にshebangを書いておく。shファイルに実行権限をつけておく。これは相対パスだが絶対パスでもよい。

## 環境変数

- コマンドの直前で環境変数を定義すると、その時限りで有効になる↓
CONFIG_FILE=/home/hoge.txt ./config.sh

## sleep

`sleep 1`: 1秒

`sleep 0.1`: 0.1秒

## 配列

```bash
a=(2 4 6) # i=0, 1, 2

echo $a  # 0 番目
echo ${a[1]} # 1番目 添字で指定するときは{}が必要
echo $a[1] # {}がないと$a と 文字列[1]の連結

echo ${a[@]}  # @ で全て
echo ${#a[@]}  # #a[@] は要素数

a[2]=10  # 代入
echo ${a[@]}

a+=(20 30) # 要素追加
echo ${a[@]}

echo `date` # Unixの日時表示
d=(`date`)
echo ${d[3]}  # その3番目の要素
```

## 条件判定

```sh
test 1 -eq 1; echo $?
test 1 -eq 1 -a 2 -eq 2; echo $?

test で判定した結果がtrueなら0

-ne not equal
-gt greater than
-ge greater than or equal
-lt less than
-le less than or equal


**文字列の判定**

=
!=

**ファイルの判定**

-nt (newer than)
-ot
-e exsist
-d directory

**論理演算子**

-a (and)
-o (or)
!
```

## if文

* if .. elif .. else .. fi

```sh
#!/bin/bash

x=70
if [ $x -gt 60 ]; then # [  ] の前後は文字列がくるとエラー
  echo "ok!"
elif [ $x -gt 40 ]; then
  echo "soso..."
else
  echo "ng!..."
fi

```

## case文

* case .. esac
* ;; で処理終わり
* *) がその他の場合

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

## while文

* while : .. do .. done
* while の直後は要スペース
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


## for文
* for .. do .. done

```sh
for i in 1 2 3 4 5
do
  echo $i
done

`seq 1 5` とかくと連続数になる
```

## キー入力
keyインプットをコンソール上に出力

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

## ファイルを読む

(省略)

## 関数を作る

```sh

hello() {
  echo "hello $1 and $2"
  local i=5
  echo $i
}

hello Mike Tom
echo $i
```

## try catchはないらしい

ないそうです


# 例forループで日付を替えて処理をする

- 日付をつけたフォルダをまとめている
- フォルダ自身を選択しないように"a"をYEARの前につけている
- $dはGNUのTimestampの処理を使っている。

```
#!/bin/bash

d=20200801
while [ "$d" != 20210101 ]; do
  echo $d
  # touch $d".txt"
  mkdir "a"$d/
  mv $d* "a"$d/

  d=$(date -d "$d+1day" +%Y%m%d)
done

# d=$(date -I -d "$d + 1 day")  # %y-%m-%d
# https://stackoverflow.com/questions/28226229/how-to-loop-through-dates-using-bash
```

# 例 実行しているfileのbasenameを出力

$0は  argsの1番目。つまりそのファイル名になる。

```
#!/bin/bash
me=$(basename "$0")

echo $0
echo $me
```
