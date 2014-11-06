| rank | ▲ | ✰ | vote | url |
|:-:|:-:|:-:|:-:|:-:|
|   7  |  1112 | 431 | 1201 | [url](http://stackoverflow.com/questions/36932/how-can-i-represent-an-enum-in-python) |

***

## 在Python里如何用枚举类型?

我是一个C#开发者,但是我现在做的工作是关于Python的.

怎么在Python里代替枚举类型呢?

***

PEP435标准里已经把枚举添加到Python3.4版本,在Pypi中也可以向后支持3.3, 3.2, 3.1, 2.7, 2.6, 2.5, 和 2.4版本.

如果想向后兼容`$ pip install enum34`,如果下载`enum`(没有数字)将会是另一个版本.

```python
from enum import Enum
Animal = Enum('Animal', 'ant bee cat dog')
```

或者等价于:

```python
class Animals(Enum):
    ant = 1
    bee = 2
    cat = 3
    dog = 4
```

在更早的版本,下面这种方法来完成枚举:

```python
def enum(**enums):
    return type('Enum', (), enums)
```

像这样来用:

```python
>>> Numbers = enum(ONE=1, TWO=2, THREE='three')
>>> Numbers.ONE
1
>>> Numbers.TWO
2
>>> Numbers.THREE
'three'
```

也很容易支持自动计数,像下面这样:

```python
def enum(*sequential, **named):
    enums = dict(zip(sequential, range(len(sequential))), **named)
    return type('Enum', (), enums)
```

这样用:

```python
>>> Numbers = enum('ZERO', 'ONE', 'TWO')
>>> Numbers.ZERO
0
>>> Numbers.ONE
1
```

如果要把值转换为名字可以加入下面的方法:

```python
def enum(*sequential, **named):
    enums = dict(zip(sequential, range(len(sequential))), **named)
    reverse = dict((value, key) for key, value in enums.iteritems())
    enums['reverse_mapping'] = reverse
    return type('Enum', (), enums)
```

这样会覆盖名字下的所有东西,但是对于枚举的输出很有用.如果转换的值不存在就会抛出`KeyError`异常.用前面的例子:

```python
>>> Numbers.reverse_mapping['three']
'THREE'
```
