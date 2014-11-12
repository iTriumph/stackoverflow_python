| rank | ▲ | ✰ | vote | url |
|:-:|:-:|:-:|:-:|:-:|
|  72 | 349 | 49 | 512 | [url](http://stackoverflow.com/questions/1185524/how-to-trim-whitespace-including-tabs) |

***

## 怎么样去除空格(包括tab)?

有什么函数既可以去掉空格也能去掉tab?

***

在两侧的空白:

```python
s = "  \t a string example\t  "
s = s.strip()
```

右边的空白:

```python
s = s.rstrip()
```

左边的空白:

```python
s = s.lstrip()
```

也可以指定字符来去除:

```python
s = s.strip(' \t\n\r')
```

上面的将会去掉在`s`左右的空白,\t,\n和\r字符.
