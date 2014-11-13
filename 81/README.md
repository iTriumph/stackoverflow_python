| rank | ▲ | ✰ | vote | url |
|:-:|:-:|:-:|:-:|:-:|
|  81 | 336 | 74| 573 | [url](http://stackoverflow.com/questions/541390/extracting-extension-from-filename-in-python) |

***

## 去除文件扩展名

***

可以用[os.path.splitext](http://docs.python.org/library/os.path.html#os.path.splitext):

```python
>>> import os
>>> fileName, fileExtension = os.path.splitext('/path/to/somefile.ext')
>>> fileName
'/path/to/somefile'
>>> fileExtension
'.ext'
```
