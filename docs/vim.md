もくじ

- [:substituteでクリップボードの中身を使う](#substituteでクリップボードの中身を使う)
- [vimで肯定/否定・先読み/後読み](#vimで肯定否定先読み後読み)
- [インストールしているプラグインを確認する](#インストールしているプラグインを確認する)
- [vimgrepでファイルの中を検索](#vimgrepでファイルの中を検索)
- [non-greedy(非貪欲)マッチ](#non-greedy非貪欲マッチ)
- [連番を作る](#連番を作る)
- [大文字小文字を変換](#大文字小文字を変換)
- [検索した結果を含む行を削除/含まない行を削除](#検索した結果を含む行を削除含まない行を削除)
- [コマンド内で計算](#コマンド内で計算)
- [分割ウィンドウの入れ替え](#分割ウィンドウの入れ替え)
- [.vimrcにvim scriptを書く](#vimrcにvim-scriptを書く)
- [文字数、バイト数、単語数をカウント](#文字数バイト数単語数をカウント)
- [Vim のタブ補間候補でpathの区切りは / だが、 \ でないとエラーになるときがある](#vim-のタブ補間候補でpathの区切りは--だが--でないとエラーになるときがある)

----

### :substituteでクリップボードの中身を使う

クリップボードのコピー内容は レジスタ "+ にあるので、`<C-R>+`で呼び出すことができる。
```
:%s/foo/<C-R>+/g
```
`<C-R>+`を押すと、中身が貼り付けられる。

### vimで肯定/否定・先読み/後読み

name       | regex | example| explanation
-----------|-----|-------|------
肯定先読み | \\@= | ed\\@= |  dが後にあるe
否定先読み | \\@! | ed\\@! |  dが後にないe
肯定後読み | \\@<=| e\\@<=d|  eの後にあるd
否定後読み | \\@<!| e\\@<!d|  eの後にないd

### インストールしているプラグインを確認する

:scriptnames

さらにフィルターする

:filter hoge scriptnames



### vimgrepでファイルの中を検索

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

### non-greedy(非貪欲)マッチ


`.*`は貪欲マッチ

`.\{-}`は非貪欲マッチ


abbbb

aと最初のbをマッチするなら `a.\{-}b`

a と 最後のbをマッチするなら `a.*b`


### 連番を作る

先に、各行に0を作る。
(INSERTモード)`0 Ent Esc 5 .` と入力して、0\r を5回繰り返す。

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


### 大文字小文字を変換

(NORMAL)変換したい文字の上にカーソルおいて、`~`<チルダ>

### 検索した結果を含む行を削除/含まない行を削除

含む行を削除 `:g/abc/d`

含まない行を削除 `:v/abc/d`


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

### Vim のタブ補間候補でpathの区切りは / だが、 \ でないとエラーになるときがある

`copy C:/.../... C:/.../...`

だと（候補には出るのに）動かず、`コマンドの構文が誤っています`と出てエラー。 / を \ に変えるとOK。
NEADTreeも同じでcopyが動かない。moveはOK。
http://takuan93.blog62.fc2.com/blog-entry-86.html
