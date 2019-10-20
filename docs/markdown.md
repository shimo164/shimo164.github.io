
# 数字	全角	０ １ ２ ３ ４ ５ ６ ７ ８ ９
# 半角	0 1 2 3 4 5 6 7 8 9
# 英字	全角	
# Ａ Ｂ Ｃ Ｄ Ｅ Ｆ Ｇ Ｈ Ｉ Ｊ Ｋ Ｌ Ｍ Ｎ Ｏ Ｐ Ｑ Ｒ Ｓ Ｔ Ｕ Ｖ Ｗ Ｘ Ｙ Ｚ

# ａ ｂ ｃ ｄ ｅ ｆ ｇ ｈ ｉ ｊ ｋ ｌ ｍ ｎ ｏ ｐ ｑ ｒ ｓ ｔ ｕ ｖ ｗ ｘ ｙ ｚ

半角	
# A B C D E F G H I J K L M N O P Q R S T U V W X Y Z

# a b c d e f g h i j k l m n o p q r s t u v w x y z

カナ	全角	
# ア イ ウ エ オ カ キ ク ケ コ サ シ ス セ ソ タ チ ツ テ ト ナ ニ ヌ ネ ノ ハ ヒ フ ヘ ホ マ ミ ム メ モ ヤ ユ ヨ ラ リ ル レ ロ ワ ヲ ン

# ヴ ガ ギ グ ゲ ゴ ザ ジ ズ ゼ ゾ ダ ヂ ヅ デ ド バ ビ ブ ベ ボ パ ピ プ ペ ポ ァ ィ ゥ ェ ォ ャ ュ ョ ッ

# aーb◌゙c◌゚d 、e 。f ・g 「h 」

半角	
# ｱ ｲ ｳ ｴ ｵ ｶ ｷ ｸ ｹ ｺ ｻ ｼ ｽ ｾ ｿ ﾀ ﾁ ﾂ ﾃ ﾄ ﾅ ﾆ ﾇ ﾈ ﾉ ﾊ ﾋ ﾌ ﾍ ﾎ ﾏ ﾐ ﾑ ﾒ ﾓ ﾔ ﾕ ﾖ ﾗ ﾘ ﾙ ﾚ ﾛ ﾜ ｦ ﾝ

# ｳﾞ ｶﾞ ｷﾞ ｸﾞ ｹﾞ ｺﾞ ｻﾞ ｼﾞ ｽﾞ ｾﾞ ｿﾞ ﾀﾞ ﾁﾞ ﾂﾞ ﾃﾞ ﾄﾞ ﾊﾞ ﾋﾞ ﾌﾞ ﾍﾞ ﾎﾞ ﾊﾟ ﾋﾟ ﾌﾟ ﾍﾟ ﾎﾟ ｧ ｨ ｩ ｪ ｫ ｬ ｭ ｮ ｯ

# aｰb ﾞc ﾟd ､e ｡f ･g ｢h ｣

# ASCII記号	全角	a！b ＂c ＃d ＄e ％f ＆g ＇h （a ）b ＊c ＋d ，e －f ．g ／h ：a ；b ＜c ＝d ＞ e？ f＠ g［ h＼a ］b ＾c ＿d ｀e ｛f ｜g ｝h ～
# 半角	a!b "c #d $e %f &g 'h (a )b * c +d ,e - f.g / a: b; c< d= e> f? g@a \[b \c ] d^ e_f \` g { a| f} b~e
# 非ASCII記号	全角	a｟ b｠ c￠ d￡ e￢ f￣ a￤ b￥c ￦d │e ←f ↑a →b ↓c■d ○e
# 半角	⦅ ⦆ ¢ £ ¬ ¯ ¦ ¥ ₩ ￨ ￩ ￪ ￫ ￬ ￭ ￮


# マークダウンのチートシート■ＳｓSs（test）(t)

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
