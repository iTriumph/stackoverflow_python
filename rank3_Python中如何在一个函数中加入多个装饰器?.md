| rank | ▲ | ✰ | vote | url |
|:-:|:-:|:-:|:-:|:-:|
|   3  |  1346 | 1485 | 2648 | [url](http://stackoverflow.com/questions/739654/how-can-i-make-a-chain-of-function-decorators-in-python) |

## Question:Python中如何在一个函数中加入多个装饰器?

怎么做才能让一个函数同时用两个装饰器,像下面这样:

```python
@makebold
@makeitalic
def say():
   return "Hello"
```

我希望得到

```python
<b><i>Hello</i></b>
```

我只是想知道装饰器怎么工作的!

## Answer:1

去看看[文档](https://docs.python.org/2/reference/compound_stmts.html#function),答在下面:

```python
def makebold(fn):
    def wrapped():
        return "<b>" + fn() + "</b>"
    return wrapped

def makeitalic(fn):
    def wrapped():
        return "<i>" + fn() + "</i>"
    return wrapped

@makebold
@makeitalic
def hello():
    return "hello world"

print hello() ## returns <b><i>hello world</i></b>
```

## Answer:2

如果你不想看详细的解释的话请看上面那个答案.

### 装饰器基础

#### Python的函数都是对象

要了解装饰器,你必须了解Python中的函数都是对象.这个意义非常重要.让我们看看一个简单例子:

```python
def shout(word="yes"):
    return word.capitalize()+"!"

print shout()
# 输出 : 'Yes!'

# 作为一个对象,你可以把它赋值给任何变量

scream = shout

# 注意啦我们没有加括号,我们并不是调用这个函数,我们只是把函数"shout"放在了变量"scream"里.
# 也就是说我们可以通过"scream"调用"shout":

print scream()
# 输出 : 'Yes!'

# 你可以删除旧名"shout",而且"scream"依然指向函数

del shout
try:
    print shout()
except NameError, e:
    print e
    #输出: "name 'shout' is not defined"

print scream()
# 输出: 'Yes!'
```

好了,先记住上面的,一会还会用到.

Python函数另一个有趣的特性就是你可以在一个函数里定义另一个函数!

```python
def talk():

    # 你可以在"talk"里定义另一个函数 ...
    def whisper(word="yes"):
        return word.lower()+"..."

    # 让我们用用它!

    print whisper()

# 每次调用"talk"时都会定义一次"whisper",然后"talk"会调用"whisper"
talk()
# 输出: 
# "yes..."

# 但是在"talk"意外"whisper"是不存在的:

try:
    print whisper()
except NameError, e:
    print e
    #输出 : "name 'whisper' is not defined"*
```

### 函数引用

好,终于到了有趣的地方了...

已经知道函数就是对象.因此,对象:

* 可以赋值给一个变量
* 可以在其他函数里定义

这就意味着函数可以返回另一个函数.来看看!☺

```python
def getTalk(kind="shout"):

    # 在函数里定义一个函数
    def shout(word="yes"):
        return word.capitalize()+"!"

    def whisper(word="yes") :
        return word.lower()+"...";

    # 返回一个函数
    if kind == "shout":
        # 这里不用"()",我们不是要调用函数
        # 只是返回函数对象
        return shout  
    else:
        return whisper

# 怎么用这个特性呢?

# 把函数赋值给变量
talk = getTalk()      

# 可以看到"talk"是一个函数对象
print talk
# 输出 : <function shout at 0xb7ea817c>

# 函数返回的是对象:
print talk()
# 输出 : Yes!

# 不嫌麻烦你也可以这么用
print getTalk("whisper")()
# 输出 : yes...
```

既然可以`return`一个函数, 你也可以在把函数作为参数传递:

```python
def doSomethingBefore(func): 
    print "I do something before then I call the function you gave me"
    print func()

doSomethingBefore(scream)
# 输出: 
#I do something before then I call the function you gave me
#Yes!
```

学习装饰器的基本知识都在上面了.装饰器就是"wrappers",它可以让你在你装饰函数之前或之后执行程序,而不用修改函数本身.

#### 自己动手实现装饰器

怎么样自己做呢:

```python
# 装饰器就是把其他函数作为参数的函数
def my_shiny_new_decorator(a_function_to_decorate):

    # 在函数里面,装饰器在运行中定义函数: 包装.
    # 这个函数将被包装在原始函数的外面,所以可以在原始函数之前和之后执行其他代码..
    def the_wrapper_around_the_original_function():

        # 把要在原始函数被调用前的代码放在这里
        print "Before the function runs"

        # 调用原始函数(用括号)
        a_function_to_decorate()

        # 把要在原始函数调用后的代码放在这里
        print "After the function runs"

    # 在这里"a_function_to_decorate" 函数永远不会被执行
    # 在这里返回刚才包装过的函数
    # 在包装函数里包含要在原始函数前后执行的代码.
    return the_wrapper_around_the_original_function

# 加入你建了个函数,不想修改了
def a_stand_alone_function():
    print "I am a stand alone function, don't you dare modify me"

a_stand_alone_function() 
#输出: I am a stand alone function, don't you dare modify me

# 现在,你可以装饰它来增加它的功能
# 把它传递给装饰器,它就会返回一个被包装过的函数.

a_stand_alone_function_decorated = my_shiny_new_decorator(a_stand_alone_function)
a_stand_alone_function_decorated()
#输出s:
#Before the function runs
#I am a stand alone function, don't you dare modify me
#After the function runs
```

现在,你或许每次都想用`a_stand_alone_function_decorated`代替`a_stand_alone_function`,很简单,只需要用`my_shiny_new_decorator`返回的函数重写`a_stand_alone_function`:

```python
a_stand_alone_function = my_shiny_new_decorator(a_stand_alone_function)
a_stand_alone_function()
#输出:
#Before the function runs
#I am a stand alone function, don't you dare modify me
#After the function runs

# 想到了吗,这就是装饰器干的事!
```

#### 让我们看看装饰器的真实面纱

用上一个例子,看看装饰器的语法:

```python
@my_shiny_new_decorator
def another_stand_alone_function():
    print "Leave me alone"

another_stand_alone_function()  
#输出:  
#Before the function runs
#Leave me alone
#After the function runs
```

就这么简单.`@decorator`就是下面的简写:

```python
another_stand_alone_function = my_shiny_new_decorator(another_stand_alone_function)
```

装饰器就是[ decorator design pattern](http://en.wikipedia.org/wiki/Decorator_pattern)的pythonic的变种.在Python中有许多经典的设计模式来满足开发者.

当然,你也可以自己写装饰器:

```python
def bread(func):
    def wrapper():
        print "</''''''\>"
        func()
        print "<\______/>"
    return wrapper

def ingredients(func):
    def wrapper():
        print "#tomatoes#"
        func()
        print "~salad~"
    return wrapper

def sandwich(food="--ham--"):
    print food

sandwich()
#outputs: --ham--
sandwich = bread(ingredients(sandwich))
sandwich()
#outputs:
#</''''''\>
# #tomatoes#
# --ham--
# ~salad~
#<\______/>
```

用Python装饰器语法糖:

```python
@bread
@ingredients
def sandwich(food="--ham--"):
    print food

sandwich()
#outputs:
#</''''''\>
# #tomatoes#
# --ham--
# ~salad~
#<\______/>
```

改变一下顺序:

```python
@ingredients
@bread
def strange_sandwich(food="--ham--"):
    print food

strange_sandwich()
#outputs:
##tomatoes#
#</''''''\>
# --ham--
#<\______/>
# ~salad~
```

#### 现在:回答你的问题...

作为结论,相信你现在已经知道答案了:

```python
# 字体变粗装饰器
def makebold(fn):
    # 装饰器将返回新的函数
    def wrapper():
        # 在之前或者之后插入新的代码
        return "<b>" + fn() + "</b>"
    return wrapper

# 斜体装饰器
def makeitalic(fn):
    # 装饰器将返回新的函数
    def wrapper():
        # 在之前或者之后插入新的代码
        return "<i>" + fn() + "</i>"
    return wrapper

@makebold
@makeitalic
def say():
    return "hello"

print say() 
#输出: <b><i>hello</i></b>

# 这相当于 
def say():
    return "hello"
say = makebold(makeitalic(say))

print say() 
#输出: <b><i>hello</i></b>
```

别轻松太早,看看下面的高级用法

### 装饰器高级用法

#### 在装饰器函数里传入参数

```python
# 这不是什么黑魔法,你只需要让包装器传递参数:

def a_decorator_passing_arguments(function_to_decorate):
    def a_wrapper_accepting_arguments(arg1, arg2):
        print "I got args! Look:", arg1, arg2
        function_to_decorate(arg1, arg2)
    return a_wrapper_accepting_arguments

# 当你调用装饰器返回的函数时,也就调用了包装器,把参数传入包装器里,
# 它将把参数传递给被装饰的函数里.

@a_decorator_passing_arguments
def print_full_name(first_name, last_name):
    print "My name is", first_name, last_name

print_full_name("Peter", "Venkman")
# 输出:
#I got args! Look: Peter Venkman
#My name is Peter Venkman
```
