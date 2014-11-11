| rank | ▲ | ✰ | vote | url |
|:-:|:-:|:-:|:-:|:-:|
|  32  |  527 | 250 | 408 | [url](http://stackoverflow.com/questions/3220404/why-use-pip-over-easy-install) |

***

## 为什么用pip比用easy_install的好?

一个推特写道:

    别用easy_install,除非你想自讨苦吃.那么用pip.

为什么要用`pip`而不是`easy_install`?如果一个作者上传一个损坏的源文件包(比如丢失文件,没有setup.py),那么`pip`和`easy_install`都将失效.除了这些,为什么Python使用者(像上面那个)**强烈**建议用`pip`而不是`easy_install`?

***

来自Lan Bicking的自己对于[`pip`](https://pip.pypa.io/en/1.5.X/other-tools.html#easy-install)的介绍:


pip是对easy_install以下方面进行了改进:

* 所有的包是在安装之前就下载了.所以不可能出现只安装了一部分.
* 在终端上的输出更加友好.
* 对于动作的原因进行持续的跟踪.例如,如果一个包正在安装,那么pip就会跟踪为什么这个包会被安装.
* 错误信息会非常有用.
* 代码简洁精悍可以很好的编程.
* 不必作为egg存档,能扁平化安装(仍然保存egg元数据)
* 原生的支持其他版本控制系统(Git, Mercurial and Bazaar)
* 卸载包
* 可以简单的定义修改一系列的安装依赖,还可以可靠的赋值一系列包.


