| rank | ▲ | ✰ | vote | url |
|:-:|:-:|:-:|:-:|:-:|
|  43  |  465 | 86 | 790 | [url](http://stackoverflow.com/questions/663171/is-there-a-way-to-substring-a-string-in-python) |

***

## 获得一个字符串的子串

有什么方法获得一个字符串的字串,比如从一个字符串的第三个字符到最后.

可能是`myString[2:end]`?

***

```python
>>> x = "Hello World!"
>>> x[2:]
'llo World!'
>>> x[:2]
'He'
>>> x[:-2]
'Hello Worl'
>>> x[-2:]
'd!'
>>> x[2:-2]
'llo Worl'
```

上面的概念在Python中叫"slicing"(切片),不止在字符串上有这个方法.在[这里](http://stackoverflow.com/questions/509211/good-primer-for-python-slice-notation)有详细的解释.
