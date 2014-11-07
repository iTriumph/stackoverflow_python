| rank | ▲ | ✰ | vote | url |
|:-:|:-:|:-:|:-:|:-:|
|  26  |  568 | 149 | 609 | [url](http://stackoverflow.com/questions/6470428/catch-multiple-exceptions-in-one-line-except-block) |

***

## 类里的静态变量

有可能在python中类有静态变量或方法?用什么语法实现?

***

变量是在类定义时声明的,不是在类方法或静态变量中:

```python
>>> class MyClass:
...     i = 3
...
>>> MyClass.i
3
```

上面的"i"变量是类级别的,所以它是和所有实体级的"i"变量是不一样的,你可以:

```python
>>> m = MyClass()
>>> m.i = 4
>>> MyClass.i, m.i
>>> (3, 4)
```

这与C++和Java不一样,但是和C#相同,那就是静态成员不能被实例所引用.

看一下[Python教程中关于类和类对象的主题](https://docs.python.org/2/tutorial/classes.html#class-objects)

在这里李四已经回答了[静态方法](http://web.archive.org/web/20090214211613/http://pyref.infogami.com/staticmethod),官方文档[内建函数](https://docs.python.org/2/library/functions.html#staticmethod)中也提到了.

```python
class C:
    @staticmethod
    def f(arg1, arg2, ...): ...
```

