--批捅特殊章

‍

## Header

Python特殊, 包括数据分析, 可视化, 爬虫等特殊操作功能

‍

‍

# 可视化

‍

## Turtle画笔工具

‍

### 介绍

[源码地址](https://docs.python.org/3.11/library/turtle.html#)

‍

‍

### 实例

‍

#### turtle绘制奥运五环图

>  小学时候就学过的矢量图形的小乌龟作画

```python
import turtle as p #导入

def drawCircle(x,y,c='red'):#定义画圆函数
    p.pu()# 抬起画笔
    p.goto(x,y) # 绘制圆的起始位置
    p.pd()# 放下画笔
    p.color(c)# 绘制c色圆环
    p.circle(30,360) #绘制圆：半径，角度

p = turtle
p.pensize(3) # 画笔尺寸设置3

drawCircle(0,0,'blue')
drawCircle(60,0,'black')
drawCircle(120,0,'red')
drawCircle(90,-30,'green')
drawCircle(30,-30,'yellow')  
p.done()
```

‍

‍

#### turtle绘制漫天雪花

‍

```python
# -*- coding: gbk -*-
import turtle as p
import random


def snow(snow_count):
    p.hideturtle()  # 隐藏画笔
    p.speed(500)  # 这个方法可以接受数值和字符串(例如,fast),预设的速度:“fastest”: 0 “fast”: 10 “normal”: 6 “slow”: 3 “slowest”: 1 ,这里超出范围用的是像素速度的表达
    p.pensize(2)  # 尺寸

    for i in range(snow_count):

        # 通过随机数生成随机颜色
        r = random.random()
        g = random.random()
        b = random.random()
        p.pencolor(r, g, b)

        p.pu()  # put the pen up ,no draw when moving
        p.goto(random.randint(-350, 350), random.randint(1, 270))  # 折跃
        p.pd()  # put the pen down , draw when moving

        # 生成通过同心射线组织成的雪花
        dens = random.randint(8, 12)
        snowsize = random.randint(10, 14)
        for _ in range(dens):
            p.forward(snowsize)  # 向当前画笔方向移动snowsize像素长度
            p.backward(snowsize)  # 向当前画笔相反方向移动snowsize像素长度
            p.right(360 / dens)  # 顺时针移动360 / dens度


def ground(ground_line_count):
    p.hideturtle()
    p.speed(500)

    for i in range(ground_line_count):
        p.pensize(random.randint(5, 10))
        x = random.randint(-400, 350)
        y = random.randint(-280, -1)
        r = -y / 280
        g = -y / 280
        b = -y / 280

        p.pencolor(r, g, b)
        p.penup()  # 抬起画笔
        p.goto(x, y)  # 让画笔移动到此位置
        p.pendown()  # 放下画笔
        p.forward(random.randint(40, 100))  # 眼当前画笔方向向前移动40~100距离


def main():
    p.setup(800, 600, 0, 0)
    # p.tracer(False)
    p.bgcolor("black")
    snow(30)
    ground(30)
    # p.tracer(True)
    p.mainloop()
```

‍

‍

## plotly工具

‍

### 介绍

‍

‍

### 实例

‍

#### 基本图

```python
#柱状图+折线图: 通过网页显示
import plotly.graph_objects as go
fig = go.Figure()
fig.add_trace(
    go.Scatter(
        x=[0, 1, 2, 3, 4, 5],
        y=[1.5, 1, 1.3, 0.7, 0.8, 0.9]
    ))
fig.add_trace(
    go.Bar(
        x=[0, 1, 2, 3, 4, 5],
        y=[2, 0.5, 0.7, -1.2, 0.3, 0.4]
    ))
fig.show()
```

‍

‍

‍

## matplotlib工具

‍

### 介绍

‍

‍

### 实例

‍

‍

#### 饼图

‍

```python
# -*- coding: gbk -*-
import matplotlib.pyplot as plt

plt.rcParams['font.sans-serif'] = ['SimHei']  # 设置中文显示

# 设置标签和数据
labels = ['娱乐', '育儿', '饮食', '房贷', '交通', '其它']
sizes = [1, 4, 5, 8, 13, 14]
explode = [0, 0, 0, 0, 0.1, 0]

plt.pie(sizes, explode=explode, labels=labels, autopct='%1.1f%%', shadow=False, startangle=150)  # 绘制饼图

plt.title('饼图数据可视化')
plt.show()

```

‍

‍

#### 折线图

‍

工具文件  `example_utils`​

```python
# -*- coding: gbk -*-

import matplotlib.pyplot as plt


# 创建画图fig和axes
def setup_axes():
    fig, axes = plt.subplots(ncols=3, figsize=(6.5, 3))  # 创建画图fig和axes
    for ax in fig.axes:
        ax.set(xticks=[], yticks=[]) # 设置x轴和y轴的刻度
    fig.subplots_adjust(wspace=0, left=0, right=0.93)  # 设置子图之间的间距
    return fig, axes


# 图片标题
def title(fig, text, y=0.9):
    fig.suptitle(text, size=14, y=y, weight='semibold', x=0.98, ha='right',
                 bbox=dict(boxstyle='round', fc='floralwhite', ec='#8B7E66',
                           lw=2))


# 为数据添加文本注释
def label(ax, text, y=0):
    ax.annotate(text, xy=(0.5, 0.00), xycoords='axes fraction', ha='center',
                style='italic',
                bbox=dict(boxstyle='round', facecolor='floralwhite',
                          ec='#8B7E66'))

```

‍

生成文件

```python
# -*- coding: gbk -*-
import numpy as np
import matplotlib.pyplot as plt
import example_utils

x = np.linspace(0, 10, 100)  # 生成0-10之间的100个数
fig, axes = example_utils.setup_axes()  # 创建画图fig和axes
for ax in axes:
    ax.margins(y=0.10)  # 设置y轴的边距

# 子图1 默认plot多条线，颜色系统分配
for i in range(1, 6):
    axes[0].plot(x, i * x)  # plot(x, y, ...) x和y的长度必须相等

# 子图2 展示线的不同linestyle
for i, ls in enumerate(['-', '--', ':', '-.']):
    axes[1].plot(x, np.cos(x) + i, linestyle=ls)

# 子图3 展示线的不同linestyle和marker
for i, (ls, mk) in enumerate(zip(['', '-', ':'], ['o', '^', 's'])):
    axes[2].plot(x, np.cos(x) + i * x, linestyle=ls, marker=mk, markevery=10)  # markevery=10表示每10个点标一个点

# 设置标题
example_utils.title(fig, '"ax.plot(x, y, ...)": Lines and/or markers', y=0.95)
fig.savefig('plot_example.png', facecolor='none')  # 保存图片
plt.show()
```

‍

‍

#### 直方图

‍

‍

#### 散点图

```python
# -*- coding: gbk -*-
import numpy as np
import matplotlib.pyplot as plt
import example_utils

# 随机生成数据
np.random.seed(1874)
x, y, z = np.random.normal(0, 1, (3, 100))
t = np.arctan2(y, x)
size = 50 * np.cos(2 * t) ** 2 + 10
fig, axes = example_utils.setup_axes()

# 子图1
axes[0].scatter(x, y, marker='o', color='darkblue', facecolor='white', s=80)
example_utils.label(axes[0], 'scatter(x, y)')

# 子图2
axes[1].scatter(x, y, marker='s', color='darkblue', s=size)
example_utils.label(axes[1], 'scatter(x, y, s)')

# 子图3
axes[2].scatter(x, y, s=size, c=z, cmap='gist_ncar')
example_utils.label(axes[2], 'scatter(x, y, s, c)')

example_utils.title(fig, '"ax.scatter(...)": Colored/scaled markers', y=0.95)

fig.savefig('scatter_example.png', facecolor='none')
plt.show()
```

‍

#### 柱状图

```python
# -*- coding: gbk -*-
import numpy as np
import matplotlib.pyplot as plt
import example_utils


def main():
    fig, axes = example_utils.setup_axes()
    basic_bar(axes[0])
    tornado(axes[1])
    general(axes[2])
    # example_utils.title(fig, '"ax.bar(...)": Plot rectangles')
    fig.savefig('bar_example.png', facecolor='none')
    plt.show()


# 子图1
def basic_bar(ax):
    # 数据
    y = [1, 3, 4, 5.5, 3, 2]
    err = [0.2, 1, 2.5, 1, 1, 0.5]
    x = np.arange(len(y))

    # 绘图
    ax.bar(x, y, yerr=err, color='lightblue', ecolor='black')  # ecolor: error bar color
    ax.margins(0.05)  # 0.05: 5% of the y-axis range
    ax.set_ylim(bottom=0)  # set the bottom of the y-axis to 0
    example_utils.label(ax, 'bar(x, y, yerr=e)')  # label the axes


# 子图2
def tornado(ax):
    # 数据
    y = np.arange(8)
    x1 = y + np.random.random(8) + 1
    x2 = y + 3 * np.random.random(8) + 1

    # 绘图
    ax.barh(y, x1, color='lightblue')  # barh: horizontal bar chart
    ax.barh(y, -x2, color='salmon')  # negative values are drawn to the left
    ax.margins(0.15)  # 0.15: 15% of the x-axis range
    example_utils.label(ax, 'barh(x, y)')  # label the axes


# 子图3
def general(ax):
    # 数据
    num = 10
    left = np.random.randint(0, 10, num)
    bottom = np.random.randint(0, 10, num)
    width = np.random.random(num) + 0.5
    height = np.random.random(num) + 0.5

    # 绘图
    ax.bar(left, height, width, bottom, color='salmon')  # bar: general bar chart
    ax.margins(0.15)  # 0.15: 15% of the x-axis range
    example_utils.label(ax, 'bar(l, h, w, b)')  # label the axes


main()
```

‍

#### 等高线图

```python
# -*- coding: gbk -*-
import matplotlib.pyplot as plt
import numpy as np
from matplotlib.cbook import get_sample_data
import example_utils

z = np.load(get_sample_data('bivariate_normal.npy'))
fig, axes = example_utils.setup_axes()
axes[0].contour(z, cmap='gist_earth')
example_utils.label(axes[0], 'contour')
axes[1].contourf(z, cmap='gist_earth')
example_utils.label(axes[1], 'contourf')
axes[2].contourf(z, cmap='gist_earth')
cont = axes[2].contour(z, colors='black')
axes[2].clabel(cont, fontsize=6)
example_utils.label(axes[2], 'contourf + contour\n + clabel')
# example_utils.title(fig, '"contour, contourf, clabel": Contour/label 2D data',
#                     y=0.96)
fig.savefig('contour_example.png', facecolor='none')
plt.show()
```

‍

#### 动画制作

‍

‍

‍

‍

## pyecharts

‍

‍

### 介绍

‍

> pyecharts对Numpy数据绘图不支持,请使用原生的List

‍

‍

‍

### 实例

‍

‍

设置y轴显示在右侧

```python
.set_global_opts(yaxis_opts=opts.AxisOpts(position='right'))
```

‍

‍

#### 入门样例

‍

```python
from pyecharts.charts import Bar
bar = Bar()
    bar.add_xaxis(["衬衫", "羊毛衫", "雪纺衫", "裤子", "高跟鞋", "袜子"])
    bar.add_yaxis("商家A", [5, 20, 36, 10, 75, 90])
    # render 会生成本地 HTML 文件，默认会在当前目录生成 render.html 文件
    # 也可以传入路径参数，如 bar.render("mycharts.html")
    .set_global_opts(title_opts=opts.TitleOpts(title="主标题", subtitle="副标题"))

bar.render()
```

‍

‍

#### 饼图

‍

‍

‍

```python
# -*- coding: gbk -*-
from pyecharts import options as opts
from pyecharts.charts import Pie
from random import randint


def pie_base() -> Pie:
    c = (
        Pie()
        .add("", [list(z) for z in zip(['宝马', '法拉利', '奔驰', '奥迪', '大众',
                                        '丰田', '特斯拉'],
                                       [randint(1, 20) for _ in range(7)])])
        .set_global_opts(title_opts=opts.TitleOpts(title="Pie-基本示例"))
        .set_series_opts(label_opts=opts.LabelOpts(formatter="{b}: {c}"))
    )
    return c


pie_base().render('./pie_pyecharts.html')
```

‍

‍

‍

#### 仪表盘

‍

```python
from pyecharts import options as opts
from pyecharts.charts import Gauge

# 指示针的颜色与数值所属区间的颜色一致
c = (
    # 设置展示的图形大小
    Gauge(init_opts=opts.InitOpts(width="800px", height="500px"))
    .add(
        "业务指标",
        [("完成率", 80.5)],
        # 设置比例大小
        radius="70%",
        # 设置起始、终止刻度
        min_=0, max_=100,
        # 分割成为5段
        split_number=10,
        axisline_opts=opts.AxisLineOpts(
            linestyle_opts=opts.LineStyleOpts(
                # 设置区间颜色、仪表宽度
                color=[(0.3, "#67e0e3"), (0.7, "#37a2da"), (1, "#fd666d")], width=30
            )
        ),
        # 文字部分的字体颜色及大小设置
        title_label_opts=opts.LabelOpts(
            font_size=32, color="blue", font_family="Microsoft YaHei"
        ),
        # 标注的数字字体及格式
        detail_label_opts=opts.LabelOpts(
            # 数值标签的格式设定
            formatter="{value}%", font_size=32, color="black", font_family="Microsoft YaHei"),

    )
    .set_global_opts(
        title_opts=opts.TitleOpts(title="Gauge-分割段数-Label"),
        legend_opts=opts.LegendOpts(is_show=False),
    )
    # 生成网页形式
    .render("gauge_splitnum_label.html")
)
c.render_notebook()  # 在notebook直接生成图形
```

‍

简单版

```python
# -*- coding: gbk -*-
from pyecharts import charts

gauge = charts.Gauge()
gauge.add('Python小例子', [('Python机器学习', 30), ('Python基础', 70.), ('Python正则', 90)])
gauge.render(path="./仪表盘.html")

```

‍

‍

#### 漏斗图

‍

以7种车型及某个属性值绘制的漏斗图，属性值大越靠近漏斗的大端

‍

```python
# -*- coding: gbk -*-
from pyecharts import options as opts
from pyecharts.charts import Funnel
from random import randint


def funnel_base() -> Funnel:
    c = (
        Funnel()  # Funnel() is a class
        .add("豪车", [list(z) for z in zip(['宝马', '法拉利', '奔驰', '奥迪', '大众', '丰田', '特斯拉'], [randint(1, 20) for _ in range(7)])])
        .set_global_opts(title_opts=opts.TitleOpts(title="豪车漏斗图"))
    )
    return c


funnel_base().render('./car_fnnel.html')  # 输出到html文件
```

‍

‍

‍

#### 日历图

‍

类似那种Github贡献统计的样子

‍

```python
# -*- coding: gbk -*-
import datetime
import random
from pyecharts import options as opts
from pyecharts.charts import Calendar


def calendar_interval_1() -> Calendar:
    # 数据集
    begin = datetime.date(2019, 1, 1)
    end = datetime.date(2019, 12, 27)
    data = [
        [str(begin + datetime.timedelta(days=i)), random.randint(1000, 25000)]
        for i in range(0, (end - begin).days + 1, 2)  # 隔天统计
    ]

    calendar = (
        Calendar(init_opts=opts.InitOpts(width="1200px")).add(
            "", data, calendar_opts=opts.CalendarOpts(range_="2019"))
        .set_global_opts(
            title_opts=opts.TitleOpts(title="Calendar-2019年步数统计"),
            visualmap_opts=opts.VisualMapOpts(
                max_=25000,
                min_=1000, orient="horizontal",
                is_piecewise=True,
                pos_top="230px",
                pos_left="100px",
            ),
        )
    )
    return calendar


calendar_interval_1().render('./calendar.html')
```

‍

‍

#### 极坐标图

‍

类似地理的风向玫瑰图

‍

极坐标表示为 (夹角,半径) ，如(6,94)表示夹角为6，半径94的点

```python
import random
from pyecharts import options as opts
from pyecharts.charts import Page, Polar


def polar_scatter0() -> Polar:
    data = [(alpha, random.randint(1, 100)) for alpha in range(101)]  # r =random.randint(1, 100)
    print(data)
    c = (
        Polar()
        .add("", data, type_="bar", label_opts=opts.LabelOpts(is_show=False))
        .set_global_opts(title_opts=opts.TitleOpts(title="Polar"))
    )
    return c


polar_scatter0().render('./polar.html')
```

‍

‍

#### 词云图

‍

以大小来展示字词的出现概率

‍

‍

("C",65) 表示在本次统计中C语言出现65次

```python
# -*- coding: gbk -*-
from pyecharts import options as opts
from pyecharts.charts import Page, WordCloud

words = [  # 数据集
    ("Python", 100),
    ("C++", 80),
    ("Java", 95),
    ("R", 50),
    ("JavaScript", 79),
    ("C", 65)
]


def wordcloud() -> WordCloud:
    c = (
        WordCloud()
        # word_size_range: 单词字体大小范围
        .add("", words, word_size_range=[20, 100], shape='cardioid')
        .set_global_opts(title_opts=opts.TitleOpts(title="WordCloud"))
    )
    return c


wordcloud().render('./wordcloud.html')
```

‍

‍

#### 系列柱状图

‍

可以指示出一个对象的两个属性的情况,并且还能悬停查看.

```python
# -*- coding: gbk -*-
from pyecharts import options as opts
from pyecharts.charts import Bar
from random import randint


def bar_series() -> Bar:
    c = (
        Bar()
        .add_xaxis(['宝马', '法拉利', '奔驰', '奥迪', '大众', '丰田', '特斯拉'])
        .add_yaxis("销量", [randint(1, 20) for _ in range(7)])
        .add_yaxis("产量", [randint(1, 20) for _ in range(7)])
        .set_global_opts(title_opts=opts.TitleOpts(title="Bar的主标题",
                                                   subtitle="Bar的副标题"))
    )
    return c


bar_series().render('./bar_series.html')
```

‍

‍

#### 热力图

‍

```python

import random
from pyecharts import options as opts
from pyecharts.charts import HeatMap


def heatmap_car() -> HeatMap:
    x = ['宝马', '法拉利', '奔驰', '奥迪', '大众', '丰田', '特斯拉']
    y = ['中国', '日本', '南非', '澳大利亚', '阿根廷', '阿尔及利亚', '法国', '意大利', '加拿大']
    value = [[i, j, random.randint(0, 100)]
             for i in range(len(x)) for j in range(len(y))]
    c = (
        HeatMap()
        .add_xaxis(x)
        .add_yaxis("销量", y, value)
        .set_global_opts(
            title_opts=opts.TitleOpts(title="HeatMap"),
            visualmap_opts=opts.VisualMapOpts(),
        )
    )
    return c


heatmap_car().render('./heatmap_pyecharts.html')

```

‍

‍

‍

## seaborn

‍

### 介绍

‍

### 实例

‍

#### 简单热力图

‍

方格式的, 因为是图片,不如上面的pyecharts好看实用,就扔这里了.

```python
import seaborn as sns
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
# 生成数据集
data = np.random.random((6,6))
np.fill_diagonal(data,np.ones(6))
features = ["prop1","prop2","prop3","prop4","prop5", "prop6"]
data = pd.DataFrame(data, index = features, columns=features)
print(data)
# 绘制热力图
heatmap_plot = sns.heatmap(data, center=0, cmap='gist_rainbow')
plt.show()
```

‍

‍

#### pairplot图

‍

```python
from sklearn.datasets import load_iris
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn import tree

sns.set(style="ticks")
df02 = df.iloc[:, [0, 2, 4]]  # 选择一对特征
sns.pairplot(df02)
sns.pairplot(df02, hue="species")
sns.pairplot(df, hue="species")
plt.show()
```

‍

‍

‍

# 数分

‍

‍

## 工具

‍

### Series

‍

创建

```python
A = Series(['织田', '岛津', '武田', '今川', '北条', '毛利', '上杉', '伊达'], index=[1, 2, 3, 4, 5, 6, 7, 8])
B = Series(['洛阳', '大都', '建邺'], index=range(1, 4)) # 列表输入(Good): 索引直接加上了可以下标访问
```

‍

连接

```python
A = A.append(B)
A = concat([A, B])  # 可以重新设定参数等 左右拼接axis=1
```

‍

判断存在

```python
#in ,  数字逻辑不用'' ,需要用.values找值,不然一直是找不到
print('岛津' in A.values)
```

‍

删除

```python
# 删除: 按Index 和 按位置 和 按值 , 需要返回s
B = B.drop(1)
B = B.drop(B.index[1])
B = B['大都' != B.values]
```

‍

访问

```python
# 通过值访问index: 
print(A.index[A.values == '岛津'])

# 更改index : 整齐右移几位.
A.index = range(11, len(A) + 11)
```

‍

排序

```python
C = C.sort_index(ascending=False) # 改成降序
```

‍

‍

### Dataframe

‍

创建

‍

```python
df1 = DataFrame({'age': Series([24, 25, 26]), 'name': Series(['李田所', '浩二', '先辈'])})
```

‍

‍

增加

‍

```python
df1['体重'] = [114, 514, 1919]

# 合并两个, 生成新的索引:
df3 = df1.append(df2, ignore_index=True)
print(df3)
```

‍

访问

‍

```python
# 访问列
df['name'] 列名

# 访问行 
df[1:2]  切片

# 访问块 
df1.iloc[0:1, 0:2]   iloc双重切片[:,:], 前面是列, 后面是行

# 访问位置  
df1.at[1, 'name']  at[], 前面是行, 后面是列

# 访问指定列的值
df1[df1.columns[0:1]]

# 更改列名 :直接
```

‍

删除

```python
# .drop: 行索引 , 列索引, 需要axis
# df1 = df1.drop(1, axis=0)  # 0 == 行轴
# df1 = df1.drop('name', axis=1)  # 1== 列轴


del df1['name']  删除列
```

‍

‍

## 数据处理

‍

‍

导入文件:

read_table()   #TXT文件

‍

‍

导出文件:

to_csv()       #导出到csv

‍

‍

## 数据分析

‍

### 基本统计

又名描述性统计分析

‍

常用方法

size 计数(不需要括号)

sum 求和

mean 均值

var 方差

std 标准差

‍

‍

### 分组分析

划分为不同部分

‍

常用方法

groupby()

sum 求和

mean 均值

‍

‍

### 分布分析

由分析目的等/不等距分组

‍

‍

### 交叉分析

分析2个及上分组变量之间的关系,用交叉表形式进行对比分析

‍

常用方法

pivot_table()

‍

### 结构分析

在分组基础上计算各部分所占的比重来分析结构

‍

常用方法

df_pt.sum(axis)

df_pt.div(df_pt.sum(axis),axis)

‍

### 相关分析

研究现象之间是否有依存关系,看相关系数

‍

常用方法

DataFrame.corr()

Series.corr()

‍

‍

# 爬虫

‍

略

‍

示例

```python
import re
from urllib import request

# 爬虫爬取百度首页内容
data = request.urlopen("http://www.baidu.com/").read().decode()
# 分析网页,确定正则表达式
pat = r'<title>(.*?)</title>'
result = re.search(pat, data)
print(result)  #
result.group()  # 百度一下，你就知道
```

‍

‍

‍

‍

‍

‍
