| rank | ▲ | ✰ | vote | url |
|:-:|:-:|:-:|:-:|:-:|
|  87 | 321 | 127 | 454 | [url](http://stackoverflow.com/questions/3964681/find-all-files-in-directory-with-extension-txt-with-python) |

***

## 找出目录中以.txt为扩展名的文件

***

可以用[`glob`](https://docs.python.org/2/library/glob.html):

```python
from __future__ import print_function
import glob
import os
os.chdir("/mydir")
for file in glob.glob("*.txt"):
    print(file)
```

或者用简单的[`os.listdir`](https://docs.python.org/2/library/os.html#os.listdir):

```python
from __future__ import print_function
import os
for file in os.listdir("/mydir"):
    if file.endswith(".txt"):
        print(file)
```

或者你想递归目录:

```python
from __future__ import print_function
import os
for root, dirs, files in os.walk("/mydir"):
    for file in files:
        if file.endswith(".txt"):
             print(os.path.join(root, file))
```
