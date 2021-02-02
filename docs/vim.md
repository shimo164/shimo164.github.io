**VIMのまとめ**

- [基本的なこと](#基本的なこと)
  - [移動](#移動)
  - [1文字を変更](#1文字を変更)
  - [検索した結果を含む行を削除/含まない行を削除](#検索した結果を含む行を削除含まない行を削除)
  - [分割ウィンドウの入れ替え](#分割ウィンドウの入れ替え)
  - [:substituteでクリップボードの中身を使う](#substituteでクリップボードの中身を使う)
  - [n回インデントするにはビジュアルモードで囲って`n>`とする](#n回インデントするにはビジュアルモードで囲ってnとする)
  - [diffを見る。縦に開きたい](#diffを見る縦に開きたい)
  - [バッファを消す](#バッファを消す)
  - [registerを消す](#registerを消す)
  - [vimのmap](#vimのmap)
  - [VIMでコマンドプロンプトを実行する](#vimでコマンドプロンプトを実行する)
  - [:source ~/.vimrcでファイル読み込み](#source-vimrcでファイル読み込み)
  - [キーワードを元にファイルを検索](#キーワードを元にファイルを検索)
- [正規表現](#正規表現)
  - [non-greedy(非貪欲)マッチ](#non-greedy非貪欲マッチ)
  - [vimで肯定/否定・先読み/後読み](#vimで肯定否定先読み後読み)
  - [検索でregexを使うときは \ をつかう](#検索でregexを使うときは--をつかう)
- [Tips](#tips)
  - [vim検索で、/ でなくて ; を使うと、/ のエスケープがいらなくなる](#vim検索で-でなくて--を使うと-のエスケープがいらなくなる)
  - [コマンド内で計算](#コマンド内で計算)
  - [文字数、バイト数、単語数をカウント](#文字数バイト数単語数をカウント)
  - [browse oldfiles](#browse-oldfiles)
  - [権限がなくて保管できないときは`:w !sudo tee %`で権限昇格させて保存](#権限がなくて保管できないときはw-sudo-tee-で権限昇格させて保存)
  - [自動補完の候補でを選ぶとき](#自動補完の候補でを選ぶとき)
  - [コマンドを記録](#コマンドを記録)
  - [レジスタ "% はファイル名](#レジスタ--はファイル名)
- [設定系](#設定系)
  - [vimでsyntaxが消えるとき `set ft=html`で直した](#vimでsyntaxが消えるとき-set-fthtmlで直した)
  - [fencのエラー](#fencのエラー)
  - [行番号の色の変更](#行番号の色の変更)
  - [gVIMフォントの設定](#gvimフォントの設定)
  - [静的変数](#静的変数)
  - [コメントから改行時にコメントを追加しない](#コメントから改行時にコメントを追加しない)
  - [vimを終了したときに、バッファ情報を残して次回起動時も使えるようにする](#vimを終了したときにバッファ情報を残して次回起動時も使えるようにする)
- [その他コマンド](#その他コマンド)
  - [連番を作る](#連番を作る)
  - [インストールしているプラグインを確認する](#インストールしているプラグインを確認する)
  - [vimgrepでファイルの中を検索](#vimgrepでファイルの中を検索)
  - [Vimのタブ補間候補でpathの区切りは / だが、 \ でないとエラーになるときがある](#vimのタブ補間候補でpathの区切りは--だが--でないとエラーになるときがある)
- [vim script](#vim-script)
  - [.vimrcにvim scriptを書く](#vimrcにvim-scriptを書く)
  - [vimで !commandの結果が消えずに残っていく](#vimで-commandの結果が消えずに残っていく)
  - [コマンドプロンプトで、文字列を含んだファイルを探すfindstr](#コマンドプロンプトで文字列を含んだファイルを探すfindstr)
- [プラグイン](#プラグイン)
  - [`:scriptnames` プラグイン一覧を表示する](#scriptnames-プラグイン一覧を表示する)
  - [NERDTree](#nerdtree)
- [neocompleteで辞書を設定する](#neocompleteで辞書を設定する)
- [TODO](#todo)


## 基本的なこと

### 移動
ノーマルモード

|入力| 意味|
|---|---|
|hjkl |左下上右|
|.|前の操作を繰り返し|
|G|最後の行へ|
|gg |最初の行へ|
|fy |同じ行の次のy にジャンプ|
|ty| 同じ行の前のyにジャンプ|
|; |ジャンプ繰り返し 前方向|
|, |ジャンプ繰り返し 後ろ方向|
|x|1文字切り取り|
|dd|1行切り取り|
|u| Undo|
|C-r|Redo|
|J |改行をなくす(カーソル行と次の行をつなげる)|
|:sp[lit]|画面を分ける|
|:clo[se]|画面を閉じる
|:onl[y]|その画面だけを残す|
|:!cmd|コマンドプロンプトのcmdを実行|
|:r!cmd| cmdを実行して結果を開いているテキストに追加する|
|% | 対応するカッコに飛ぶ|
|/ | 検索 |
|単語上で * | その単語を検索 |
|30% |ファイルの30％のところへ|
|30gg| 30行目へ|
|H,M,L|画面の上、中央、下に移動|
|小文字p| だとカーソルの後|
|大文字 P| だとカーソルの前|
|小文字o |カーソルの下に行挿入|
|大文字O |カーソルの上に行挿入
|:%y Enter|全選択してコピー |
|:%d|全選択してカット|
|C-w x |ウィンドウ入れ替え|
|C-w < / > |横幅を変更 C-w 50 > とか|
|C-w + / - |高さを変更 |
|:ls |バッファを確認する|
|:bd[N] |N番のバッファを消す|
|:bd% |現在開いているバッファを消す|

### 1文字を変更

NORMALモードで文字の上にカーソルおいてコマンドによって変更

|コマンド|効果|
|:--:|:--|
|~ | 大文字小文字を変換|
|+/- |数字を増やす/減らす|
|r   |一文字を置き換えるモード|

### 検索した結果を含む行を削除/含まない行を削除

含む行を削除 `:g/abc/d`

含まない行を削除 `:v/abc/d`



### 分割ウィンドウの入れ替え
`<C-W> X` 入れ替え

`<C-W> h, j, k,l` で縦横を変更する


### :substituteでクリップボードの中身を使う

クリップボードのコピー内容はレジスタ "+ にあるので、`<C-R>+`で呼び出すことができる。
```
:%s/foo/<C-R>+/g
```
`<C-R>+`を押すと、中身が貼り付けられる。

### n回インデントするにはビジュアルモードで囲って`n>`とする

### diffを見る。縦に開きたい

今開いているファイルと、もうひとつfile2を開く

:vert diffs[plit] <file2>

:diffo[ff] で終了

### バッファを消す
- bd% とすると現在のものだけが消える
- %bdとすると全部消える。保存されていないもの(+)は残る

### registerを消す
* 1つ消すとき。例えばaを消す
`:call setreg("a", [])`

* [まとめて消すとき ](https://stackoverflow.com/questions/19430200/how-to-clear-vim-registers-effectively)
```
let regs=split('abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789/-"', '\zs')
for r in regs
    call setreg(r, [])
endfor
```


### vimのmap

|例|map|
|---|---|
|マップを確認する |`:imap [:vmap, :nmap, :map] <C-v>`|
|マップする |`:imap <C-X> <Esc>:%d<CR>i`|
|マップを消す |`:iunmap <C-V>`|
|インクリメントのマップ元々のC-aを外した | `nnoremap + <C-a>`, `nnoremap - <C-x>`|
|コピーのショートカット |`noremap <C-C> <M-E>"+y`|
|保存をCtrl + S|`noremap <C-S> <M-E>:w<CR>`|
| | |


- マップで `/(スラッシュ)`や`,(コンマ)`、`Shift+Alt+` など組み合わせるコマンドは使えない。`<Space>/`はOK。
- コマンドはgvimのメニューに書いてあるものが使えた

### VIMでコマンドプロンプトを実行する

hoge.pyを開いていて、`$ python hoge.py `を実行するには、VIMコマンドラインで
`:!python %`とする。% は今のファイルという意味。結果を同じファイルに出力するときはrをつけて`r!python %`とする。

### :source ~/.vimrcでファイル読み込み

.vimrcに変更を加えたときに、再読み込みする。
### キーワードを元にファイルを検索

!findstr /s /n "keyword" *.*

- *.* で今いるディレクトリ
- /sで再帰的に見る
- /nで結果の行番号を出力


## 正規表現

:g/pattern/d
マッチした行を消す

:v/pattern/d
マッチしなかった行を消す

### non-greedy(非貪欲)マッチ


`.*`は貪欲マッチ

`.\{-}`は非貪欲マッチ


例えば、`abbbb`に対して、

- aと最初のbをマッチするなら `a.\{-}b`
- aと最後のbをマッチするなら `a.*b`


### vimで肯定/否定・先読み/後読み

name       | regex | example| explanation
-----------|-----|-------|------
肯定先読み | \\@= | ed\\@= |  dが後にあるe
否定先読み | \\@! | ed\\@! |  dが後にないe
肯定後読み | \\@<=| e\\@<=d|  eの後にあるd
否定後読み | \\@<!| e\\@<!d|  eの後にないd

### 検索でregexを使うときは \ をつかう

|||
|--|--|
|\w |何か文字|
|\s |何かスペース|
|\d |何か数字|
|`\{n}` |n文字|

`\d\{n}` →数字n文字(エスケープの `\` が毎回いることに注意)

 \(pattern\) でグループを `\1` で。置換で有効。

## Tips

### vim検索で、/ でなくて ; を使うと、/ のエスケープがいらなくなる

### コマンド内で計算
`<C-R>=1+2<CR>` で3が入力される。

### 文字数、バイト数、単語数をカウント

ビジュアルモードで囲ってから`g C-g` とする。選択部分の文字数、バイト数、単語数、全体の文字数が表示される。

### browse oldfiles

番号を選んで過去開いたファイルにジャンプできる

oldffilesだけだとファイル名だけを調べられる

### 権限がなくて保管できないときは`:w !sudo tee %`で権限昇格させて保存

### 自動補完の候補でを選ぶとき
挿入モードで、cnt-n / cnt-pを押すと候補がでるが候補を選ぶときは、cnt-n/pを連打する方がよい。
矢印上下で選ぶと、C-yで決定、または<CR>だがこれは改行されてしまうので。

### コマンドを記録

-ノーマルモードで`q`で開始する。
-（コピーするときなどは）カーソルを移動させてから
```
qa
yy <CR> P
q
```
とするとaにコマンドを記録できる。`[数字]@a`で[数字]回繰り返すことができる。

### レジスタ "% はファイル名

`"%p` とすると、ファイル名を貼り付けられる。`:"%p`でない。(`:`は不要)


## 設定系

### vimでsyntaxが消えるとき `set ft=html`で直した

### fencのエラー

変換失敗 (上書きするには 'fenc' を空にしてください).とかメッセージが出るとき、`set fenc=utf-8`


### [行番号の色の変更](https://stackoverflow.com/questions/237289/vim-configure-line-number-coloring)


(vim):highlight LineNr ctermfg=grey

(gvim):highlight LineNr guifg=#ccffcc

### gVIMフォントの設定

- gvim Winのフォントを変えた
- MigMIXをダウンロードして設定するといい感じ
  -ダウンロードは公式から。migmix-1m-regular.ttf, migmix-1m-bold.ttf
をコントロールパネル\デスクトップのカスタマイズ\フォントに移動して他のフォントが置かれているところに置く。ダブルクリックでインストールできる。


### 静的変数

保持したいときに使える。
```
  static int a = 0;

  int sales[3]; /*配列の確保 [0][1]][2]までなので注意*/

  sales[0] = 200;
  sales[1] = 400;
  sales[2] = 300;
  int sales2[] = {200, 300, 400}; /*こうやって確保と同時に定義もできる [3]の3を省略している*/
```
### コメントから改行時にコメントを追加しない

例えばcppのファイルで// の行から改行すると、自動的に新しい行にも改行が入ります	。この機能を消すには

:set formatoptions-=ro

とします。rが改行で、oはインサートモードでのoに対してです。

戻すときは+=roとすればよいです。

### vimを終了したときに、バッファ情報を残して次回起動時も使えるようにする

.vimrcに以下を記載
`:set viminfo='100,<50,s10,h,rA:,rB:,%`

%nとするとn個のバッファを残せる

**ファイル指定しないで**vimを再起動すると、バッファがviminfoから読み込まれる

■mingw includeのPathを
http://d.hatena.ne.jp/osyo-manga/20131219/1387465034
[インクルードディレクトリを設定]
Vimでインクルードディレクトリは 'path' に設定しておくとよいです。

こうしておく事でgfなどでヘッダーファイルを開きたい場合に 'path' を参照してヘッダーファイルを開くことができます。






## その他コマンド
### 連番を作る

■g C-aで

先に、各行に0を作る。
(INSERTモード)`0 Ent Esc 5 .` と入力して、0\rを5回繰り返す。

```
0
0
0
0
0
0
```

2行目から6行目を`<C-v>`矩形選択で囲んで、`g C-a`とすると1ずつ増える。

```
0
1
2
3
4
5
```

■マクロ

- 0 を入力しておいて
- qq yyp<C-+> q
- 100@q

### インストールしているプラグインを確認する

:scriptnames

さらにフィルターする

:filter hoge scriptnames


### vimgrepでファイルの中を検索

`:vimgrep /*/j **/**.* | cw`

/*/jの * が検索語

|cw : QuickFixリストを出力

/j : QuickFixを更新するでけでジャンプしない、の意味

`**/**.*` : 再帰、どのファイルでも

|cwを入力しないでもgrepしたときは自動でQuickFixになるようにできる

```
" .vimrc
augroup QuickFixCmd
  autocmd!
  autocmd QuickFixCmdPost *grep* cwindow
augroup END
```

### Vimのタブ補間候補でpathの区切りは / だが、 \ でないとエラーになるときがある

`copy C:/.../... C:/.../...`

だと（候補には出るのに）動かず、`コマンドの構文が誤っています`と出てエラー。 / を \ に変えるとOK。
NEADTreeも同じでcopyが動かない。moveはOK。
http://takuan93.blog62.fc2.com/blog-entry-86.html


## vim script

### .vimrcにvim scriptを書く

**function**
```vim
fucntion! Test()
  echo "This is test!"
endfunction
```

`:call Test()`として呼び出せる。

**command**
```vim
command! Hoge call Test()
```
`:Hoge`で呼び出せる。

参考:
[Vim scriptとvimrcの正しい書き方＠nagoya.vim #1](https://www.slideshare.net/cohama/vim-script-vimrc-nagoyavim-1)

### vimで !commandの結果が消えずに残っていく

`:clear` を実行すると消える。

### コマンドプロンプトで、文字列を含んだファイルを探すfindstr

`findstr /N /S string *`

* /Nは該当の行番号出力
* /Sはサブフォルダも再帰的
* string検索する文字列
* \* カレントディレクトリにあるファイルpath。*でよい。別のディレクトリを指定できる。
* AND検索: パイプしてfindstrを追加する | findstr string2
* OR検索: スペースで区切るfindstr string string2 *

## プラグイン

### `:scriptnames` プラグイン一覧を表示する

### NERDTree
* 起動は:NERDTree , :qで閉じる
* マップしたほうがよいNERDTreeToggle
* ファイルを選択してmでmenuを開ける
* ファイルの再読込rでディレクトリ、Rで全体。名前を変えたときは自動で反映されないので手動で。
* 拡張子によって表示色を変えるvimrcに書くコマンドがある。



## neocompleteで辞書を設定する

XXX.dictを作って
/.vim/dicts/
に入れた。
.dictは単に用語が1列に列挙されていればよい。

使うときは、
:NeoCompleteDictionaryMakeCache
を入力すること。
対応した言語拡張子になっていることで機能する。

_vimrcで 以下のようにした。rubyにしているのは参考にしたサイトのものを使ったので。
https://henry.animeo.jp/blog/2013/08/24/ruby-dict/
https://ktrysmt.github.io/blog/use-php-dict-by-neocomplete/

```
" Define dictionary.
let $DOTVIM = $VIM . '/.vim'
let g:neocomplete#sources#dictionary#dictionaries = {
    \ 'default' : '',
    \ 'vimshell' : $HOME.'/.vimshell_hist',
    \ 'scheme' : $HOME.'/.gosh_completions',
    \ 'ruby' : $DOTVIM.'/dicts/ruby.dict',
        \ }
```
## TODO

* vim bufferの番号を振り直せるようにしたい
バッファの番号がだんだん大きくなってしまうと鬱なので、小さいものにしたい
変更はできない？

* ファイルの更新時間を見る --> !dir % で見ている

* Pythonでコメントアウトが強制的に行の最初になるが、スペースでない書き始めの直前にコメント#を入れたい
＞プラグインにはあったはず。caw

- neocompleteは入っていていちおう使えるがdeopleteが使えない
deopleteはインストールできたが動いていない？
help deopleteは見れる。が、Python3が入っていない？
+python3/dyn
はOK、
echo has('python3') も1がでてOKだがだめ。
:set pythonthreedll= も存在するpython36.dllにしているが・・・
