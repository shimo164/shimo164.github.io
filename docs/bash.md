* 区切りは空白。コンマで区切るとコンマが文字列扱いになる

### 配列

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

### 条件判定

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

### if文

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

### case文

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

### while文

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


### for文
* for .. do .. done

```sh
for i in 1 2 3 4 5
do
  echo $i
done

`seq 1 5` とかくと連続数になる
```

### キー入力
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

### ファイルを読む

(省略)

### 関数を作る

```sh

hello() {
  echo "hello $1 and $2"
  local i=5
  echo $i
}

hello Mike Tom
echo $i
```
