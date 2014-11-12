| rank | ▲ | ✰ | vote | url |
|:-:|:-:|:-:|:-:|:-:|
|  80 | 336 | 196 | 470 | [url](http://stackoverflow.com/questions/372885/how-do-i-connect-to-a-mysql-database-in-python) |

***

## 如何连接MySQL?

在python程序里如何链接MySQL数据库?

***

### 连接MYSQL需要3步

#### 1 安装

你必须先安装MySQL驱动.和PHP不一样,Python只默认安装了SQLite的驱动.最常用的包是[MySQLdb](http://pypi.python.org/pypi/MySQL-python/)但是用`easy_install`安装很困难.

对于Window用户,你可以获取[MySQLdb](http://sourceforge.net/project/showfiles.php?group_id=22307)的exe.

对于Linux,可以下载`python-mysqldb`(可以用`sudo apt-get install python-mysqldb`命令直接在命令行下载)

对于Mac用户,可以用Macport下载[MySQLdb](http://stackoverflow.com/questions/1448429/how-to-install-mysqldb-python-data-access-library-to-mysql-on-mac-os-x#1448476)

#### 2 使用

装完之后重启.这不是强制的,但是这样做可以减少问题.所以请重启.

然后就像用其他包一样:

```python
#!/usr/bin/python
import MySQLdb

db = MySQLdb.connect(host="localhost", # your host, usually localhost
                     user="john", # your username
                      passwd="megajonhy", # your password
                      db="jonhydb") # name of the data base

# you must create a Cursor object. It will let
#  you execute all the queries you need
cur = db.cursor()

# Use all the SQL you like
cur.execute("SELECT * FROM YOUR_TABLE_NAME")

# print all the first cell of all the rows
for row in cur.fetchall() :
    print row[0]
```

当然,还有许多用法和选项,我只是举了一个基本的例子.你可以看看文档.[A good starting point.](http://www.mikusa.com/python-mysql-docs/)

#### 3 高级用法

一旦你知道它是如何工作的,你可能想用[ORM](https://en.wikipedia.org/wiki/Object-Relational_Mapping)来避免手动写入SQL,来把表变成Python对象.Python中最有名的ORM叫做[SQLAlchemy](http://www.sqlalchemy.org/)

我强烈推荐:你的生活质量肯定能获得提高.

最近在Python里又发现了一个好东西:[peewee](http://peewee.readthedocs.org/en/latest/index.html).它是个非常轻巧的ORM,非常容易安装和使用.我的小项目和独立app都使用它,而那些工具像SQLLAlchemy或者Django用在这里有点小题大做了:

```python
import peewee
from peewee import *

db = MySQLDatabase('jonhydb', user='john',passwd='megajonhy')

class Book(peewee.Model):
    author = peewee.CharField()
    title = peewee.TextField()

    class Meta:
        database = db

Book.create_table()
book = Book(author="me", title='Peewee is cool')
book.save()
for book in Book.filter(author="me"):
    print book.title

Peewee is cool
```

这个例子可以运行.除了peewee(`pip install peewee`)不需要别的的依赖.安装不复杂.它非常cool!
