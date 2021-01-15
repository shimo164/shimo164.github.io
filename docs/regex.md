# 否定/肯定・先読み/後読み

## look ahead 先読み

否定/肯定 |意味| Vim | Python, VSCODE
---|---|---|---|---
肯定先読み | fooが後ろにあるbarにマッチ|`barfoo\@=` | `bar(?=foo)` |
否定先読み |  fooが後ろにないbarにマッチ|`barfoo\@!` | `bar(?!foo)` |

## look behind 後読み

| 否定/肯定 | 意味         | Vim      | Python, VSCODE    |
| --------- | -------- | --------- | ------------ |
| 肯定後読み      | fooが前にあるbarにマッチ | `foo\@<=bar` | `(?<=foo)bar` |
| 否定後読み      | fooが前にないbarにマッチ |`foo\@<!bar` | `(?<!foo)bar` |
