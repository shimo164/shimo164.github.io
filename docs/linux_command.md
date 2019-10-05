## Linuxコマンドをメモする

### 検索する

`locate aaa` aaa がpathに入っているものを探す

`locate aaa bbb` OR 検索

`locate -A aaa bbb` AND検索

`locate -b aaa` basenameのみ検索


### 今いるディレクトリを開く

`nautilus .`


### ○○日以内に更新したファイルを探す

`$ find . -mtime -12 -name \*.py`

`$ find . -mtime -12 |grep "\.py$"`

12日以内に更新した.pyファイルを探す(どちらも使えるが、.pyへのマッチが違う)
