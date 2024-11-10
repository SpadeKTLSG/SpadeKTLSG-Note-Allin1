--批捅高级章

‍

### Header

Python高级, 包括数构等Python学习体系内难度较为高的部分

‍

‍

# 高级

‍

‍

## 生成器

使用yield的函数被称为生成器函数

next()和send()都可以调用生成器

‍

#### 原理

return作为结尾的普通函数直接返回所有结果，程序终止不再运行，并销毁局部变量

yield会产生一个断点，暂停挂起函数，保存当前状态并且在yield处返回某个值，之后程序就不再往下运行了

有yield的函数则返回一个可迭代的生成器generator对象，可以使用for循环或者调用next()方法，send()方法遍历这个产生的实例化生成器对象来提取结果

每次调用含有`yield`​的函数行成一个新的`generator`​实例，各实例互不影响

‍

‍

#### next()

不能传值

‍

第一次对生成器对象调用next()，相当于启动生成器，会从生成器函数的第一行代码开始执行，直到第一次执行完yield语句后，跳出生成器函数。

之后调用next()，进入生成器函数后，会从yield语句的**下一句语句开始执行**，然后**重新运行到yield语句**，执行后，**跳出生成器函数**, 后面再次调用next()，依此类推。即执行一次next()则调用一次生成器函数，直至抛出不可迭代的错误。

python提供生成器的方法next()，next()就相当于“下一步”生成哪个数，这一次的next()开始的地方是接着上一次的yield停止的地方，所以调用next()的时候，生成器并不会将函数从头开始执行，只是接着上一步停止的地方开始，然后遇到yield后，返回表达式，此步就结束。

‍

‍

#### send()

可以传入值

如果用`send`​的话，出现的情况是，先接着上一次（`yield 4`​之后）执行，**先把接受到的1，2，3等赋值**给了`res`​，然后执行打印的作用，程序执行再次遇到`yield`​关键字，`yield`​会返回后面的值后，程序再次暂停，直到再次调用`next()`​方法或`send()`​方法。

​`生成器`​在迭代的过程中可以改变当前迭代值，而修改`普通迭代器`​的当前迭代值往往会发生异常，影响程序的执行。（因为`生成器`​有`send()`​方法）

‍

‍

#### from()

yield from 表达式将两个生成器连接起来,允许一个`generator`​生成器将其部分操作委派给另一个生成器

‍

‍

对于简单的迭代器，`yield from iterable`​本质上等于`for item in iterable: yield item`​的缩写版

```python
 def g(x):
      yield from range(x, 0, -1)
      yield from range(x)
   
list(g(5))
[5, 4, 3, 2, 1, 0, 1, 2, 3, 4]
```

‍

‍

**总结**

1. 迭代器（即可指子生成器）产生的值直接返还给调用者
2. 任何使用send()方法发给委派生产器（即外部生产器）的值被直接传递给迭代器。如果send值是None，则调用迭代器**next**()方法；如果不为None，则调用迭代器的send()方法。如果对迭代器的调用产生StopIteration异常，委派生产器恢复继续执行`yield from`​后面的语句；若迭代器产生其他任何异常，则都传递给委派生产器。
3. 除了GeneratorExit 异常外的其他抛给委派生产器的异常，将会被传递到迭代器的throw()方法。如果迭代器throw()调用产生了StopIteration异常，委派生产器恢复并继续执行，其他异常则传递给委派生产器。
4. 如果GeneratorExit异常被抛给委派生产器，或者委派生产器的close()方法被调用，如果迭代器有close()的话也将被调用。如果close()调用产生异常，异常将传递给委派生产器。否则，委派生产器将抛出GeneratorExit 异常。
5. 当迭代器结束并抛出异常时，`yield from`​表达式的值是其StopIteration 异常中的第一个参数。
6. 一个生成器中的`return expr`​语句将会从生成器退出并抛出 StopIteration(expr)异常。

‍

‍

#### 示例

‍

列表全展开

```python
a=[1,2,[3,4,[5,6],7],8,["python",6],9]    #多层列表展开成单层列表
def function(lst):
    for i in lst:
        if type(i)==list:
            yield from function(i)
        else:
            yield i


print(list(function(a))) # [1, 2, 3, 4, 5, 6, 7, 8, 'python', 6, 9]
```

‍

```python
#  改变当前迭代值的测试  
def myList(num):      # 定义生成器
    now = 0           # 当前迭代值，初始为0
    while now < num:
        val = (yield now)  # 返回当前迭代值，并接受可能的send发送值；
        now = now + 1 if val is None else val  # val为None，迭代值自增1，否则重新设定当前迭代值为val
 
my_list = myList(5)   # 得到一个生成器对象
print my_list.next()  # 返回当前迭代值
print my_list.next()
my_list.send(3)       # 重新设定当前的迭代值
print my_list.next()
```

‍

‍

**yield高级文件读取**

直接对文件对象调用`read()`​方法，会导致不可预测的内存占用。好的方法是利用固定长度的缓冲区来不断读取文件内容。通过`yield`​，不再需要编写读文件的迭代类，就可以轻松实现文件读取

```python
def read_file(fpath): 
    BLOCK_SIZE = 1024  # 设定读取长度
    with open(filepath//, 'rb') as f: 
        while True: 
            block = f.read(BLOCK_SIZE) #读取指定长度的文件段
            if block: #若读完了这段,这段有东西,进入yield等待
                yield block 
            else: 
                return #否则读完了,回去
```

‍

‍

‍

## lambda匿名函数

(行内函数)  

‍

### 性质

‍

自由参数之坑

```python
a = [lambda x: x+i for i in range(3)] # NO!
for f in a:
    print(f(1))

# 你可能期望输出：1,2,3. 但是实际却输出: 3,3,3. 定义lambda使用的 i 被称为自由参数，它只在调用lambda函数时，值才被真正确定下来
a = [lambda x,i=i: x+i for i in range(3)] # YES!
```

‍

注意如果在lambda中使用if进行条件判断，则else是必须声明的，否则会引起报错。如果不返回结果可以用 else None 表示。

‍

if...elif...else的多条件判断也可以用于lambda，但会使得代码过于复杂，所以不推荐

```python
squares_odd = list(map(lambda x: x ** 2 if x % 2 == 0 else None, range(10)))
print(f'squares_odd: {squares_odd}')
```

‍

‍

### 使用

‍

‍

赋值给变量, 变量调用lambda接口使用功能

```python
my_adder = lambda x, y: x + y
print(my_adder(1, 4))
```

‍

‍

赋值给其他函数，从而将其他函数用该lambda函数替换

```python
time.sleep = lambda x: None
time.sleep(1)
print(1)
```

‍

‍

作为其他函数的返回值，返回给调用者

```python
def my_count(g=33, h=66):
    a = lambda y, u: y + u  # 这里就定义了一个内部函数
    return a(g, h)
```

‍

‍

### 实例

‍

‍

配合map()

```python
a = [1, 3, 5]
b = [2, 4, 6]
c = list(map(lambda x, y: x * y, a, b))  # 这里就是直接从后面的a,b里面提取放到lambda鞭打
print(f'c: {c}')
```

‍

配合filter()

```python
# 此时 lambda返回的结果应是一个bool值，作为判断条件
e = list(filter(lambda x: x % 2 == 0, [1, 2, 3, 4, 5, 6]))
```

‍

配合sorted()

```python
# 利用lambda表达式作为排序的准则  key=lambda....
people = {"yoyo": 20, "admin": 28, "zhangsan": 25}
print(sorted(people.items(), key=lambda x: x[1], reverse=True))

people_2 = [
    {"name": "yoyo", "age": 20},
    {"name": "admin", "age": 28},
    {"name": "zhangsan", "age": 25},
]
print(sorted(people_2, key=lambda x: x['age'], reverse=True))
```

‍

配合reduce()

```python
# 此时lambda函数用于指定列表中两两相邻元素的结合条件。
f = reduce(lambda x, y: x + y, [1, 2, 3, 4, 5])
```

‍

‍

## 正则

正则表达式是对字符串操作的一种逻辑公式,这个“规则字符串”用来表达对字符串的一种过滤逻辑,需要re模块

re **.** match **(** pattern **,**  string **,**  flags  **=**  **0**)

‍

略

‍

## 流

‍

### With

**上下文管理器(清洁工)**

他能确保不管发生什么情况都可以清理(自动关闭文件,锁的获取释放)

​`with open(r'filename') as f: for i in f: print(i)`​

‍

‍

### 基础操作

路径指定    dos = r"c:\news"  #c:\\news 也可以,转义字符

‍

‍

‍

#### 打开

open/ with as

```python
open(file, mode='r', encoding=None)
fb = open(file=r"a.txt", mode="r", encoding="utf-8")
 # 同一个文件夹下直接使用 test.txt 作为文件名来操作
fb = open(file=r"D:\works\file1.txt", mode="r", encoding="utf-8")
 
#file 包含文件名的字符串，可以是绝对路径，可以是相对路径。
#mode 一个可选字符串，用于指定打开文件的模式。默认值 r 表示文本读。
#encoding 文本模式下指定文件的字符编码

with open(file=r"a.txt", mode="r", encoding="utf-8") as fb:

mode:
字符       意义
'r'       文本读取（默认）
'w'       文本写入，并先清空文件（慎用），文件不存在则创建
'x'       文本写，排它性创建，如果文件已存在则失败
'a'	  文本写，如果文件存在则在末尾追加，不存在则创建

组合字符:
'b'	    图像模式(二进制模式)，例如：'rb'表示二进制读：不需要encoding参数
't'	    文本模式（默认），例如：rt 一般省略 t
'+'	    读取与写入，例如：'r+' 表示同时读写
```

```python
#  with as 语句去除关闭以及finally过程简化代码.   
with open(file=r"a.txt", mode="a", encoding="utf-8") as fb: 
    fb.write("\n我是新写入的内容\n")
```

这个结构很常用

```python
f=open('cwx')
for line in f:
    print(line)
```

‍

‍

#### 操作

.read() 读所有,为字符串

.readline()  读一行

.readlines()  按行读取所有,为[],一行一个

‍

write()  写入

tell()  返回文件指针距离文件开头的字节整数

seek()  移动文件句柄,两个参数

```python
offset 表示偏移指针的字节数
whence 表示偏移参考，默认为 0  ;注意文本模式下只允许从文件的开头进行偏移，也即只支持 whence=0
0 表示偏移参考文件的开头，offset 必须是 >=0 的整数
1 表示偏移参考当前位置，offset 可以是负数
2 表示偏移参考文件的结尾，offset 一般是负数

直接seek(0)回到开头.
```

‍

‍

‍

## 序列化

‍

略

‍

## 多线程

‍

略

‍

‍

‍

## 装饰器

‍

略

‍

@lru_cache(None) 可以缓存一个函数的最近调用结果，从而提高程序执行的效率，特别适合于耗时的函数

‍

我们知道，Python中的变量名和对象是分离的。变量名其实是指向一个对象的引用。从本质上，装饰器起到的作用就是名称绑定(namebinding)，让同一个变量名指向一个新返回的函数对象，从而达到修改函数对象的目的。只不过，我们很少彻底地更改函数对象。在使用装饰器时，我们往往会在新函数内部调用旧的函数，以便保留旧函数的功能。这也是“装饰”名称的由来。

‍

示例

```python
#测试函数执行时间的装饰器示例
import time

def timing_func(fn):
    def wrapper():
        start=time.time()
        fn()   #执行传入的fn参数
        stop=time.time()
        return (stop-start)
    return wrapper

@timing_func
def test_list_append():
    lst=[]
    for i in range(0,100000):
        lst.append(i)  


@timing_func
def test_list_compre():
    [i for i in range(0,100000)]  #列表生成式


a=test_list_append()
c=test_list_compre()

print("test list append time:",a)
print("test list comprehension time:",c)
print("append/compre:",round(a/c,3))

test list append time: 0.0219423770904541
test list comprehension time: 0.007980823516845703
append/compre: 2.749
```

‍

```python
统计异常出现次数和时间的装饰器
装饰器，统计某个异常重复出现指定次数时，经历的时长:

def excepter(f):
    i = 0
    t1 = time.time()
    def wrapper():
        try:
            f()
        except Exception as e:
            nonlocal i
            i += 1
            print(f'{e.args[0]}: {i}')
            t2 = time.time()
            if i == n:
                print(f'spending time:{round(t2-t1,2)}')
    return wrapper
```

‍

```python
使用创建的装饰函数 excepter , n 是异常出现的次数。共测试了两类常见的异常： 被零除 和 数组越界 。
n = 10 # except count
   
@excepter
def divide_zero_except():
    time.sleep(0.1)
    j = 1/(40-20*2)
for _ in range(n):
    divide_zero_except()

@excepter
def outof_range_except():
    a = [1,3,5]
    time.sleep(0.1)
    print(a[3])


# test out of range except
for _ in range(n):
    outof_range_except()
```

‍

‍

函数作为返回值

闭包: 函数+环境变量

‍

函数curve()是一个二次函数。它除了自变量x外，还有a、b、c三个参数。通过curve_closure()这个闭包，我们可以预设a、b、c三个参数的值。从而起到减参的效果。

```java
def curve_closure(a, b, c):
    def curve(x):
        return a*x**2 + b*x+c
    return curve

curve1 = curve closure(1, 2，1)

```

‍

‍

## 可执行脚本制作

‍

### 安装制作器

PyInstaller

```java
pip install pyinstaller
```

‍

### 创建

导航到包含‘ main.py’脚本的目录并运行

这个命令将生成一个可执行文件。“—— onefile”选项确保将所有依赖项绑定到单个可执行文件中。

```java
pyinstaller —— onefile main.py
```

‍

### 找到对象

PyInstaller 将创建一个“ dist”目录

找到‘ main’可执行文件, 测试其

‍

### 

‍

‍

# 数构

> 记录数据结构API等

‍

‍

## 方法

‍

* 合并两个字典`{**{'a':1,'b':2}, **{'c':3}}`​  解包合并
* 列表去重`set([1,2,2,3,3,3])`​
* 逆序序列`list(range(10,-1,-1))`​
* 多个列表中的最大值`max(max([ [1,2,3], [5,1], [4] ], key=lambda v: max(v)))`​
* ‍

‍

‍

‍

## 列表[]

‍

### 性质

‍

‍

‍

### 操作

‍

‍

#### 增

‍

append()    表尾追加单元素

insert(sit ,str)    按位插入

extend()    将可迭代对象合并进来

a[n,n]    列表n位置插入元素

‍

‍

‍

#### 删

‍

del list[sit]    按位删除

pop（sit）    默认删除尾部 / 删除指定位置元素并返回此元素。

remove(e)    按值找第一次出现的删除(注意索引更改,尽量倒序删除,或者化为字典查找值删除)

‍

‍

列表删除

一般做法

```python
a=[x for x in lst if a != 114]
```

```python
def del_item(lst,e):
    return [lst.remove(i) for i in e if i==e] # NO!删不干净

#正确做法1
def del_item(lst,e):
    d = dict(zip(range(len(lst)),lst)) # YES! 构造字典
    return [v for k,v in d.items() if v!=e]

#正确做法2
a=[x for x in lst if a != 114]
```

‍

‍

#### 改

‍

```python
[] + []    组合
[] * n   重复
```

‍

**排序**

list.sort() & list.reverse()    永久, 只能对列表list进行排序

newl=sorted(list) & newl=reversed(list)    临时, 可以对所有可迭代类型进行排序

‍

reverse()    倒序

‍

‍

#### 查

‍

count()    统计某个元素出现的次数

index()    查找某个元素在列表中索引

‍

‍

#### 示例

‍

‍

‍

## 元组()

‍

### 性质

‍

### 操作

‍

#### 增

​​c = (5,)​​    创建含单个元素的元组需要加逗号!

('a', 'b') +  ('a', 'b')     组合

‍

#### 删

‍

‍

‍

#### 改

‍

‍

‍

#### 查

‍

‍

### 示例

‍

使用namedtuple命名元组提高可读性

```python
from collections import namedtuple

Point = namedtuple('Point', ['x', 'y', 'z'])  # 定义名字为Point的元祖，字段属性有x,y,z

lst = [Point(1.5, 2, 3.0), Point(-0.3, -1.0, 2.1), Point(1.3, 2.8, -2.5)]

print(lst[0].y - lst[1].y)
```

‍

‍

## 集合{}

‍

### 性质

元素为不可变对象。列表和字典是不能作为集合的元素的

‍

‍

### 操作

‍

‍

#### 增

set()    创建空集合,直接用{}不行

‍

‍

#### 删

discard    丢弃(相较于remove, **不会找不到报错**)

pop    随机删除一项

clear    清空

‍

‍

#### 改

update()    并集更新

‍

‍

#### 查

&,|,-    求交,并,差集

‍

‍

‍

### 示例

‍

‍

‍

## 字典{:}

‍

‍

### 性质

‍

元素为不可变对象。列表和字典是不能作为集合的元素的

‍

‍

### 操作

‍

#### 增

dict(name=value)    ()键值对创建

dict([('name', 'value')])    [(,)] list + tuple创建

‍

#### 删

del dict['XXX']    删除键值对'XXX'

clear()    清空

del dict_name    删除

‍

#### 改

update()    追加

‍

#### 查

get()    获取值

items()    获取字典的项列表，二元元组构成

‍

‍

### 示例

‍

计数器

```python
for item in lists:  
   if item in count_dict:   
        count_dict[item] += 1  //在内就添加
   else: count_dict[item] = 1  //否则初始化(这里会自动生成键值对)  
```

‍

两个List: key&value 创建dict

```python
dict(zip(key,value))
dict1 = dict(zip(key_list,value_list))
```

‍

‍

‍
