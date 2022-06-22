# grep

| オプション | grep 出力                  |
| :--------: | -------------------------- |
|     -r     | 配下ディレクトリを対象に   |
|     -v     | マッチしないものを出力     |
|     -n     | 行番号                     |
|     -H     | ファイル名あり             |
|     -h     | ファイル名なし             |
|     -l     | ファイル名のみ             |
|     -L     | マッチしないファイル名     |
|     -o     | 検索にマッチした部分だけ   |
|     -q     | 結果を出力しない           |
|     -s     | --no-messages エラー無出力 |

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
