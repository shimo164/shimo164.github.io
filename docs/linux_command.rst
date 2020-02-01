.. contents::

grep
####

``ls -l | grep foo | grep bar`` grepをつなぐときはパイプ

``ls -l | grep -v foo`` 含まないときは ``-v`` をつける

``cd -`` で直前にいたディレクトリに戻る
####################################

間違ってホームディレクトリに行ってしまったときに便利。

findコマンドの使い方
#####################
``find | grep filename`` でfilenameを探す。 findのあとに探索ディレクトリ(例 /home/)、-type f/d などを入れることも可能

``find ./ -type f | grep <filename>`` ファイルfilenameを探す。自動でワイルドカード、再帰。

``find ./ -type f | xargs grep -l -s searchword`` searchwordを中に含むファイルを探す。-l ファイル名だけを出力　-s エラーメッセージをスキップ


negative wildcard
#################
``$ ls -l !(*csv)`` csvをファイル名に含まないファイルをlsする

filenameという文字列を名前に含むファイルを探す
##############################################

find ./ -type f | grep &lt;filename&gt;

./ :　カレントディレクトリより下で、という意味

grepで正規表現の利用　filename$ するとfilenameで終わるファイル

stringを本文に含むファイルを探すとき
####################################

find ./ -type f | xargs grep -l -s "string"

locate検索
##########

.. csv-table::
  :header: command, meaning 
  
  locate aaa, aaa がpathに入っているものを探す
  locate aaa bbb, aaa OR bbb
  locate -A aaa bbb, aaa AND bbb
  locate -b aaa, basenameのみ検索

今いるディレクトリを開く
########################

``nautilus .``


XX日以内に更新したファイルを探す
##################################

``-mtime X`` : x 日前

``-mtime -X`` : x 日前*以降*

``-mtime 0`` : 0-24時間前

``-mtime 1`` : 24-48時間前

``-mtime -1`` : 0-24時間前

``find . -mtime -12 -name \*.py``

``find . -mtime -12 |grep "\.py$"``

12日以内に更新した.pyファイルを探す(どちらも使えるが、.pyへのマッチが違う)

whichコマンドで実行ファイルを探す
#################################

```
which python
/home/user/anaconda3/bin/python

which git
/usr/bin/git
```

full pathを出力
################

``readlink -f filename.txt``
