- [リンクのいい感じの書き方](#リンクのいい感じの書き方)
- [md見出しがhtmlでどう変換されるか](#md見出しがhtmlでどう変換されるか)
- [Width of Table column in Markdown](#width-of-table-column-in-markdown)
  - [img widthで幅設定する](#img-widthで幅設定する)
  - [`&nbsp;`でスペースを入れる](#nbspでスペースを入れる)
- [マークダウンのチートシート](#マークダウンのチートシート)
    - [リンク](#リンク)
    - [画像](#画像)
    - [エスケープ](#エスケープ)
    - [コード](#コード)
    - [引用](#引用)
    - [表](#表)
    - [注意](#注意)
    - [参考](#参考)

# リンクのいい感じの書き方
[こちら][ref1]で紹介されているように、長いURLを別のところに書いてrefで参照することで、スッキリした記述にできる。

[ref1]:https://miraium.com/markdown-summary-for-vscode/

画像埋め込みの場合は、！マークが必要です。

![][ref2]

[ref2]: /image/python.png

```
[こちら][ref1]で紹介されているように、長いURLを別のところに書いてrefで参照することで、スッキリした記述にできる。

[ref1]:https://miraium.com/markdown-summary-for-vscode/

画像埋め込みの場合は、！マークが必要です。

![][ref2]

[ref2]: /image/python.png
```

# md見出しがhtmlでどう変換されるか
ここがまとめている
* http://tbpgr.hatenablog.com/entry/20140125/1390659050
* https://so-zou.jp/web-app/text/fullwidth-halfwidth/

```
英大文字=>英小文字
半角スペース=>半角ハイフン
ドット=>削除
シャープ=>削除
丸括弧=>削除
カンマ=>削除
コロン=>削除

変換されないのは
長音ー
半角ハイフン-
半角アンダースコア_
全角アンダーライン＿
全角スペース
```
===

```py
### TODO code中の###はリンクにならないが、判定されてしまうのを回避

#!/usr/bin/env python
# -*- coding: utf-8 -*-
"""
In .md, make contens list from header ###.

Usage:
    $ python md_make_contents.py readfile.txt

Output:
    header_content.txt
    In this txt, contents with md format is written.
"""
import re
import sys

file = sys.argv[1]
total_line = ""

sharp = "###"

def format_md_header(header_raw):
    h = header_raw
    h = h.lower()
    h = re.sub(" ", "-", h)
    ma = re.findall("[a-zA-Z0-9_＿ー　ぁ-んァ-ンぁ-んァ-ンｱ-ﾝ一-龥]", h)

    h_md = ""
    for s in ma:
        h_md += s

    return h_md

with open(file) as f:
    for line in f:
        line = re.sub("\n", "", line)
        if re.match(sharp, line):
            line = line.lstrip(sharp + " ")
            h_md = format_md_header(line)
            h_md = "* [" + line + "](#" + h_md + ")"
            total_line += h_md + "\n"
    fw = open("header_content.txt", "w")
    fw.write(total_line)
```

# Width of Table column in Markdown
複数行の最大幅で決まる。下の場合、左の列はName、右の列はLong explanationが最大幅。

| Name | Value            |
| ---- | ---------------- |
| a    | Long explanation |
| b    | Long explanation |
| c    | Long explanation |

## img widthで幅設定する

下の行に、空白のimgをwidth指定で入れる。
- 空白行が下に付いていることに注意
- 幅を大きくすることはできるが、小さくはできない

```
| Name             | Value            |
| ---------------- | ---------------- |
| a                | Long explanation |
| b                | Long explanation |
| c                | Long explanation |
| <img width=100/> |                  |
```
| Name             | Value            |
| ---------------- | ---------------- |
| a                | Long explanation |
| b                | Long explanation |
| c                | Long explanation |
| <img width=100/> |                  |

左右とも200pxにする。
```
| Name             | Value            |
| ---------------- | ---------------- |
| a                | Long explanation |
| b                | Long explanation |
| c                | Long explanation |
| <img width=200/> | <img width=200/> |
```

| Name             | Value            |
| ---------------- | ---------------- |
| a                | Long explanation |
| b                | Long explanation |
| c                | Long explanation |
| <img width=200/> | <img width=200/> |

左400px,右200pxにした。
- 一行開けると、Markdownでの最下部の線がなくなる。
  - 空白行があるのは変わらない

```
| Name | Value            |
| ---- | ---------------- |
| a    | Long explanation |
| b    | Long explanation |
| c    | Long explanation |

| <img width=400/> | <img width=200/> |
```

| Name | Value            |
| ---- | ---------------- |
| a    | Long explanation |
| b    | Long explanation |
| c    | Long explanation |

| <img width=400/> | <img width=200/> |

## `&nbsp;`でスペースを入れる
- mdテキスト上での幅が大きくなるのがデメリット
- imgと違って空白行ができない
```
| Name &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; | Value            |
| ----------------------------------------------------------- | ---------------- |
| a                                                           | Long explanation |
| b                                                           | Long explanation |
| c                                                           | Long explanation |
```

| Name &nbsp; &nbsp; &nbsp; &nbsp;&nbsp; &nbsp; &nbsp; &nbsp; | Value            |
| ----------------------------------------------------------- | ---------------- |
| a                                                           | Long explanation |
| b                                                           | Long explanation |
| c                                                           | Long explanation |



# マークダウンのチートシート
```markdown

# Header 1
## Header 2
### Header 3
- Bulleted
- List

1. Numbered
2. List

**Bold**
_Italic_
`Code`
※スペースを入れないこと
```
### リンク
ふつうに書くとリンク　https://example.com

リンクに文字列を与える
`[Link](url) `

ページ内リンク
`[文字列](./#見出し)`
* 見出しのスペースはハイフンに、大文字は小文字になる

absolute link
`[link](file:///home/user/.../)`

TODO　ローカルのフルパスでは記述できなくなった

### 画像
`![Image](src)`

### エスケープ
\\
バックスラッシュでエスケープ

### コード
```
バッククォート3つで囲むと、コード
```

\```python

\```

などと指定してシンタックスハイライトをつけられる。

```python
def test(n):
    a = "string"
    return 0
```

### 引用
`> 引用`

> 引用

### 表
```
| col1                            | col2 |
| ------------------------------- | ---- |
| 何か                            | 何か |
| Pagesだと表にならないときがある |      | の数を合わせること |
```

| col1                            | col2                  |
| ------------------------------- | --------------------- |
| 何か                            | 何か                  |
| Pagesだと表にならないときがある | \| の数を合わせること |

### 注意
- GitHub上での更新と、github.ioの更新はタイムラグがある。jekyllによる更新をしているから？
- GitHub上でのmarkdown表示と、github.ioでは少し異なるときがある。表で|の数合わせなど。

### 参考
https://guides.github.com/features/mastering-markdown/
https://github.github.com/gfm/
