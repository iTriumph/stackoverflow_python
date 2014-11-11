| rank | ▲ | ✰ | vote | url |
|:-:|:-:|:-:|:-:|:-:|
|  64 | 365 | 174 | 322 | [url](http://stackoverflow.com/questions/1301346/the-meaning-of-a-single-and-a-double-underscore-before-an-object-name-in-python) |

***

## 在实例名字前单下划线和双下划线的含义

我想刨根问底,这到底是什么意思?解释一下他俩的区别.

***

### 单下划线

在一个类中的方法或属性用单下划线开头就是告诉别的程序这个属性或方法是私有的.然而对于这个名字来说并没有什么特别的.

引自[PEP-8](http://www.python.org/dev/peps/pep-0008/):


>单下划线:"内部使用"的弱指示器.比如,from M import * 将不会引进用但下划线开头的对象.

### 双下划线

来自[Python文档](http://docs.python.org/tutorial/classes.html#private-variables-and-class-local-references):


>任何`__spam`形式(至少两个下划线开头,至多一个下划线结尾)都是代替`_classname__spam`,其中classname是当前类的名字.This mangling is done without regard to the syntactic position of the identifier.所以它能用来定义私有类的实例和类变量,方法,在全局中的变量,甚至是实例中的变量.可以区别不同类的实例.


### 例子

```Python
>>> class MyClass():
...     def __init__(self):
...             self.__superprivate = "Hello"
...             self._semiprivate = ", world!"
...
>>> mc = MyClass()
>>> print mc.__superprivate
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: myClass instance has no attribute '__superprivate'
>>> print mc._semiprivate
, world!
>>> print mc.__dict__
{'_MyClass__superprivate': 'Hello', '_semiprivate': ', world!'}
```

***

`__foo__`:一种约定,Python内部的名字,用来区别其他用户自定义的命名,以防冲突.

`_foo`:一种约定,用来指定变量私有.程序员用来指定私有变量的一种方式.

`__foo`:这个有真正的意义:解析器用`_classname__foo`来代替这个名字,以区别和其他类相同的命名.

在Python中没有其他形式的下划线了.

这种约定方式和类,变量,全局变量等没有区别.
