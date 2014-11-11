| rank | ▲ | ✰ | vote | url |
|:-:|:-:|:-:|:-:|:-:|
|  48  | 432 | 75 | 793 | [url](http://stackoverflow.com/questions/306400/how-do-i-randomly-select-an-item-from-a-list-using-python) |

***

## 在列表中随机取一个元素

例如我有如下列表:

```python
foo = ['a', 'b', 'c', 'd', 'e']
```

从列表中随机取一个元素最好的方法是什么?

***

```python
import random

foo = ['a', 'b', 'c', 'd', 'e']
print(random.choice(foo))
```
