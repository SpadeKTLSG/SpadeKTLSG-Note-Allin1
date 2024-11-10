--批捅基础章

‍

‍

## Header

Python基础, 包括知识和搭建等Python学习体系内难度较为低的部分

‍

‍

### 速查

‍

### 环境

‍

#### **安装Python** 

[CSDN很细教程](https://blog.csdn.net/2201_75362610/article/details/129275182?ops_request_misc=&amp;request_id=&amp;biz_id=102&amp;utm_term=Python%E5%AE%89%E8%A3%85&amp;utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-129275182.142^v93^insert_down1&amp;spm=1018.2226.3001.4187)

Path设置:  上方环境变量加入Python位置

‍

#### venv/virtualenv

‍

大部分情况下的要求

所有的python项目都需要使用 venv 来进行，同时注意**避免引用系统的python库**。(保证包管理的可靠性)

‍

### 优化

‍

‍

#### 代码性能分析

```java
import heartrate
heartrate.trace(browser=True)
```

运行后访问127.0.0.1:9999

‍

#### pretty_errors

优化输出的异常信息

```python
import pretty_errors
```

‍

‍

# 知识

‍

## 底层

‍

‍

‍

## 命名

‍

### 基础命名

‍

一般    全小写_式驼峰  `this_is_potato`​

‍

类    单词首字母大写驼峰  `ClassName()`​

‍

全局变量    全大写加_驼峰  `MAX_SIZE`​

‍

‍

### 下划线命名

‍

1. 前置的单下划线：_var    弱内部使用标记(非强制), import不来(_name)
2. 后置的单下划线：var_
3. 前置的双下划线：__var
4. 前后置的双下划线：__var__    (内置)标识
5. 单独的下划线：_    抛弃变量

‍

通过通配符号从模块中导入所有函数将不会导入带下划线的函数

常规导入不受前缀下划线命名约定的影响

‍

## 工程

‍

### 模块

（Moudle）：模块是一个Python文件 ，以.py 结尾，包含了Python对象定义和Python语句

‍

导入模块方法: **推荐使用第一种**

```python
import 模块名

import 模块名 as new模块名

from 模块名 import methods
```

‍

‍

### 包

（Package）：Python中的包就是一个包含一个__init__.py文件的目录(文件夹), 模块组织成包.

‍

与文件夹的区别：

1. 包里面多一个__init__.py文件；
2. 导入包的时候，包里面的__init__.py文件自动执行

‍

导入包方法:

**注意：不可以import 包名,可以import 包名.模块名**

**推荐使用方式一、方式三**

**包导入时，会将导入的文件，全部执行一遍**

```python
from 包名 import 模块名

from 包名.模块名 import 函数名，变量名，类名

from 包名.包名 import 模块名（包里嵌套包）
```

‍

#### **init**.py

（开源封装时使用）

是python包的标识; 在__init__.py文件中导入包内函数后，在其他地方可以直接在包层次调用函数，无需找到包内具体文件

‍

‍

‍

# 基础

‍

‍

## 数型

‍

1. 重命名类型名字: a= int ,  可以利用a指代int了
2. ‍

‍

‍

### char

‍

是python不支持的数据类型

‍

‍

‍

### Int

‍

在Python中，整数除以整数的结果一定是一个浮点数，而不是一个整数

‍

‍

‍

#### 舍入

‍

round(number,(digits))  取整数,有参则指定小数位

‍

‍

### FLoat

‍

float(x) 返回x的浮点数

浮点数只有十进制和科学计数法的表示方法,必须必须要带小数部分

‍

‍

### Str

‍

使用引号（'）、双引号（"）、三引号<sup>（三引号可以由多行组成）</sup>（''' 或 """）来表示

Python使用字符串驻留来提高性能, 一些带有特殊符号的字符串可能有特性

‍

‍

‍

x * 3 表明将x对应的字符串连续执行3次

‍

‍

#### 格式操作

‍

* upper / lower    转大/小写
* swapacase    大小写交换
* capitalize    首字母大写其余小写
* title    首字母大写
* casefold    所有字母小写
* isalpha    全字母
* isdigit    全数字
* isalnum    全字母或数字

‍

‍

##### 例

‍

将一句话每个word大写`print(" ".join([word.capitalize() for word in a.split()]))`​

‍

‍

#### 查

‍

find (str, beg=0, end=len(string)) / rfind (右侧)    找不到返回-1

‍

count(str, start=0, end=len(string))    可选参数:搜索的开始与结束位置

‍

endswith("suffix", start, end)   以指定的字符段结尾（判断范围） ，空字符以TRUE计算

‍

‍

#### 改

‍

replace(old, new, maxreplacetimes)  替换

‍

strip()  lstrip()  rstrip()  去除

‍

split(str, num=string.count(str),keepends)  分割   |   num: 仅分隔 num+1 个子字符串, 默认-1分割所有 |  keepends参数 – 在输出结果里是否去掉换行符(‘\r’, ‘\r\n’, \n’)，默认为      False，不包含换行符，如果为 True，则保留换行符。

‍

取列表

> str.split(“o”)[1]得到的是第一个o和第二个o之间的内容 str.split(“o”)[3]得到的是第三个o后和第四个o前之间的内容

‍

partition(separator)    指定的分隔符将字符串分割成三元数组 (前面的,分隔符,后面的),找不到只有前面的

‍

‍

‍

#### 字符串对齐

‍

左对齐：str.ljust(width[, fillchar])  右对齐-rjust   居中：str.center(width[, fillchar])

‍

width – 指定字符串长度；fillchar – 填充字符，默认为空格

‍

‍

### 布尔

‍

等价表达

False    ==    None、0、""、()、[]、{}

‍

### 可变与否

‍

内建数据类型中可变的为list,dict,set;不可变为number,byte,string

‍

可变对象和不可变对象在传参时候注意,不可变对象的修改内容之后会影响到本身(引用);可变对象等于传值,改变的是别的东西(拷贝)

‍

‍

### 数学

‍

complex(re, im)	定义复数

复数类型中默认实部和虚部都是浮点类型(别被整数骗了)

‍

‍

### 编码

‍

ord() 函数返回单个字符的编码，chr() 函数把编码转成相应字符

‍

‍

‍

#### 解决编码解码问题

‍

问题情境:出现了找到这种十六进制字节串 ( bytes )

`b'\xc8\xcb\xc9\xfa\xbf\xe0\xb6\xcc\xa3\xac\xce\xd2\xd3\xc3Python'`​

‍

第一步，安装 chardet, 它是 char detect 的缩写。  
第二步，pip install chardet  
第三步，chardet.detect(`上面的编码`​)

‍

‍

## 基础语法

‍

‍

### 语法糖

‍

置换操作    a, b = b, a

‍

注意

如果a和b存在引用关系(特别在数组中)后者不能引用前者

‍

多重赋值操作    a=b=c=XX

‍

链式比较    1 < i < 3

‍

* eval('表达式'),执行并返回值
* exec('表达式'),执行不返回

‍

‍

### 拷贝

‍

‍

拷贝**可变类型元素**注意

‍

b = a					  # 简单赋值

c = a[:]				# 浅拷贝  

d = copy.copy(a)		      # 浅拷贝  

e = copy.deepcopy(a)	            # 深拷贝

‍

‍

一般乘号 * 与列表操作，实现快速元素复制

但是 * 复制的复合对象都是**浅引用**

深拷贝需要使用    **遍历赋值(重新生成)**

‍

### Input

‍

#### 连续输入

‍

```python
a,b,c=eval(input( ))

a,b,c=input().split(“分隔符”)
```

‍

‍

### Output

‍

​`print(XXX,end='')`​ 自定义输出结尾

​

‍

‍

### Format

‍

‍

#### 简单列表

‍

1. print('中北大学的英文是 ** %s**'  **%**  (中北))    # % 格式化，中北是变量 (多个就是括号,这里已经加上了)
2. print(**f** '中北大学的英文是 {中北}')   # f 格式化，也可以是F。GOOD
3. print('中北大学的英文是{0}'.format(中北))   # format格式化, 也可不指定位置,自动对应
4. print('{1}的英文是{0}'.format(中北,nuc))   #format索引格式化
5. print('{name}的英文是{ename}'.format(name='中北大学',ename='North University of China'))   #临时使用关键字格式化输出
6. yu = {'uppercase':'A','lowercase':'a'}  #字典格式化(索引)  
    print('The lowercase letter of {uppercase} is {lowercase}'.format(**yu))

    ‍

    ‍

    扩展

    **print('{name}非常喜欢{play}'.format(**dict1))        #通过字典设置**

    **print('{0[0]}非常喜欢{0[1]}'.format(['tom','打球']))       #通过列表设置**

‍

* %s   字符串
* %d   整数
* %f   浮点数

‍

填充和对齐:  **^ &lt; &gt;** 分别表示 **居中、左对齐、右对齐**，后面带宽度

```python
print('1.{:^14}'.format('陈某某'))  #共占位 14 个宽度，陈某某居中 
print('2.{:>14}'.format('陈某某'))  #陈某某居右对齐 
print('3.{:<14}'.format('陈某某'))  #陈某某居左对齐 
print('4.{:*<14}'.format('陈某某')) #陈某某居左对齐其它的*填充 
print('5.{:&>14}'.format('陈某某')) #陈某某居右对齐其它的&填充 
```

‍

‍

#### 数字格式化

‍

举例:

​`print('{:10.2f}'.format(3.1415926))`​

‍

{: 10.2f}    float输出宽度10,保留到2位.

{:+.2f}    float带符号保留小数点后两位

{:.0f}    float不带小数

‍

{:0>5d}    int数字补零 (右对齐, 宽度为5，剩余以0填充)

{:a<5d}    int数字补a (左对齐, 宽度为5，剩余以a填充)

‍

{:,}    , 以逗号分隔的数字

{:.2%}    % 转换百分制并保留小数点后2位

{:.2e}    e 指数计数并保留小数点后2位

‍

‍

#### 符号查询表

符号 `% + X`​

|符号|说明|
| ------| --------------------------------------------|
|**c**|**格式化字符及其 ASCII 码**|
|**s**|**格式化字符串**|
|**d**|**格式化整数**|
|**o**|**格式化无符号八进制数**|
|**x**|**格式化无符号十六进制数**|
|**X**|**格式化无符号十六进制数（大写）**|
|f|格式化浮点数字，可指定小数点后的精度|
|e|用科学计数法格式化浮点数|
|**E**|**作用同 %e，用科学计数法格式化浮点数**|
|**g**|**根据值的大小决定使用 %f 或 %e**|
|G|作用同 %g，根据值的大小决定使用 %f 或者 %E|

​​

格式化操作符辅助命令

|符号|说明|
| ------| ------------------------------------------------------------|
|m.n|m 是显示的最小总宽度，n 是小数点后的位数|
|-|用于左对齐|
|+|在正数前面显示加号（+）|
|#|在八进制数前面显示 '0o'，在十六进制数前面显示 '0x' 或 '0X'|
|0|显示的数字前面填充 '0' 取代空格|

‍

Python 的转义字符及其含义

|符号|说明|
| ------------| ----------------------|
|\'|单引号|
|\"|双引号|
|\a|发出系统响铃声|
|\b|退格符|
|\n|换行符|
|\t|横向制表符（TAB）|
|\v|纵向制表符|
|\r|回车符|
|\f|换页符|
|\o|八进制数代表的字符|
|\x|十六进制数代表的字符|
|\0|表示一个空字符|
|\\\\|反斜杠|

‍

‍

### 行函数

(列表推导式,好像也叫生成器表达式,列表解析)

可以添加更多的for语句部分对应不同的变量`[[x,y] for x in range(3) for y in range(3)]`​

‍

例

‍

使用列表生成式 [i+1 for i in a] 创建一个长度与 a 一行的临时列表，这步完成后，再做 sum 聚合试想如果你的数组 a 长度十百万级，再创建一个这样的临时列表就很不划算，最好是一边算一边聚合

```java
sum([i+1 for i in a])
```

```python

sum(i+1 for i in a)   这个参数将会得到一个生成器( generator )对象,生成器每迭代一步吐出( yield )一个元素并计算和聚合后，进入下一次迭代，直到终点
```

‍

‍

按条件f(x)是否成立分组

```python
def bif_by(lst, f):
    return [ [x for x in lst if f(x)],[x for x in lst if not f(x)]]
records = [25,89,31,34]
bif_by(records, lambda x: x<80) # [[25, 31, 34], [89]]
```

​

### Range

‍

**range[** 起始值 **，** 结束值 **，** 步长 **]    **

对应位置的 正下标 - 负下标 = len(a)

‍

使用正下标时，下标i取值范围为  `0  <=  i  < len(a)`​     超出范围为越界

使用负下标时，下标i取值范围为  `-1 >= i  > -len(a)-1`​    超出范围为越界

‍

range()返回的是range对象,不是list[]

‍

‍

### 流程控制

‍

#### elif

‍

超级嵌套	`return None if n < 0 else 1 if n < 1 else n * n`​​​​

‍

#### while

‍

‍

#### for

‍

for循中突然给遍历量赋值不会导致任何退出, 该循环的一定会执行下去

‍

#### switch

‍

switch-case字典实现

```python
def num_to_string(num):
    numbers = {
        0 : "zero",
        1 : "one",
        2 : "two",
        3 : "three"
    }
    return numbers.get(num, None)
```

‍

‍

‍

### 迭代器

‍

创建

```java
iter(obj [,sentinel])
```

返回一个可迭代对象, sentinel可省略(一旦迭代到此元素，立即终止)

‍

‍

使用

```java
for i in iter(lst):
```

‍

‍

### 序列解包

‍

一般的序列解包: 几个变量=容器()[], 对应位子的值赋值到变量中.

函数中的序列解包: 单星号参数/双星号参数

‍

实参是简单类型，形参是组合类型。利用可变长度参数，在形参名前加单星号（*）或双星号（**），把接收到的实参值组合为元组或字典。

实参是组合类型，形参是简单类型。在实参名前加写一个星号（*），把实参分解为独立元素，值传递给形参。

```python
def student(**stu): #键值匹配以及可变参数的例子
    cnt = 0
    for item in stu.values():
        if item == "河北":
            cnt += 1
        return cnt
```

```python
def make_car(brand, model, **car_info):
    car = {'品牌': brand, '型号': model}
    for key, value in car_info.items(): #items返回键值对,用两个变量接受
        car[key] = value
    return car

my_car = make_car('虎式', '改进型重型坦克底盘', 涂装='雪地特攻迷彩', 悬挂类型="克里斯蒂娜悬挂")
print(my_car)
```

‍

‍

‍

‍

## 进阶语法

‍

### counter

可以使用`Counter(List)`​​来计算List中每个元素出现的次数: 返回一个字典，键是列表中的元素，值是该元素在列表中出现的次数. Counter对象间可以做+,-运算来扩大缩小选区.

```python
#实现多个列表内元素的个数统计
def sumc(*c):
    if (len(c) < 1):        return
    mapc = map(Counter, c)
    s = Counter([])
    for ic in mapc: # ic 是一个Counter对象
        s += ic
    return s
```

‍

‍

### reduce

reduce函数先从列表（或序列）中取出2个元素执行指定函数，并将输出结果与第3个元素传入函数，输出结果再与第4个元素传入函数，…，以此类推，直到列表每个元素都取完。(比for循环慢), 也可以用lambda实现

```python
reduce(fun,[]) = f(f(f(x1,x2),x3),x4)
```

‍

‍

### filter

接受一个函数和一个序列, 根据后面的对象作用于函数返回的是False 和True 来确定是否保留.

```python
filter(is_odd,[])
```

‍

‍

### map

将传入的fun函数依次作用到序列S上的**每个元素**,并返回结果序列,还可以是多个参的函数

```python
map(fun, [])
```

‍

‍

### zip

聚合迭代器 zip(t,s) 利用传入的两个[]或range或str对象, 返回键值一一匹配组成元组为项的列表,长度不够以短的为主.

‍

组合使用生成字典

```python
d=dict(zip('abc',range(1,4)))
```

‍

‍

### enumerate

把一个可迭代/可遍历的对象组成一个索引序列, 同时获得其 **索引** 和 **值**

​`start`​：整数，表示索引开始的值，默认为 0

```python
enumerate(iterable, start=0)    
```

‍

e.g.

```python
for index, item in enumerate(list1, 1): print index, item
```

‍

‍

### slice

创建自定义切片对象, 包含切片信息, 调用时使用对应list[**slice**] 引用即可

```python
Var = slice(start, stop, [step])
```

‍

‍

​​  
​​

### groupby

单字段分组  + 多字段分组

‍

示例

```python
a =   #给一段天气记录:
[{'date': '2019-12-15', 'weather': 'cloud'},
 {'date': '2019-12-13', 'weather': 'sunny'},
 {'date': '2019-12-14', 'weather': 'cloud'}]
```

```python
a.sort(key=lambda x: x['weather'])

for k, items  in  groupby(a,key=lambda x:x['weather']): #后面指定key来判断是否选取
     print(k)  #输出按什么排列
     for i in items:  #遍历输出Groupby中的值
         print(i)

#输出:
cloud
{'date': '2019-12-15', 'weather': 'cloud'}
{'date': '2019-12-14', 'weather': 'cloud'}
sunny
{'date': '2019-12-13', 'weather': 'sunny'}
```

‍

‍

### chain

高效串联多个容器对象

‍

示例

```python
from itertools import chain

a = [1,3,5,0]
b = (2,4,6)
for i in chain(a,b):
  print(i)
```

‍

‍

### :=

海象表达式(赋值表达式)在表达式中进行变量赋值,可以看作是一种新的赋值运算符 **py3.8新特性**

‍

‍

if-else 条件表达式

```python
if a := 15 > 10:
    print('hello, walrus operator!')
```

‍

​​while​ 循环

```python
n = 5
while (n := n - 1) + 1: # 需要加1是因为执行输出前n就减1了
    print('hello, walrus operator!')
```

‍

更简洁的文件读取

```python
fp = open("test.txt", "r")
while line := fp.readline():
    print(line.strip())
```

‍

同样适合用于字典推导式和集合推导式

可能提升性能, 但是降低可读性

‍

‍

## 数学

‍

### 常数

‍

math.pi    π

math.e    自然常数

math.inf    正无穷大

math.nan    非浮点数

‍

‍

### 特殊方法

‍

fabs()    绝对值

ceil(),floor()    上下取整

factorial()    阶乘

pow(x,y)    X<sup>y</sup>幂

sqrt()    平方根

modf()    取整数和小数部分

‍

‍

‍

### 随机

‍

random()   无参: [0.0-1.0)之间的随机小数     双参: ==[a,b]==之间的整数

seed()    更改随机种子

Random.choice(list)     从列表随机返回一项

uniform(a, b)    返回一个在[a,b]范围内均匀分布的浮点数

‍

‍

抽样示例

```python
#从100个中抽取
from random import randint,sample
lst = [randint(0,50) for _ in range(100)] #特殊random写法
print(lst[:5])# [38, 19, 11, 3, 6]
lst_sample = sample(lst,10)
print(lst_sample) # [33, 40, 35, 49, 24, 15, 48, 29, 37, 24]
```

‍

‍

### 时间

‍

‍

**示例**

‍

计算运行时间

```python
def eval_function(func): 
    import time
    start_time = time.time()
    eval(func)
    end_time = time.time()
    print(f"{func}, 耗时：{int(round((end_time - start_time) * 1000000))} 微秒")
```

‍

‍

获取时间

```python
from datetime import date, datetime
from time import localtime
today_date = date.today()
print(today_date)  # 2019-12-22
today_time = datetime.today()
print(today_time)  # 2019-12-22 18:02:33.398894
local_time = localtime()
print(strftime("%Y-%m-%d %H:%M:%S", local_time))  # 转化为定制的格式
```

‍

‍

查看该月份的天数

```python
days = calendar.monthrange(2016, 2)[-1]  # 获取2016年2月的天数  
```

‍

‍

#### 时间转换

‍

字符时间和时间转换

```python
struct_time = strptime('2019-12-22 10:10:08', "%Y-%m-%d %H:%M:%S")
time.struct_time(tm_year=2019, tm_mon=12, tm_mday=22, tm_hour=10, tm_min=10,tm_sec=8, tm_wday=6, tm_yday=356, tm_isdst=-1)
```

‍

打印时间

```python
 print(time.strftime("%m-%d-%Y %H:%M:%S", time.localtime()))
```

‍

‍

#### 格式填充符

%m 月

%M 分钟

```python
%Y Year with century as a decimal number.
%m Month as a decimal number [01,12].
%d Day of the month as a decimal number [01,31].
%H Hour (24-hour clock) as a decimal number [00,23]
%M Minute as a decimal number [00,59].
%S Second as a decimal number [00,61].
%z Time zone offset from UTC.
%a Locale's abbreviated weekday name.
%A Locale's fu1l weekday name.
%b Locale's abbreviated month name .
```

‍

‍

‍

## 方法

‍

### 性质

‍

1. 方法第一个参数一定是self
2. 当一个 Python 文件被直接运行时，`__name__`​ 变量的值为 `"__main__"`​, 故可以这样来划分出主方法的空间:

    ```python
    if __name__ == '__main__':
        pass
    ```

    ‍
3. 含有默认参数的函数如果类型为容器且需要设置为空, 则务必设置此类默认参数为None(最好还要在使用前判断一下是否为None)：

    ​`def f(a,b=None):   # YES!`​

    ‍

    如果一个容器类型的默认参数被设置为空列表(）或空字典(l）等可变对象时，那么每次调用该函数时都会使用**同一个引用**，这可能会导致出现多次调用使用同一个变量的结果

‍

‍

==关键字==

* global    绑定共享变量
* nonlocal    用于内嵌函数中,声明变量 i 为非局部变量(常用于嵌套)

‍

‍

### 方法注释

‍

说明     `"""    """`​​

‍

‍

显示​    `函数名.__doc__`​​

‍

‍

注释    参数的`: 类型`​​是对该参数的注释。在函数参数列表与冒号之间的部分是对函数返回值的注释 `->类型`​​

‍

‍

‍

### 嵌套方法

‍

设置函数为返回值,函数内部的函数

```python
def parabola(a, b, c):
    def para(x):
        return a * x ** 2 + b * x + c 
    return para
p = parabola(2, 3, 4)
print(p(3))
```

‍

‍

### 参数

‍

#### **位置参数**

不能缺少,必须一一对应，缺一不可

‍

‍

#### **关键字参数**

通过‘键--值’方式为函数形参传值，不用按照位置为函数形参传值;

关键字参数**必须在位置参数右边**;

对同一个形参不能重复传值;

‍

‍

#### **默认参数**

无论是函数的定义还是调用，默认参数的定义**应该在位置形参右面**;

重复调用的时候,默认形参会**继承前一次调用结束后的值**

在Python中，函数的默认参数是在函数**定义时被计算**的，而不是在函数调用时计算。

如果默认参数是可变对象（例如列表、字典或类实例），则每次调用函数时都会继承前一次调用结束后的值。

为了避免这种情况，可以将默认参数设置为不可变对象（例如数字、字符串或元组）或使用None作为默认参数并在函数内部创建一个新对象。

‍

‍

#### **可变位置参数**

‍

​`def f(*a):`​​

‍

‍

‍

#### **可变关键字参数**

‍

​`def f(**a):`​​​

‍

‍

传值修改对象属性示例

```python
def make_car(brand, model, **car_info):
    car = {'品牌': brand, '型号': model}
    for key, value in car_info.items():
        car[key] = value
    return car


# 调用函数，传入汽车的品牌、型号和其他信息
my_car = make_car('虎式', '改进型重型坦克底盘', 涂装='雪地特攻迷彩', 悬挂类型="克里斯蒂娜悬挂")
```

‍

## OO

‍

‍

‍

## 异常

‍

‍

### 性质

‍

#### raise

抛出

‍

显示触发异常+ErrorType

‍

‍

#### assert

断言

‍

后接表达式, 如果为真什么都不做,否则直接抛异常

‍

‍

### **基本异常链**

Try-except

A-except-else-finally

‍

‍

**完整顺序(必须如此)**

```python
try:
    normal block=1
except X:
    Exception X handle
except IOError:
    print("IOError")
except NameError as e: # 异常捕获
    print(e,"NameError")

except:
    print("Error")

else: (可有)
    print("No Error, go here") #没有任何异常才来这里

finally: (可有,一定最后)
    print("Finally, go here")
```

‍

‍

‍

### 常见错误类型

‍

+Error

Name  访问未声明变量

Syntax  语法错误

ZeroDivision  除数为0

Index  索引超出

Key  请求不存在字典关键字

IO  输入输出错误

Attribute  尝试访问未知对象属性

Indentation  冒号换行后没有缩进

Value  传递的参数类型不正确

‍

‍
