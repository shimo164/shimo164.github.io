**Linuxコマンドメモ もくじ**

- [検索](#検索)
- [### findコマンドの使い方](#-findコマンドの使い方)
  - [findでfilenameを名前に含むファイルを探す](#findでfilenameを名前に含むファイルを探す)
  - [findで"string"を本文に含むファイルを探す](#findでstringを本文に含むファイルを探す)
  - [locate検索](#locate検索)
  - [findで更新時間で探す](#findで更新時間で探す)
  - [findで/var/.. を検索するときの注意](#findでvar-を検索するときの注意)
- [コマンド](#コマンド)
  - [lsコマンド](#lsコマンド)
  - [grepコマンド](#grepコマンド)
- [Tips いろいろ](#tips-いろいろ)
  - [Ubuntuがフリーズしたとき](#ubuntuがフリーズしたとき)
    - [pidを確認してkill](#pidを確認してkill)
    - [Ubuntuで安全に強制終了、強制再起動するコマンド link](#ubuntuで安全に強制終了強制再起動するコマンド-link)
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
  - [05 コマンド置換](#05-コマンド置換)
  - [算術式評価](#算術式評価)
  - [ファイルディスクリプタ](#ファイルディスクリプタ)
  - [リダイレクト](#リダイレクト)

## 検索

### findコマンドの使い方
---
filenameを探す。<directory> `./` `/home/`....。(自動でワイルドカード、再帰)
```
find <directory> [-type f/d]| grep filename
```
---
カレントディレクトリからfilenameを探す(自動で再帰)
```
find ./ -type f | grep filename
```
---
その1:patternを中に含むファイルを探す。-l:ファイル名だけを出力 -s:エラーメッセージをスキップ
```
find directory -type f | xargs grep -l -s pattern
```
---
その2:patternを中に含むファイルを探す。Rと*があるとファイル名にスペースがあっても有効。。-R:再帰。-n:マッチした行を出力。
```
find ./ -type f | xargs grep -R -n -s "pattern" *
```
---
directoryから階層1だけ探す
```
find directory -maxdepth 1 | grep filename
```


### findでfilenameを名前に含むファイルを探す

```
find ./ -type f | grep <filename>
```

`./` : カレントディレクトリより下で、という意味



### findで"string"を本文に含むファイルを探す

```find ./ -type f | xargs grep -l -s "string"```

例: pwd以下にあるファイルで、ファイル名にpyを含み、ファイルの中に b = .*driver の文字列があるものを探す。

```find ./ -type f | grep py | xargs grep -l -s "b = .*driver" ```


### locate検索

| Command             | Description                     |
| ------------------- | ------------------------------- |
| `locate aaa`        | aaaがpathに入っているものを探す |
| `locate aaa bbb`    | aaa OR bbb                      |
| `locate -A aaa bbb` | aaa AND bbb                     |
| `locate -b aaa`     | basenameのみ検索                |


### findで更新時間で探す

| Command                            | Description                   |
| ---------------------------------- | ----------------------------- |
| `find . -mtime X`                  | x+1 日前                      |
| `find . -mtime -X`                 | x+1 日前*以降*                |
| `find . -mtime 0`                  | 0-24時間前                    |
| `find . -mtime 1`                  | 24-48時間前                   |
| `find . -mtime -1`                 | 0-48時間前                    |
| `find . -mtime -12 -name \*.py`    | 12日以内に更新した.pyファイル |
| `find . -mtime -12 | grep "\.py$"` | 同上                          |


### findで/var/.. を検索するときの注意

`/var/`の中を探すとき、`find ./ | grep var`ではできない。正しくは`find /var/...` とする。`find ./` は `/home/` 以下を検索する。`/var/`はrootのサブディレクトリで`/home/`と同列。


## コマンド

### lsコマンド

| Command              | Description                   | Note                                                                                                                         |
| -------------------- | ----------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `ls -d */`           | ディレクトリのみ出力          |                                                                                                                              |
| `ls -l | grep ^d`    | ディレクトリのみ出力          | -lの出力でディレクトリはdで始まる                                                                                            |
| `ls -l | grep -v ^d` | ファイルだけを出力            | -v:上記の反対                                                                                                                |
| `ls -d .?*`          | .で始まるファイル             | regex ? => match exactly one                                                                                                 |
| `$ ls -l !(xxx)`     | ファイル名にxxxを**含まない** |                                                                                                                              |
| ls \| wc -l          | ファイル数をカウント          | [[link](https://unix.stackexchange.com/questions/1125/how-can-i-get-a-count-of-files-in-a-directory-using-the-command-line)] |




### grepコマンド

| オプション | grep出力                 |
| :--------: | ------------------------ |
|     -r     | 配下ディレクトリを対象に |
|     -v     | マッチしないものを出力   |
|     -n     | 行番号                   |
|     -H     | ファイル名あり           |
|     -h     | ファイル名なし           |
|     -l     | ファイル名のみ           |
|     -L     | マッチしないファイル名   |
|     -o     | 検索にマッチした部分だけ |
|     -q     | 結果を出力しない         |


```
# ファイル名にfooとbarを含むgrepをつなぐときはパイプ。
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



## Tips いろいろ

### Ubuntuがフリーズしたとき

#### pidを確認してkill

- Ctrl + Alt + F2 でコマンドウィンドウに移動 Ctrl + Alt + F7 で戻る
- `top`でプロセスpidを確認。`kill -9 <pid>` でkill

#### Ubuntuで安全に強制終了、強制再起動するコマンド [link](https://wiki.ubuntulinux.jp/UbuntuTips/Others/MagicSysRq)

- **終了** Alt+PrintScreenを押しながらR+S+E+I+U+**O**
- **再起動** Alt+PrintScreenを押しながらR+S+E+I+U+**B**
-

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


## シェルスクリプト本より

### パス名展開

| 記号    | 意味                          |
| ------- | ----------------------------- |
| ?       | 任意の1文字                   |
| *       | 任意の文字列                  |
| []      | []に含まれる、いずれか1文字   |
| [!],[^] | []に含まれない、いずれか1文字 |

ブレース展開

{}で指定した複数コマンドを順に実行する。
注意:　{}の中はコンマ区切りにするが、コンマの後にスペースを入れないこと

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
### 05 コマンド置換

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
