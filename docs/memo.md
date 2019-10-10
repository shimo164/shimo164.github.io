### vim で !command の結果が消えずに残っていく

`:clear`　を実行すると消える。


### 文字列をsplitしたいが、デリミタを複数にしたい

```python
string = '\n\n\t\t\t日付\n\t\t\n14月\n15火\n16水\n17木\n18金\n19土\n20日\n'
``` 
というとき、\t,\nをデリミタとしてリストに分割したい。

```python
import re
 
print(re.findall(r"[\w']+", string))
```

https://stackoverflow.com/questions/1059559/split-strings-with-multiple-delimiters
