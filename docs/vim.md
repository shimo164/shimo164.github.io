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

(NORMALモード)数字の上にカーソルおいて、`~`

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
