| rank | ▲ | ✰ | vote | url |
|:-:|:-:|:-:|:-:|:-:|
|  44  |  455 | 162 | 534 | [url](http://stackoverflow.com/questions/11241523/why-does-python-code-run-faster-in-a-function) |

***

## 为什么代码在一个函数里运行的更快?

```python
def main():
    for i in xrange(10**8):
        pass
main()
```

在Python中运行速度:

```python
real    0m1.841s
user    0m1.828s
sys     0m0.012s
```

然而不把它放在函数里:

```python
for i in xrange(10**8):
    pass
```

它比上一个运行时间更长

```python
real    0m4.543s
user    0m4.524s
sys     0m0.012s
```

为啥会这样?

注:这个计时是用LinuxBASH中的时间函数.

***

你或许想问为什么存取一个本地变量比全局变量要快.这有关于CPython的实现细节.

记住CPython解析器运行的是被编译过的字节编码(bytecode).当一个函数被编译后,局部变量被存储在了固定大小的数组(不是一个`dict`),而变量名赋值给了索引.这就是为什么你不能动态的为一个函数添加局部变量.检查一个局部变量就好像是一个指针去查找列表,对于在`PyObject`上的引用计数的增长是微不足道的.

相反的在查找全局变量(`LOAD_GLOBAL`)时,涉及到的是一个实实在在的`dict`的哈希查找.顺便说一句,这就是为什么当你想要一个全局变量你必须要在前面加上`global i`:如果你在一个区域内指定一个变量,编译器就会建立一个`STORE_FAST`的入口,除非你不让它那么做.

在说一句,全局查找速度也不慢.真正拖慢速度的是像`foo.bar`这样的属性查找!

***

在一个函数里,字节码是这样的:

```python
  2           0 SETUP_LOOP              20 (to 23)
              3 LOAD_GLOBAL              0 (xrange)
              6 LOAD_CONST               3 (100000000)
              9 CALL_FUNCTION            1
             12 GET_ITER
        >>   13 FOR_ITER                 6 (to 22)
             16 STORE_FAST               0 (i)

  3          19 JUMP_ABSOLUTE           13
        >>   22 POP_BLOCK
        >>   23 LOAD_CONST               0 (None)
             26 RETURN_VALUE
```

如果在全局,字节码:

```python
  1           0 SETUP_LOOP              20 (to 23)
              3 LOAD_NAME                0 (xrange)
              6 LOAD_CONST               3 (100000000)
              9 CALL_FUNCTION            1
             12 GET_ITER
        >>   13 FOR_ITER                 6 (to 22)
             16 STORE_NAME               1 (i)

  2          19 JUMP_ABSOLUTE           13
        >>   22 POP_BLOCK
        >>   23 LOAD_CONST               2 (None)
             26 RETURN_VALUE
```

两者的区别是[`STORE_FAST`](http://docs.python.org/library/dis.html#opcode-STORE_FAST)比[`STORE_NAME`](http://docs.python.org/library/dis.html#opcode-STORE_NAME)要快很多.这是因为在函数里`i`是一个局部变量而在全局区域它是一个全局变量.

可以看一下字节编码,用到了[`dis`](http://docs.python.org/library/dis.html)模块.我可以直接解析函数,但是要解析全局代码必须用[`compile`](http://docs.python.org/library/functions.html#compile)内建模块.
