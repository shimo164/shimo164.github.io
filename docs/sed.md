# sed

https://linuxjm.osdn.jp/html/GNU_sed/man1/sed.1.html
https://hydrocul.github.io/wiki/commands/sed.html

詳しいGNUのマニュアル:
https://www.gnu.org/software/sed/manual/sed.html

## 書式

```
sed [OPTION]... {script-only-if-no-other-script} [input-file]...
```

- sed はストリームエディター
- 入力ストリーム (ファイルまたはパイプラインからの入力) に対して基本的なテキスト変換を行う
- sed は 他のエディターと似ている面もあるが、
  - sed は入力に対して 1 パスだけで動作するので、より効率的
  - sed はパイプラインのテキストに対してフィルター動作を行うことができる


```sh
置換は 's/パターン/置き換え/'
- s コマンド(substitute)
- パターン部分は正規表現が使える。

# echoを入力ストリームとして、パイプする例
echo abcdab
abcdab

# 一回だけ置換
echo abcdab | sed 's/a/x/'
xbcdab

# gを使うとパターンにマッチした全部が置換
echo abcdab | sed 's/a/x/g'
xbcdxb

# ダブルクォートでも同じ
echo abcdab | sed "s/a/x/"
xbcdab

# パターン部分に正規表現はオプションなしで使える
# [bc]で、bまたはc全部
echo abcdab | sed 's/[bc]/x/g'
axxdax

# ファイルを入力ストリームにする
file.txtというファイルがあるとする。

abcdab
abcdab
abcdab

# ファイルの全行に対象
sed 's/[bc]/x/g' file.txt
axxdax
axxdax
axxdax

# 別ファイルに保存
sed 's/[bc]/x/g' file.txt -> file2.txt

# 2行目だけを対象に
sed '2s/[bc]/x/g' file.txt

# 1,2行目だけを対象に
sed '1,2s/[bc]/x/g' file.txt

# 変数を上書きする方法
S=abcdab
S=$(echo $S | sed "s/a/x/g")

# \ でエスケープして/ を置換する
echo //path// | sed  's/\/\//\//g'

# 区切り文字を他の記号(ここでは%)にすれば/ をエスケープしなくてよい
echo //path// | sed  's%//%/%g'

# sedの中に変数を使うときはシングルクォートで囲む
# その1
var=[bc]
sed 's/'$var'/x/g' file.txt

# その2
var=1,2
sed ''$var's/[bc]/x/g' file.txt

```

## オプション
```sh
-E, -r, --regexp-extended 拡張正規表現を使う

-n, --quiet, --silent
パターンスペースの自動出力を抑制します
echo  abcdab | sed -n 's/[bc]/x/g'
<出力なし>


--debug
プログラム実行状況を表示します

echo  abcdab | sed --debug 's/[bc]/x/g'
SED PROGRAM:
  s/[bc]/x/g
INPUT:   'STDIN' line 1
PATTERN: abcdab
COMMAND: s/[bc]/x/g
MATCHED REGEX REGISTERS
  regex[0] = 1-2 'b'
PATTERN: axxdax
END-OF-CYCLE:
axxdax

-e script, --expression=script
実行するコマンドとして script を追加します

通常の書き方と同じ。-eは省略できる、上記では省略している。

-f script-file, --file=script-file
実行するコマンドとして script-file の内容を追加します

echo 's/aa/bb/' > script

cat script
s/aa/bb/

として
echo  abcdaabb | sed -f script
abcdbbbb

--follow-symlinks
インプレース処理においてシンボリックリンクをたどります

???
https://unix.stackexchange.com/questions/192012/how-do-i-prevent-sed-i-from-destroying-symlinks

-i[SUFFIX], --in-place[=SUFFIX]
ファイルをインプレース処理で編集します (SUFFIX 指定時はバックアップを取ります)

Macでは -i "" とする。
sed -i 's/aaa/bbb/g' file

SUFFIX部分に文字を入れると、sed -i で編集される前のバックアップファイルがfileSUFFIXという名前で作られる。

-l N, --line-length=N
`l' コマンドの出力行を折り返す長さを指定します

???

--posix
全ての GNU 拡張を無効にします
-E, -r, --regexp-extended

スクリプトで拡張正規表現を使用します (-E 利用は POSIX 互換性のため)

-s, --separate
複数の入力ファイルを一続きのストリームとして扱わずに個別のファイルとして扱います

defaultでは一続きの扱いなのを個別にする。
- range addresses (such as ‘/abc/,/def/’) are not allowed to span several files
- line numbers are relative to the start of each file
- $ refers to the last line of each file
- files invoked from the R commands are rewound at the start of each file.

--sandbox
サンドボックス (sandbox) モードで実行します (e/r/w コマンドを無効にします)

???

-u, --unbuffered
入力ファイルからデータをごく少量ずつ取り込み、頻繁に出力バッファを掃き出します (flush)

???

-z, --null-data
NUL 文字で行を分割します

https://blog.cles.jp/item/11585
https://stackoverflow.com/questions/52538158/sed-replacing-newlines-with-z


--help
ヘルプを表示して終了します

--version
バージョン情報を出力して終了します
```
