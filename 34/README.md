| rank | ▲ | ✰ | vote | url |
|:-:|:-:|:-:|:-:|:-:|
|  34  |  525 | 50 | 764 | [url](http://stackoverflow.com/questions/1712227/how-to-get-the-size-of-a-list) |

***

## 怎么样获取一个列表的长度?

```python
items = []
items.append("apple")
items.append("orange")
items.append("banana")

# FAKE METHOD::
items.amount()  # 返回 3
```

怎么样做才对?

***

[len](https://docs.python.org/2/library/functions.html#len)函数可以用于Python中许多的类型,包括内建类型和标准库类型.

```python
>>len([1,2,3])
3
```
