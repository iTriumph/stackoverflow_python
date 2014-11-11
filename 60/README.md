| rank | ▲ | ✰ | vote | url |
|:-:|:-:|:-:|:-:|:-:|
|  60 | 383 | 111 | 805 | [url](http://stackoverflow.com/questions/582336/how-can-you-profile-a-python-script) |

***

## 如何测量脚本运行时间?

我在Project Euler发现好多这样的问题,许多其他地方也问怎么测量执行时间.但是有的时候答案有点kludgey-比如,在`__main__`中加入时间代码,所以我想在这里分享一下解决方案.

***

Python自带了一个叫`cProfile`的分析器.它不仅实现了计算整个时间,而且单独计算每个函数运行时间,并且告诉你这个函数被调用多少次,它可以很容易的确定你要优化的值.

你可以在你的代码里或是交互程序里调用,像下面这样:

```python
import cProfile
cProfile.run('foo()')
```

更有用的是,你可以在运行脚本的时候用`cProfile`:

```
python -m cProfile myscript.py
```

为了使用更简单,我做了一个小脚本名字叫`profile.bat`:

```
python -m cProfile %1
```

所以我可以这么调用了:

```
profile euler048.py
```

下面是结果:

```python
1007 function calls in 0.061 CPU seconds

Ordered by: standard name
ncalls  tottime  percall  cumtime  percall filename:lineno(function)
    1    0.000    0.000    0.061    0.061 <string>:1(<module>)
 1000    0.051    0.000    0.051    0.000 euler048.py:2(<lambda>)
    1    0.005    0.005    0.061    0.061 euler048.py:2(<module>)
    1    0.000    0.000    0.061    0.061 {execfile}
    1    0.002    0.002    0.053    0.053 {map}
    1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler objects}
    1    0.000    0.000    0.000    0.000 {range}
    1    0.003    0.003    0.003    0.003 {sum}
```

注:更新一个来自PyCon2013的视频网址:  http://lanyrd.com/2013/pycon/scdywg/
