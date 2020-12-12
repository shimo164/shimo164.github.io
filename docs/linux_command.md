## Linuxコマンドメモ　もくじ
<!--Contents-->
- [Linuxコマンドメモ　もくじ](#linuxコマンドメモもくじ)
  - [*mistake*](#mistake)
  - [lsコマンド](#lsコマンド)
  - [mkdirで深いディレクトリを同時に作る](#mkdirで深いディレクトリを同時に作る)
  - [grep](#grep)
  - [`cd -` で直前にいたディレクトリに戻る](#cd---で直前にいたディレクトリに戻る)
  - [findコマンドの使い方](#findコマンドの使い方)
  - [negative wildcard](#negative-wildcard)
  - [filenameという文字列を名前に含むファイルを探す](#filenameという文字列を名前に含むファイルを探す)
  - [stringを本文に含むファイルを探すとき](#stringを本文に含むファイルを探すとき)
  - [locate検索](#locate検索)
  - [今いるディレクトリを開く](#今いるディレクトリを開く)
  - [findで更新時間で探す](#findで更新時間で探す)
  - [whichコマンドで実行ファイルを探す](#whichコマンドで実行ファイルを探す)
  - [full pathを出力](#full-pathを出力)
<!--Contents-->

### *mistake*

I could not detect /var/ file-directory with `find ./ | grep var.` Because /var/ is subdirectory of root　like /home/ . And `find ./` means find under /home/ directory.

### lsコマンド

|Command| Explanation|
|---|---|
|`ls -d */`| ディレクトリのみ出力|
|`ls -l | grep ^d`| ディレクトリのみ出力（-l の出力でディレクトリはdで始まる）|
|`ls -l | grep -v ^d`| ファイルだけを出力（-v 上記の反対）|
|`ls -d .?*` | .で始まるファイル。※regex ?は"match exactly one"|

`ll`コマンドはlsのエイリアスになっている(.bashrcで設定)。
```
ll is aliased to `ls -alF'
```

### mkdirで深いディレクトリを同時に作る

`mkdir -p dir1/dir2`　option:-p をつける。dir1がなくても同時に作成する。


`mkdir dir1/dir2`

はエラー。`mkdir: cannot create directory ‘dir1/dir2’: No such file or directory`

### grep

`ls -l | grep foo | grep bar` grepをつなぐときはパイプ

`ls -l | grep -v foo` 含まないときは `-v` をつける

### `cd -` で直前にいたディレクトリに戻る

間違ってディレクトリを移動したときなどに便利。

### findコマンドの使い方

`find directory | grep filename` filenameを探す。 findのあとに探索ディレクトリ(例 ./, /home/)、-type f/d などを入れることも可能。自動でワイルドカード、再帰になる。

`find directory -type f | xargs grep -l -s searchword` searchwordを中に含むファイルを探す。-l ファイル名だけを出力　-s エラーメッセージをスキップ

`find dirctory -maxdepth 1 | grep filename` dirctoryから階層1だけ探す。

### negative wildcard
`$ ls -l !(*csv)` csvをファイル名に含まないファイルをlsする

### filenameという文字列を名前に含むファイルを探す
find ./ -type f | grep &lt;filename&gt;

./ :　カレントディレクトリより下で、という意味

grepで正規表現の利用　filename$ するとfilenameで終わるファイル

### stringを本文に含むファイルを探すとき
find ./ -type f | xargs grep -l -s "string"

### locate検索

command | meaning
----|----
`locate aaa` | aaa がpathに入っているものを探す
`locate aaa bbb` | aaa OR bbb
`locate -A aaa bbb`| aaa AND bbb
`locate -b aaa` | basenameのみ検索

### 今いるディレクトリを開く

`nautilus .`

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

### whichコマンドで実行ファイルを探す

```
which python
/home/user/anaconda3/bin/python

which git
/usr/bin/git
```

### full pathを出力

`readlink -f filename.txt`
