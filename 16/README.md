| rank | ▲ | ✰ | vote | url |
|:-:|:-:|:-:|:-:|:-:|
|  16  |  789 | 431 | 938 | [url](http://stackoverflow.com/questions/986006/how-do-i-pass-a-variable-by-reference) |

***

## 怎么用引用来传递一个变量?

Python的文档对参数传递的是值还是引用没有明确说明,下面的代码没有改变值`Original`

```python
class PassByReference:
    def __init__(self):
        self.variable = 'Original'
        self.Change(self.variable)
        print self.variable

    def Change(self, var):
        var = 'Changed'
```

有什么方法能让我同过引用来传递变量吗?

***

参数是通过assignment来传递的.原因是双重的:

1. 传递的参数实际上是一个对象的引用(但是这个引用是通过值传递的)
2. 一些数据类型是可变的,但有一些就不是.

所以:

* 如果传递一个可变对象到一个方法,方法就会获得那个对象的引用,而你也可以随心所欲的改变它了.但是你在方法里重新绑定了这个引用,外部是无法得知的,而当函数完成后,外界的引用依然指向原来的对象.
* 如果你传递一个不可变的对象到一个方法,你仍然不能在外边重新绑定引用,你连改变对象都不可以.

为了弄懂,来几个例子.

### 列表-可变类型

**让我们试着修改当做参数传递给方法的列表**:

```python
def try_to_change_list_contents(the_list):
    print 'got', the_list
    the_list.append('four')
    print 'changed to', the_list

outer_list = ['one', 'two', 'three']

print 'before, outer_list =', outer_list
try_to_change_list_contents(outer_list)
print 'after, outer_list =', outer_list
```

输出:

```python
before, outer_list = ['one', 'two', 'three']
got ['one', 'two', 'three']
changed to ['one', 'two', 'three', 'four']
after, outer_list = ['one', 'two', 'three', 'four']
```

因为传递的参数是`outer_list`的引用,而不是副本,所以我们可以用可变列表的方法来改变它,而且改变同时反馈到了外部.

**现在让我们来看看我们试着改变作为传递参数的引用时到底发生了什么:**

```python
def try_to_change_list_reference(the_list):
    print 'got', the_list
    the_list = ['and', 'we', 'can', 'not', 'lie']
    print 'set to', the_list

outer_list = ['we', 'like', 'proper', 'English']

print 'before, outer_list =', outer_list
try_to_change_list_reference(outer_list)
print 'after, outer_list =', outer_list
```

输出:

```python
before, outer_list = ['we', 'like', 'proper', 'English']
got ['we', 'like', 'proper', 'English']
set to ['and', 'we', 'can', 'not', 'lie']
after, outer_list = ['we', 'like', 'proper', 'English']
```


