# Linuxコマンドメモ もくじ
  - [検索](#検索)
    - [findコマンドの使い方](#findコマンドの使い方)
    - [findで"filename"を名前に含むファイルを探す](#findでfilenameを名前に含むファイルを探す)
    - [findで"string"を本文に含むファイルを探す](#findでstringを本文に含むファイルを探す)
    - [locate検索](#locate検索)
    - [findで更新時間で探す](#findで更新時間で探す)
    - [findで/var/.. を検索するときの注意](#findでvar-を検索するときの注意)
  - [Tips いろいろ](#tips-いろいろ)
    - [rmでディレクトリ削除](#rmでディレクトリ削除)
    - [プロセスが固まったとき](#プロセスが固まったとき)
    - [lsコマンド](#lsコマンド)
    - [mkdirで深いディレクトリを同時に作る](#mkdirで深いディレクトリを同時に作る)
    - [今いるディレクトリを開く](#今いるディレクトリを開く)
    - [grep](#grep)
    - [`cd -` で直前にいたディレクトリに戻る](#cd---で直前にいたディレクトリに戻る)
    - [whichコマンドで実行ファイルのフルパスを出力](#whichコマンドで実行ファイルのフルパスを出力)
    - [readlinkでfull pathを出力](#readlinkでfull-pathを出力)


## 検索

### findコマンドの使い方

`find <directory> | grep filename` filenameを探す。findのあとに探索ディレクトリ(例 ./, /home/)、-type f/dなどを入れることも可能。自動でワイルドカード、再帰になる。

`find ./ -type f | grep filename` カレントディレクトリから再帰でfilenameを探す。

`find directory -type f | xargs grep -l -s searchword` searchwordを中に含むファイルを探す。-lファイル名だけを出力 -sエラーメッセージをスキップ

`find dirctory -maxdepth 1 | grep filename` dirctoryから階層1だけ探す。


### findで<filename>を名前に含むファイルを探す

`find ./ -type f | grep <filename>`

`./` : カレントディレクトリより下で、という意味

grepで正規表現の利用 filename$ するとfilenameで終わるファイル

### findで"string"を本文に含むファイルを探す

`find ./ -type f | xargs grep -l -s "string"`

### locate検索

command | meaning
----|----
`locate aaa` | aaaがpathに入っているものを探す
`locate aaa bbb` | aaa OR bbb
`locate -A aaa bbb`| aaa AND bbb
`locate -b aaa` | basenameのみ検索


### findで更新時間で探す

command | meaning
---|---
`-mtime X` | x+1 日前
`-mtime -X` | x+1 日前*以降*
`-mtime 0` | 0-24時間前
`-mtime 1` | 24-48時間前
`-mtime -1` | 0-48時間前

`find . -mtime -12 -name \*.py`

`find . -mtime -12 |grep "\.py$"`

12日以内に更新した.pyファイルを探す(どちらも使えるが、.pyへのマッチが違う)

### findで/var/.. を検索するときの注意

`/var/`の中を探すとき、`find ./ | grep var`ではできない。`/var/`はrootのサブディレクトリで`/home/`と同列にある。
`find ./` は `/home/` 以下を検索するということである。

## Tips いろいろ



### rmでディレクトリ削除

ディレクトリを削除するときは、`rm -rf direname` で丸ごと削除。

### プロセスが固まったとき

- Ctrl + Alt + F2 でコマンドウィンドウに移動 Ctrl + Alt + F7 で戻る。
- `top`でプロセスpidを確認。`kill -9 <pid>` でkill。

### lsコマンド

|Command| Explanation|
|---|---|
|`ls -d */`| ディレクトリのみ出力|
|`ls -l | grep ^d`| ディレクトリのみ出力（-lの出力でディレクトリはdで始まる）|
|`ls -l | grep -v ^d`| ファイルだけを出力（-v上記の反対）|
|`ls -d .?*` | .で始まるファイル。※regex ?は"match exactly one"|
|`$ ls -l !(*csv)` |csvをファイル名に**含まない**ファイルをlsする|




パス名展開

記号|意味
---|---
? | 任意の1文字
* | 任意の文字列
[] |[]に含まれる、いずれか1文字
[!],[^]|[]に含まれない、いずれか1文字

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

記法|変数が指定されている|未指定・空白|:省略|備考
--|--|--|--|--
`${変数:-Hoge}`|指定された値|Hoge|空白を値とみなす|-
`${変数:=Hoge}`|指定された値|Hoge|空白を値とみなす|変数=Hogeを代入
`${変数:+Hoge}`|Hoge|空白|空白を値とみなす|-


記法|出力
--|--
`${変数:?Hoge}`|エラーメッセージをHoge部分で指定できる。

`${#変数:n}`| 変数のn文字目以降

`${変数: -n}`| 変数の末尾からn文字目以降。マイナスの前にスペース。

`${変数:n:m}`| 変数のn文字目から**m文字(数)**

`${変数:n:-m}`| 変数のn文字目から**末尾m文字目まで**

`${#変数}`| 変数の文字数

※文字は(0始まり)
※`変数`を`配列[@]`にすると配列の要素を指定できる


## パターンを指定して切り出す
## 置換してから展開する

## 05 コマンド置換

(command)とすると、()の中のcommandが実行され、標準出力が文字列で展開される

$ touch $(date +%Y-%m-%d).txt

とすると2021-01-19.txtができる。

$()の代わりに ``とバッククォートで囲んでもいいが、やりにくい

## 算術式評価



ファイルディスクリプタ

プロセスから開かれたファイルに割り当てられる番号。プロセスからファイルを参照する際の識別子。

name  | number
--    | :--:
stdin | 0
stdout| 1
stderr| 2

## リダイレクト

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
```ll

### mkdirで深いディレクトリを同時に作る

`mkdir -p dir1/dir2` option:-pをつける。dir1がなくても同時に作成する。


`mkdir dir1/dir2`

はエラー。`mkdir: cannot create directory‘dir1/dir2’: No such file or directory`

### 今いるディレクトリを開く

`nautilus .`

### grep

`ls -l | grep foo | grep bar` grepをつなぐときはパイプ

`ls -l | grep -v foo` 含まないときは `-v` をつける

### `cd -` で直前にいたディレクトリに戻る

間違ってディレクトリを移動したときなどに便利。

### whichコマンドで実行ファイルのフルパスを出力

```
which python
/home/user/anaconda3/bin/python

which git
/usr/bin/git
```
### readlinkでfull pathを出力

`readlink -f filename.txt`
