| rank | ▲ | ✰ | vote | url |
|:-:|:-:|:-:|:-:|:-:|
|  54 | 397 | 83 | 538 | [url](http://stackoverflow.com/questions/279237/import-a-module-from-a-relative-path) |

***

## 如何知道一个对象有一个特定的属性?

有什么方法可以检测一个对象是否有某些属性?比如:

```python
>>> a = SomeClass()
>>> a.someProperty = value
>>> a.property
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: SomeClass instance has no attribute 'property'
```

怎样在使用之前知道`a`是否有`property`这个属性?

***

试试`hasattr()`:

```python
if hasattr(a, 'property'):
    a.property
```

在大多数实际情况下,如果一个属性有很大可能存在,那么就直接调用它或者让它引发异常,或者用`try/except`捕获.这种方法比`hasattr`快.如果这个属性很多情况下不在,或者你不确定,那么用`hasattr`将会比触法异常更快.
