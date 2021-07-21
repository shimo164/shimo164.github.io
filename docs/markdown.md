- [リンクのいい感じの書き方](#リンクのいい感じの書き方)
- [見出しでは使えない日本語文字がある](#見出しでは使えない日本語文字がある)
- [Table columnの幅](#table-columnの幅)
  - [img widthで幅設定する](#img-widthで幅設定する)
  - [`&nbsp;`でスペースを入れる](#nbspでスペースを入れる)
- [マークダウン記法をまとめる](#マークダウン記法をまとめる)
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

# 見出しでは使えない日本語文字がある
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

# Table columnの幅
複数行の最大幅で決まる。下の場合、左の列はName、右の列はLong explanationが最大幅。

| Name | Value            |
| ---- | ---------------- |
| a    | Long explanation |
| b    | Long explanation |
| c    | Long explanation |

## img widthで幅設定する
一番下の行に、空白行を挿入して、imgをwidthを指定する
- **空白行が下に付くのは避けられない**
- img幅を文字の最大幅よりも小さく指定しても効果がない

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

左右とも200pxに設定。
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

左400px、右200pxに設定。
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
- 上記のimg幅指定と違って空白行はない
- mdテキスト上での見た目の幅が大きくなるのがデメリット

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

# マークダウン記法をまとめる
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
URLを平文に書くとリンクになる https://example.com

リンクに文字列を与える
`[Link](url) `

ページ内リンク
`[Link文](./#見出し)`
* 見出しのスペースはハイフンに、大文字は小文字になる

absolute link
`[link](file:///home/user/.../)`

[link](_site/aws-link.md)

[link](/../_site/aws-link.md)


TODO ローカルのフルパスでは記述できない？

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
- GitHub上での更新と、github.ioの更新はタイムラグがある。jekyllによる更新をしている
- GitHub上でのmarkdown表示と、github.ioでは異なるときがある。表で|の数合わせなど。
  - 表の開始時に空白行を入れること(github.ioでは必要)
  - Githubでの表示とVSCodeのmarkdown previewは同じ

### 参考
https://guides.github.com/features/mastering-markdown/
https://github.github.com/gfm/
