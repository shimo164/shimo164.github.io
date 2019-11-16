### vimgrep でファイルの中を検索

`:vimgrep /*/j **/**.* | cw`

/*/j の *　が検索語

|cw :　QuickFixリストを出力

/j : QuickFixを更新するでけでジャンプしない、の意味

`**/**.*` : 再帰、どのファイルでも

|cw を入力しないでもgrepしたときは自動でQuickFixになるようにできる

```
" .vimrc
augroup QuickFixCmd
  autocmd!
  autocmd QuickFixCmdPost *grep* cwindow
augroup END
```

### non-greedy 非貪欲マッチ


`.*`は貪欲マッチ

`.\{-}`は非貪欲マッチ


abbbb

aと最初のbをマッチするなら `a.\{-}b`

a と　最後のbをマッチするなら `a.*b`


### 連番を作る。

(INSERTモード)`0 Ent Esc 5.` として0を各行につくる。
```
0
0
0
0
0
0
```
2行目から6行目を`<C-v>`矩形選択で囲んで、`g C-a`とすると1つずつ数字がずれる。

### 大文字小文字を変換

(NORMAL)変換したい文字の上にカーソルおいて、`~`<チルダ>

### 検索した結果を含む行を削除/含まない行を削除

含む行を削除 `:g/abc/d`

含まない行を削除 `:v/abc/d`

### vimgrepで検索

`vimgrep /word/j **/*.txt |cw`

recursive, .txt でword を検索　cwで表示

### コマンド内で計算
`<C-R>=1+2<CR>` で3が入力される。
  
### 分割ウィンドウの入れ替え
`<C-W> X` 入れ替え

`<C-W> h, j, k,l` で縦横を変更する


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
[Vim script と vimrc の正しい書き方＠nagoya.vim #1](https://www.slideshare.net/cohama/vim-script-vimrc-nagoyavim-1)


### 文字数、バイト数、単語数をカウント

ビジュアルモードで囲ってから`g C-g` とする。選択部分の文字数、バイト数、単語数、全体の文字数が表示される。

### Vim のタブ補間候補でディレクトリ構造は / で区切られるが、 \ にしないとエラーになるときがある。

copy C:/.../... C:/.../... 

だと（候補には出るのに）動かず、`コマンドの構文が誤っています`と出てエラー。 / を \ に変えるとOK。
NEADTreeも同じでcopyが動かない。moveはOK。
http://takuan93.blog62.fc2.com/blog-entry-86.html
