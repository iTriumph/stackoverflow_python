| rank | ▲ | ✰ | vote | url |
|:-:|:-:|:-:|:-:|:-:|
|  85 | 325 | 50 | 466 | [url](http://stackoverflow.com/questions/510348/how-can-i-make-a-time-delay-in-python) |

***

## 在Python中加入时延

***

```python
import time
time.sleep(5) # 延时5秒
```

每分钟执行一次的例子:

```python
import time
while True:
    print "This prints once a minute."
    time.sleep(60)  # 延迟一分钟
```
