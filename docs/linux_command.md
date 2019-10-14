## Linuxコマンドをメモする

### ワイルドカードで"含まない"negative wildcard 
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


### ○○日以内に更新したファイルを探す

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
