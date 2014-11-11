| rank | ▲ | ✰ | vote | url |
|:-:|:-:|:-:|:-:|:-:|
|  45  |  441| 233 | 760 | [url](http://stackoverflow.com/questions/11241523/why-does-python-code-run-faster-in-a-function) |

***

## 合并列表中的列表

可能重复的问题:

* [Flattening a shallow list in Python](http://stackoverflow.com/questions/406121/flattening-a-shallow-list-in-python)
* [Comprehension for flattening a sequence of sequences?](http://stackoverflow.com/questions/457215/comprehension-for-flattening-a-sequence-of-sequences)

我想是不是有更好的方法

我可以用一个循环来做,但是除了这样做还有什么更cool的用一行来做的方法?我用`reduce`来做,但是我得到了个错误.

代码:

```python
l = [[1,2,3],[4,5,6], [7], [8,9]]
reduce(lambda x,y: x.extend(y),l)
```

错误信息:

```python
Traceback (most recent call last): File "", line 1, in File "", line 1, in AttributeError: 'NoneType' object has no attribute 'extend'
```

***

`[item for sublist in l for item in sublist]`到目前为止来看是最快的方法.

可以用`timeit`模块验证一下:

```python
$ python -mtimeit -s'l=[[1,2,3],[4,5,6], [7], [8,9]]*99' '[item for sublist in l for item in sublist]'
10000 loops, best of 3: 143 usec per loop
$ python -mtimeit -s'l=[[1,2,3],[4,5,6], [7], [8,9]]*99' 'sum(l, [])'
1000 loops, best of 3: 969 usec per loop
$ python -mtimeit -s'l=[[1,2,3],[4,5,6], [7], [8,9]]*99' 'reduce(lambda x,y: x+y,l)'
1000 loops, best of 3: 1.1 msec per loop
```

解释:

当有L个子串的时候用`+`(即`sum`)的时间复杂度是`O(L**2)`--每次迭代的时候作为中间结果的列表的长度就会越来越长,而且前一个中间结果的所有项都会再拷贝一遍给下一个中间结果.所以当你的列表l含有L个字串:l列表的第一项需要拷贝L-1次,而第二项要拷贝L-2次,以此类推;所以总数为`I * (L**2)/2`.

列表推导式(list comprehension)只是生成一个列表,每次运行只拷贝一次(从开始的地方拷贝到最终结果).
