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
### リンク名
`[Link](url) `

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
col1 | col2
----|----
何か|何か
Pagesだと表にならないときがある| |の数を合わせること
```

col1 | col2
---- | ------
何か|何か
Pagesだと表にならないときがある| \| の数を合わせること

### 注意

- GitHub上での更新と、github.ioの更新はタイムラグがある。jekyllによる更新をしているから？
- GitHub上でのmarkdown表示と、github.ioでは少し異なるときがある。表で|の数合わせなど。



### 参考

https://guides.github.com/features/mastering-markdown/
https://github.github.com/gfm/
