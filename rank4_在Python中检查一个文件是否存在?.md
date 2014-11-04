| rank | ▲ | ✰ | vote | url |
|:-:|:-:|:-:|:-:|:-:|
|   4  |  1262 | 284 | 923 | [url](http://stackoverflow.com/questions/82831/check-if-a-file-exists-using-python) |

## Question:在Python中检查一个文件是否存在?

在不用`try:`语句的情况下怎么检查一个文件是否存在?

## Answer:

可以这么用:

```python
import os.path
os.path.isfile(fname)
```

检查你的文件是否存在
