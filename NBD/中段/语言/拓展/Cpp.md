--老大哥

‍

## Header

‍

### 快速方法

‍

### 我的简写

‍

### 版本控制

‍

### 环境相关

(使用Visual Studio时候)多子文件同时运行可能导致反复编译造成的多重调用(相当于生成了同名函数, 右键属性进行调整取消生成大概可以解决问题)

‍

‍

# 知识

‍

## 数学特性

‍

## 底层特性

‍

# 基础

‍

‍

‍

## 基本数型

‍

auto<sup>(auto关键字带上&号，不去除const语意。(不能通过引用改变这个变量))</sup>可以在声明变量<sup>（auto声明的变量必须要初始化且不能被声明为返回值）</sup>时由变量初始值的类型自动为此变量选择匹配的类型

‍

‍

### String

‍

方法

**一般**

* string str_name(n,'c')        初始化str为有连续n个字符c组成的串  
  string str_name(“value”)      直接用字符串赋值
* substr(pos,n)                 切片由pos开始的n个字符组成的字符串
* insert(pos,str)               插入字符/字符串
* erase(pos,n)                  删除从Pos开始n个字符

‍

**杂技**

* assign(str,[start],n)            [开始位置]前n个字符切片赋值新字符串  
      assign(n, char c)             n个字符c赋新字符串
* append(str,[start],n)            追加([从start开始的]str的前n个字符)

      append(int n, char c)            追加n个字符c
* (r)find(str/char* s/char c)        (right)查找str第一次匹配
* replace(pos,n,str)                  替换从pos开始的n个字符为字符/串str

‍

‍

### Array

‍

二维数组作为子函数形参时，第一维长度可以省略，第二维长度任何情况下都不能省略

‍

‍

## 基本量

‍

### 变量

‍

### 常量

define price=30(宏定义)   Const int a(常变量)

‍

### 运算符

‍

#### 逗号

​`表达式1, 表达式2`​

先求解表达式 1，再求解表达式 2。整个逗号表达式的值是表达式 2 的值

‍

#### 三元

​`条件表达式 ? 表达式1 : 表达式2`​

如果“条件表达式”为true，则整个表达式的值就是表达式1，忽略表达式2

‍

‍

## 基础语法

‍

### 输入输出

‍

cin    需要include <iostream>

getline(数组名，长度)    可以输入包含空格、TAB字符的字符串

get     接收键盘输入的一个字符，空格、回车、TAB等都可以接收

scanf    格式化

```cpp
while ((c = getchar()) != '\n')
    if (c >= 'a' && c <= 'z' || c >= 'A' && c <= 'Z')
```

‍

cout    需要include <iostream>

printf    格式化

‍

‍

### 格式化

‍

#### 简单

‍

**printf/scanf**  
     **%d    %**​**f<sup>(lf:double)</sup>**    **%s**  **m**  ** n**

**m    位宽**  
    实际数据位宽若大于m，按实际位宽输出; 实际数据位宽小于m<sup>(-m  左对齐，右补空格)</sup>时，右对齐，左补空格

 **.n    显示精度/长度**  
    输出n位小数 / n个字符

‍

**小数位控制**

```cpp
#include iomanip
cout<<setiosflags(ios::fixed)<<setprecision(n)
```

‍

**Set +**

* Fill（c）    填充字符
* Precision（n）    精度
* iosflags（ios：：fixed）    固定小数位数
* w（n）  设置字符宽度为几位

‍

‍

#### 精细控制

‍

printf() %[标志][最小宽度][.精度][类型长度]类型

‍

##### 类型（type）

‍

|字符|对应数据类型|含义|
| ------| ---------------| ----------------------------------------------------------------------------------|
|d/i|unsigned int|输出十进制有符号32bits整数, i为老式写法|
|o|unsigned int|无符号8进制整数(无前缀0)|
|u|unsigned int|无符号10进制整数|
|x/X|float(double)|无符号16进制整数，x对应的是abcdef，X对应的是ABCDEF(不输出前缀0x)|
|f/lf|float(double)|单精度浮点数用f,双精度浮点数用lf(printf可混用，但scant不能混用)|
|F|float(double)|与f格式相同，只不过infinity和nan输出为大写形式。|
|e/E|float(double)|科学计数法，使用指数(Exponent)表示浮点数，此处"e"的大小写代表在输出时"e"的大小写|
|g|float(double)|根据数值的长度，选择以最短的方式输出，%f或%e|
|G|float(double)|根据数值的长度，选择以最短的方式输出，%f或%E|
|c|char|字符型。可以把输入的数字按照ASCII码相应转换为对应的字符|
|s|char*|字符串。输出字符串中的字符直至字符串中的空字符（字符串以空字符'\0'结尾)|

​​​

‍

##### 标志（flags）

‍

‍

|字符|说明|
| :-----: | ------------------------------------------------|
|-|结果左对齐,右边空格|
|+|输出符号(+/-)|
|space|输出值为正时加上空格, 为负加上负号|
|#|type为o,x,X时增加前缀0,0x,0X|
|0|将输出前面补0直到占满指定列宽为止(不可搭配"-")|

‍

‍

e.g.

```cpp
printf("%5d\n",1000); 				//默认右对齐,左边补空格
printf("%-5d\n",1000); 				//左对齐,右边补空格

printf("%+d %+d\n",1000,-1000);		//输出正负号

printf("% d % d\n",1000,-1000);		//正号用空格替代，负号输出

printf("%x %#x\n",1000,1000);		//输出0x

printf("%.0f %#.0f\n",1000.0,1000.0)//当小数点后不输出值时依然输出小数点

printf("%g %#g\n",1000.0,1000.0);	//保留小数点后后的0

printf("%05d\n",1000);				//前面补0
```

‍

##### 宽度（width）

    int: 指定宽度

    *: 在printf的参数列表里给出

​​

##### 精度（.precision）

‍

十进制整数。  

(1)对于整型(d,i.o,u,x,X) ,precision表示输出的最小的数字个数，不足补前导零，超过不截断。  

(2)对于浮点型(a,A, e,E, f ) ,precision表示小数点后数值位数，默认为六位，不足补后置0，超过则截断。  

(3)对于类型说明符g或G，表示可输出的最大有效数字。  

(4)对于字符串(s), precision表示最大可输出字符数，不足正常输出，超过则截断。  
precision不显示指定，则默认为0

‍

可以以星号代替数值，类似于width中的*，在输出参数列表中指定精度。

​​

‍

##### 类型长度（length）

```cpp
printf("%hhd\n",'A');				//输出有符号char
printf("%hhu\n",'A'+128);			//输出无符号char
printf("%hd\n",32767);				//输出有符号短整型short int
printf("%hu\n",65535);				//输出无符号短整型unsigned short int
printf("%ld\n",0x7fffffffffffffff);	//输出有符号长整型long int
printf("%lu\n",0xffffffffffffffff);	//输出有符号长整型unsigned long int
```

‍

### 流程控制

‍

foreach遍历:  for (auto a : box)

goto：需要在要跳转语句的首行打一个标签 (方便跳出循环)

‍

‍

### 迭代器

‍

string::iterator i  **string迭代器**

```cpp
vector<string>::iterator p;
for ( p = v6.begin();  p != v6.end();  p++)  cout << *p<<" ";
```

‍

‍

### 指针

指针&地址转换技巧

```java
Time a, * p;
Time &b = a;   //b是a的引用。
p = &a; 
    p->hour       /     (*p).hour      /      a.hour    /     b.hour     三个都是等价的
```

‍

### 结构体

‍

结构体内报没有初始化的问题: 加上一个大括号{}就可默认赋值

‍

原本的struct写法示例,现在都是简写

```cpp
struct Slinklist{};
typedef struct Slinklist* SLL;
typedef struct Slinklist snode;
```

‍

### 系统交互

‍

system("pause")暂停

system("cls"); 清屏

‍

‍

## 类型转换

‍

一个0-9字符转换<sup>（a-97 ，A-65  差距：32）</sup>对应int，就要减去48; 反之，一个0-9值转换为对应的字符，要加上48

‍

#### String->其他<sup>（负号自己拼接）</sup>

‍

sstream

```cpp
stringstream s("666")  (stringstream s;s << str;)   //一个s用来进行插入提取
s >> int_num;      int类提取>（流输出到int型数据）
```

‍

==string 转 char*==

​`const char* cstr = thestr.c_str();`​

‍

#### 其他->string

‍

to_string

​`cout << std::to_string(i)`​

‍

sstream

```cpp
stringstream s;
s << i
cout << s.str()
```

‍

==char* 转 string==

```cpp
	char* p = "it";
	string str(p);
```

‍

## 类

‍

## 异常

‍

# 高级

‍

## 内存分配

‍

1. 一定要注意new 和delete的配对，new&delete用于为一个实体分配内存，而new[] & delete[]用于为数组分配内存。
2. 释放NULL指针（即空指针）是没有问题的
3. 指针被free或者delete后，一定要置为null，没有置为null的指针常称为野指针

‍

‍

==malloc &amp; free==

例:

​`(int`​ *​`) malloc (sizeof(int)`​* ​`128)`​ 计算int型的128个整形存储单元,并强制转换为int指针,后给对应指针

```cpp
short* p=(short*) malloc (len * sizeof(short)); 
//由于malloc返回类型是void*，用其返回值对其他类型指针赋值时，必须使用显示转换。
//同时，malloc参数是个无符号整数，其仅仅关心申请字节的大小，并不管申请的内存块中存储的数据类型。
//因此，申请内存的长度须由程序员通过“长度*sizeof(类型)”方式
//如果操作系统无法向`malloc`提供更多的内存，`malloc`就返回一个`NULL`指针
```

‍

==new &amp; delete==

```cpp
//动态申请了大小为int型的内存，将这块区域初始化为8，把该区域的首地址赋值给pNum。
int *pNum=new int(8);   
//delete指令会释放动态申请的内存块，但不会删除指针本身，还可以将指针重新指向另一块内存区域。  
//new无法对动态申请的数组存储区进行初始化
```

‍

## 模板

函数模板 template<typename.T> 仅函数类型不同

‍

‍

# 数据结构

‍

    STL<sup>(（Standard Template Library）标准模板库)</sup>是一个工业强度的一种类型参数化<sup>（基于模板（template））</sup>的方式实现的高效<sup>（是ANSI/ISO C++标准中最新的也是极具革命性的一部分。该库包含了诸多在计算机科学领域里所常用的基本数据结构和基本算法。为广大C++程序员们提供了一个可扩展的应用框架，高度体现了软件的可复用性`）</sup>C++程序库。特点是数据结构和算法的分离<sup>（sort()函数是完全通用的，你可以用它来操作几乎任何数据集合）</sup>和不是面向对象<sup>（为了具有足够通用性，STL主要依赖于模板而不是封装，继承和虚函数（多态性）——OOP的三个要素）</sup>以及体现了泛型化程序设计的思想<sup>（引入了诸多新的名词，比如像需求（requirements），概念（concept），模型（model），容器（container））</sup>, 容纳于C++标准程序库<sup>（（C++ Standard Library））</sup>中, 使用函数指针和回调函数来实现仿函数,有三种顺序容器适配器：queue(FIFO队列)、priority_queue(优先级队列)、stack(栈)

    等于不用自己造轮子的内置数据结构: 以下为主要组成:

‍

 **（1）**​**序列式容器<sup>（（Sequence containers））</sup>**    每个元素都有固定位置，和元素值无关

‍

Vector：

元素置于一个动态数组中加以管理，随机存取，数组尾部添加或移除元素非常快速。但是在中部或头部安插元素比较费时；

‍

Deque：

可以<sup>（double-ended queue”的缩写）</sup>随机存取元素（用索引直接存取），数组头部和尾部添加或移除元素都非常快速。但是在中部或头部安插元素比较费时；

‍

List：

双向链表，不提供随机存取，在任何位置上执行插入或删除动作都非常迅速

‍

‍

 **（2）**​**关联式容器<sup>（（Associated containers））</sup>**    元素位置取决于特定的排序准则，和插入顺序无关

‍

Set/Multiset：

内部的元素依据其值自动排序，Set内的相同数值的元素只能出现一次，Multisets内可包含多个数值相同的元素，内部由二叉树实现，便于查找；

‍

Map/Multimap：

Map的元素是成对的键值/实值，内部的元素依据其值自动排序，Map内的相同数值的元素只能出现一次，Multimaps内可包含多个数值相同的元素，内部由二叉树实现，便于查找；

‍

‍

## vector

‍

#### Tips

    所谓vector动态增加大小是新开一块更大的内存空间，然后拷贝新空间.一旦引起空间的重新配置，指向原vector的所有迭代器就都失效了

‍

#### 基操

‍

==基础==

‍

vector(nSize,[t])        创建vector元素个数nSize,[值均为t]  
vector(begin,end)        复制[begin,end)区间内元素到vector中

‍

push_back(k)                            尾插元素  
insert(iterator pos,[count],ele)     位置pos插入[count个]元素  
assign(n,x)                        设置向量中第n个元素的值为x  

‍

front()              第一个数据元素  
back()              最后一个数据元素  
begin()                返回向量头指针，指向第一个元素  
end()                    返回向量尾指针，指向最后一个元素的下一个位置  
top()                     返回栈顶元素  
at(int idx) / []         索引idx

‍

eraser(iterator first,iterator last)    删除容器中[first,last)元素  
erase(iterator it)        删除向量中迭代器指向元素  
clear()                删除容器中所有元素  
pop_back()        删除向量中最后一个元素  
clear()        清空向量所有元素

‍

v.size()  
v.capacity()             容量  
max_size()                查看最大可允许的vector元素数量  
empty()                    判断向量是否为空  
resize(num)                   重新指定<sup>（若容器变长，则以默认值/elem值填充新位置。如果容器变短，则末尾超出容器长度的元素被删除）</sup>容器的长度为num

‍

==杂技==

‍

sort(v.begin(),v.end()，cmp)                cmp排序<sup>（默认由小到大）</sup>

‍

reverse(v.begin(),v.end())                反转任意段

‍

swap(vector&)                        交换两个同类型向量的数据

‍

rbegin()                            反向迭代器，指向最后一个元素  
rend()                            反向迭代器，指向第一个元素之前的位置

‍

#### 实例

‍

‍

## List

‍

### Tips

#include <list>

‍

### 基操

类似vector

‍

### 实例

‍

‍

## Map

‍

#### Tips

==依赖==

```cpp
#include <map>
using namespace std;
std::map<std::string, int>myMap;
std::map<std::string, int>myMap{ {"C语言教程",10},{"STL教程",20} };
```

‍

#### 基操

类似vector

map[]                                添加 / 查找

erase(K);  
erase(cmap.begin(),cmap.end())        删除指向元素

map<string,int>::iterator key           迭代器

‍

#### 实例

‍

```cpp
for (auto iter = mymap.begin(); iter != mymap.end(); ++iter) { //遍历访问
        cout << iter->first << " " << iter->second << endl;
    } //auto ==map<string, string>::iterator iter
```

‍

# 附录

‍

## 手搓数构

自己造轮子的Cpp数构

‍

### 数据表

‍

定义

```cpp
typedef struct SSqlist { //Simple siquence list 基础顺序数据表
	double data[MaxSize];
	int length;
}*SSQ, qlist; //取个别名

typedef struct MSqlist { //Movable siquence list 动态顺序数据表
	double* data;
	int Maxlength, length;
}*MSQ, mlist; //取个别名
```

‍

初始化

```cpp
void SSQ_new(SSQ& L) {  //简单初始化顺序表
	L = new qlist;
	L->length = 0; //我是用长度标记表尾,代表有几个元素
}

void MSQ_new(MSQ& L) {  //简单初始化动态数据表
	L = new mlist;
	L->data = new double[MaxSize];  //C:  L->data=(double*)malloc(sizeof(double) * MaxSize);
	L->length = 0;
	L->Maxlength = MaxSize;
}
```

‍

==MSQ动态扩容==

```cpp
void MSQ_Addsize(MSQ& L, int len) { //动态增加长度,原理是申请新空间然后改变data指针指向
	double* p = L->data;
	L->data = new double[MaxSize];  //申请一片新空间
	for (int i = 0; i < L->length; i++)
		L->data[i] = p[i];     //复制数据-开销大
	L->Maxlength = L->Maxlength + len; //增加最大热容量
	free(p);
}
```

‍

写入

```cpp
void SSQ_insert(SSQ& L, int sit, double x) {  //数据表已有数据中的插入, 双判: 位置溢出,表格溢出,这里的x是第几个的意思
	if (sit<1 || sit>L->length + 1 || L->length == MaxSize)  //sit代表位置of第几位的插入操作的; 如果是动态的注意最大上限L->length >= L->Maxlength
		return;
	for (int j = L->length; j >= sit - 1; j--)
		L->data[j + 1] = L->data[j];
	L->data[sit - 1] = x;  //插入到数据中
	L->length++;
} //注意,采用Inlist初始化的list,判定无法插入的条件更为宽松,同时打印的时候也可以无脑全部打印.
```

‍

删除

```cpp
for (int j = sit - 1; j < L->length; j++)
		L->data[j] = L->data[j + 1];
```

‍

查找

```cpp
while (i <= L->length && L->data[i] != x)
	i++;
	if (i > L->length) return -1;
	else return i + 1;
```

‍

比较

```cpp
int SSQ_Compare(SSQ L1, SSQ L2) {  //特种函数:比较大小(类似str的方法);
	int s2 = 0;	int i = 0;	int s1 = 0;
	while (i < L1->length && i < L2->length && L1->data[i] == L2->data[i])
		i++;
	s1 -= i;	s2 -= i;
	if (s1 == s2 && s1 == 0) return 0;
	else if (s1 == 0 && s2 > 0 || s1 > 0 && s2 > 0 && L1->data[i] < L2->data[i])
		return -1;
	else return 1;
}
```

‍

排序

```cpp
void SSQ_sort(SSQ& L) { //降序排列,产生新的qlist,并更改L的指向.
	SSQ N;
	SSQ_new(N);
	double baddest = -114514;
	int sit = 0;
	while (L->length > 0) {
		baddest = -114514;
		for (int i = 0; i < L->length; i++) {
			if (L->data[i] > baddest) {
				baddest = L->data[i];
				sit = i + 1;
			}
		}
		SSQ_add(N, baddest);
		SSQ_del(L, sit);
	}
	L = N;
}
```

‍

‍

### 链表

==定义+初始化==

```cpp
typedef struct Slinklist{
	double data;
	Slinklist* next;
}*SLL, snode; 
```

```cpp
void SLL_new(SLL& L){
	L = new snode;
	L->next = NULL; //L->before = NULL; //DLL
	L->data = -1; //将头结点初始化.
}
```

‍

==头尾插入+定点前后插==

```cpp
void SLL_add(SLL& L, double data) { //尾插新项
	SLL p = L;//遍历指针p
	snode* N = new snode;
	N->next = NULL; //N->before = NULL; //DLL
	N->data = data;//新节点装填完毕

	while (p->next != NULL)//有头结点就不需要考虑这个问题
		p = p->next;
	p->next = N; //N->before = p; //DLL
}

void SLL_insert(SLL& L, double data) { //头插新项,考虑原来是空的情况
	snode* N = new snode;
	N->next = NULL;
	if (L->next == NULL) {
		L->next = N;
		N->data = data; //N->before = L; //DLL
		return;
	}
	else {
		N->next = L->next;
		N->data = data; //N->next->before = N; //DLL
		L->next = N; //N->before = L; //DLL
		return;
	}
}
```

```cpp
void SLL_fin(SLL p, double data) { //给定指针节点的前插操作,O1的偷天换日方法
	SLL s = new snode;
	s->next = p->next; //s->before = p; //DLL
	p->next = s;
	//data
}

void SLL_bin(SLL p, double data) { //给定指针节点的后插操作
	SLL s = new snode;
	s->next = p->next; //s->before = p;
	p->next = s;
        //data
}
```

‍

==删除==

```cpp
void SLL_Del(SLL& L, SLL& p) {
//删除一个节点p,当然也可以用偷天换日,把后面的节点覆盖到这里来,如果是表尾就delete
	SLL q = L; //移动指针,跑到p节点的前一个节点

	while (q->next != p)
		q = q->next;
	if (p->next != NULL)
		q->next = q->next->next;
	else
		q->next = NULL;
}

void SLL_Delete(SLL& p) { //偷天换日
	if (p->next == NULL)
		delete(p);
	else {
		SLL q = p->next;
		p->data = p->next->data;
		p->next = q->next;
		delete(q);
	}
}

void DLL_Del(DLL& L, DLL& p) {//删除一个节点p,同样需要两个指针
	DLL q = L; //移动指针,跑到p节点的前一个节点

	while (q->next != p)
		q = q->next;
	if (p->next != NULL) {
		q->next = p->next; //p"自动"移到了下一节点
		p->before = q;
	}
	else
		q->next = NULL;
}
```

‍

==长度==

```cpp
for (p; p != NULL; p = p->next)		length++;
```

‍

==查找+定位==

```cpp
SLL SLL_Get(SLL L, int i) {  //简单按序号查找第i位指针
	SLL p = L;  //遍历指针
	int j = 0;
	while (p->next != NULL && j < i){
		p = p->next;
		j++;
	}
	if (j == i) return p;
	else return NULL;
}

SLL SLL_Loc(SLL L, double x) {  //简单定位元素
	SLL p = L;
	while (p->next != NULL && p->data != x)
		p = p->next;
	return p;
}
```

‍

==连接链表==

```cpp
SLL SLL_Combine(SLL& A, SLL& B, SLL& L) { //最简单链接AB链表到L中去
	SLL p = A; //a1移动到末尾A 
	SLL q = B; //q去链接b中元素到L余下,保证A,B不受影响
	SLL t = L; //t去遍历L.
	SLL_new(t);
	while (p->next != NULL) {
		snode* n = new snode;//新节点,每次都要申请空间
		n->next = NULL;
		p = p->next;
		n->data = p->data;
		t->next = n;  //赋值构造得到的节点
		t = t->next;
	}
	while (q->next != NULL) {//p操作同上
	}
	return L;
}
```

‍

==去重==

```cpp
void SLL_Remove(SLL& L) { //简单的去重算法,删除连续的后一个
	SLL q, p;
	q = L;
	if (L->next != NULL){
		p = L->next;
		while (p != NULL && q != NULL){
			if (p->data == q->data){
				SLL_Del(L, p);
				p = q->next;
			}
			p = p->next;
			q = q->next;
		}
	}
	else return;  //就一个去什么重啊?
}
```

‍

==逆转==

```cpp
void SLL_Reverse(SLL& L) {  //头插法简单逆转链表
	SLL N, p = L->next;
	SLL_new(N);
	while (p != NULL) {
		SLL_insert(N, p->data);
		p = p->next;
	}
	L = N;
}
```

‍

==切片==

```cpp
SLL SLL_Slice(SLL& p, SLL& q) {  //返回一个新链表,由p,q节点以及之间的节点构成
	SLL L;
	SLL_new(L);
	while (p != q) {
		SLL_add(L, p->data);
		p = p->next;
	}
	SLL_add(L, p->data);
	return L;
}
```

‍

==降序排列==

```cpp
SLL SLL_sort(SLL& L) {   //降序排列链表,返回个新链表
	SLL p = L->next;	SLL N;    	SLL s;
	SLL_new(N);
	int times = SLL_Length(L);//判定需要找最大值的次数

	double maxpower = 0;
	while (times > 0) {
		maxpower = -114514;
		while (p != NULL) {
			if (p->data > maxpower) {
				maxpower = p->data;
				s = p;
			}
			p = p->next;
		}
		SLL_add(N, maxpower);
		SLL_Del(L, s);
		times--;
		p = L->next;
	}
	return N;
}
```

‍

‍

### 串

‍

==初始化==

```cpp
typedef struct SS {
	char ch[MaxSize];
	int length;
}*SR;

void SS_new(SR& S) {
	S = new SS;     //分配空间
	S->length = 0;
}

void SS_create(SR& S, char chs[]) { //用字符数组/字符串string创造字符串 
	int i = 0;
	while (chs[i] != '\0'){	//循环，将字符串常量的值赋值给S
		S->ch[i] = chs[i];
                i++;
	}
	S->length = i;
}
```

‍

==复制==

```cpp
S->ch[i] = T->ch[i];S->length = T->length;
```

‍

==连接,切片,比较==

```cpp
void SS_concat(SR& S, SR& Q) {   //连接Q到S
	int j = 0;
	int i = S->length;
	for (j; j < Q->length; i++, j++)
		S->ch[i] = Q->ch[j];
	S->length += Q->length;
}

void SS_Sub(SR& S, SR& T, int pos, int len) {//用T返回S的第pos个字符起长度为len的子串
	if (pos + len - 1 > S->length) return; //越界.
	for (int i = pos; i < pos + len; i++)
		T->ch[i - pos + 1] = S->ch[i];  //这步高明,可以实现i-pos
	T->length = len;
}

int SS_Compare(SR& S, SR& T) { 
        // 这个是COm,自己实现的比较算法,返回-1,0,1. 当然可以用作那个验证相等,返回0
	for (int i = 1; i < S->length && i < T->length; i++)
		if (S->ch[i] != T->ch[i]) return S->ch[i] - T->ch[i];
	return S->length - T->length;
}
```

‍

==查找==

```cpp
int SS_index(SR& S, SR& T) {  // 最简单的朴素算法,
	int i = 1, n = S->length, m = T->length;    SR sub;
	SS_new(sub); //创建暂存子串.
	while (i < n - m + 1) {
		SS_Sub(sub, S, i, m);
		if (SS_Compare(sub, T) != 0) i++;
		else return i;   //返回位置
	}
	return -1; //找不到了.
}
```

‍

‍

### 栈

‍

```cpp
typedef struct Sqstack {//顺序栈定义
	double data[MaxSize];
	int top;
}*SST;

void SQ_new(SST& S) { //初始化:设置栈顶指针为-1,接下来会指向要插入的位置
	S = new Sqstack;
	S->top = -1;
}
```

```cpp
if (S->top == -1) return true; //判断栈空
```

```cpp
void SQ_push(SST& S, double x) { //省略设置返回值
	if (S->top == MaxSize - 1) return;
	S->data[++S->top] = x;
}
void SQ_pop(SST& S) {
	S->top--;
}
```

‍

### 队列

‍

==初始化==

```cpp
//创建一个循环队列--避免空间的浪费,指针中间空一格,同时指向一个则是空,中间差一个就是满
typedef struct SqQueue {
	double data[MaxSize];
	int front, back; //队头队尾指针,rear=back,代表插入的队尾,是不断前进的,别搞翻了哈
}*SQE, que; //重写名称
```

```cpp
void SQE_new(SQE& Q) { //初始化指针
	Q = new que;//申请
	Q->back = Q->front = 0;
}
```

‍

==出入==

```cpp
void SQE_in(SQE& Q, double x) { //入队操作,设置bool返回是否成功挺好
	if (SQE_isFull(Q)) return;  //判断满的条件:避免假满,则使用取模运算
	Q->data[Q->back] = x;
	Q->back = (Q->back + 1) % MaxSize; //空间化为环形
}
void SQE_out(SQE& Q) { //出队
	if (Q->back == Q->front) return;//啥都没有
	cout << Q->data[Q->front];  //简单输出
	Q->front = (Q->front + 1) % MaxSize;
}
```

‍

==判满==

```cpp
if (Q->back == Q->front) return true; //一般的头碰尾
```

```cpp
if ((Q->back + 1) % MaxSize == Q->front) return true; //队尾的下一个就是fr,取mod就是
```

‍

### 树

‍

#### 基操

‍

==初始化==

```cpp
typedef struct BTnode {
	double data;
	struct BTnode* L, * R;
}btnode, * TR;  //TR= tree

typedef struct BBTnode {
	double data;
	struct BBTnode* C, * B; //"第一个孩子"和ta右兄弟Child_first,Brother_right
}bbtnode, * BT;  //BT =brother tree

typedef struct TBTnode {
	double data;
	struct TBTnode* L, * R;
	int ltag, rtag; //左右线索标志,0,1代表是有元素还是没有(0),没有则用来存放前驱后继线索
}tbtnode, * TB;   //TT =thread(tag) tree
```

‍

==通用==

```cpp
void TR_new_root(TR& T) { //插入根节点(这里设置为无意义
	T = new btnode; //申请节点空间
	T->data = 114514;  //好臭的树
	T->L = T->R = NULL;
}

void TR_new(TR& T) { //树的初始化
	TR_void_tree(T);
	TR_new_root(T);
}

void TR_SERT(int u, TR& p, double x) {  //参数列表第一个是判断,对应节点插入 , 0左1右
	TR N = new btnode;
	N->data = x;
	N->L = N->R = NULL;
	if (u == 1) p->R = N;
	else p->L = N;
}

void TR_delete(TR& p, int L_or_R) {  //传入要删除的节点指针,判断进行删除的是左孩子还是右孩子
	if (L_or_R == 0) { //0=Left;
		delete(p->L);
		p->L = NULL;
	}
	else {//
	}
}


int TR_Hight(TR T) { //递归求高度
	if (T == NULL) return 0;
	else return max(TR_Hight(T->L), TR_Hight(T->R)) + 1;
}

int TR_Depth(TR T, TR p) { return  TR_Hight(T) - TR_Hight(p) + 1; } //复合求"节点"深度, 根节点和要求的节点
```

==BT==

```cpp
void BT_Child_insert(BT& p, double x) {  //孩子兄弟树的手动插入一个孩子节点/兄弟节点
	BT N = new bbtnode;
	N->data = x;
	N->C = N->B = NULL;
	p->C = N;
}
```

==TB==

```cpp
void TB_lsert(TB& p, double x) {  //对应节点之后的左插/右插
	TB N = new tbtnode;
	N->data = x;
	N->L = N->R = NULL;
	p->L = N;
	p->ltag = 0;
}
```

‍

#### 遍历

‍

​`TR_visit(T);//根TR_PreOrder(T->L);//左TR_PreOrder(T->R);//右`​

```cpp
void TR_count(TR& T) {   //中序遍历计算节点个数
	if (T != NULL) {
		TR_InOrder(T->L);     //左
		Node_Counter++;  //计数器
		TR_InOrder(T->R);   //右
	}
	else {
                cout << "二叉树共有 " << Node_Counter << "个节点\n";
		Node_Counter = 0;
	}
}

void TR_LevelOrder(TR& T) { //print:层序遍历,需要用到队列
	cout << "\n\n层序遍历T: \n\n";
	LQE Q;
	LQE_new(Q);
	TR p = T; //遍历节点
	LQE_in(Q, T); //根节点入队.
	while (!LQE_isVoid(Q)) {
		LQE_out(Q, p);  //队头出队
		TR_visit(p);  //访问出队节点,这里就实现了把孩子入队的过程.
		if (p->L != NULL) LQE_in(Q, p->L);
		if (p->R != NULL) LQE_in(Q, p->R);
	}
}

void TR_Print(TR& T) {  //直接就在这里给我格式化输出了,要求: 每层换行,直观展示...
	cout << "\n\n高级层序遍历T: \n\n";
	int H = TR_Hight(T); //保存现在的树的高度
	TR p = T;
	LQE Q;	LQE_new(Q);	LQE_in(Q, T); //根节点入队.

	while (!LQE_isVoid(Q)) {//这里判断要出队访问的对象的层数与当前的保存的对象层数是否一样.
		LQE_out(Q, p);  //队头出队
		if (TR_Hight(p) != H) {
			H = TR_Hight(p);
			cout << endl;
		}
		TR_visit(p);  //访问出队节点,这里就实现了把孩子入队的过程.
		if (p->L != NULL) LQE_in(Q, p->L);
		if (p->R != NULL) LQE_in(Q, p->R);
	}
}
```

‍

‍

#### 线索

‍

##### 构建

```cpp
void TB_Pre_Create(TB& T) { //先序线索化二叉树
	pre = NULL;
	if (T != NULL) 	TB_PreOrder(T);
}
```

```cpp
TB pre = NULL;   //全局变量来指向当前访问的节点的前驱.
//构建线索二叉树: 直接使用visit来实现.
void TB_visit(TB q) {
	if (q->L == NULL) { //左子树位子空了,当作前驱线索指向前面的pre;
		q->L = pre;
		q->ltag = 1;
	}
	if (pre != NULL && pre->R == NULL) { 
        //前面有人,并且右子树是空的,当作它的后继线索指向现在的对象.
		pre->R = q;          //建立前驱节点的后继线索.
		pre->rtag = 1;
	}
	pre = q;  //让前驱跟上q节点.
}
```

‍

##### 操作

‍

==Pre==

```cpp
TB TB_Pre_FirstNode(TB p) {
	if (p->ltag == 0) 
            return p->L;
	else return p->R;
}

TB TB_Pre_NextNode(TB p) { //前序遍历p的后续:左孩子>右孩子
	if (p->rtag == 0)
            return TB_Pre_FirstNode(p);
	else return p->R;
}

void TB_Thread_PreOrder(TB& T) {  //特种前序主函数
	for (TB p = T; p != NULL; p = TB_Pre_NextNode(p))
		TB_Normal_visit(p);
}
```

‍

==In==

```cpp
TB TB_In_FirstNode(TB p) {  //找中序线索二叉树的p的后继,并且通过这实现O(1)时间复杂度的非递归中序遍历中序线索二叉树 -01
	while (p->ltag == 0) p = p->L;  //找最左下节点
	return p;
}

TB TB_In_LastNode(TB p) {  //找中序线索二叉树的p的前驱,并且通过这实现O(1)时间复杂度的非递归中序反向遍历中序线索二叉树 -02
	while (p->rtag == 0) p = p->R;  //找最右下节点
	return p;
}

TB TB_In_NextNode(TB p) {  //直接找后继 / 推演找后继
	if (p->rtag == 0) return TB_In_FirstNode(p->R);//根据中序展开的推演结果,后继不是rtag指向的目标就是右子树中的最左下节点(左根右)
	else return p->R;  //p.rtag==1 ->有线索化了.
}

TB TB_In_PreNode(TB p) {  //直接找前驱 / 推演找前驱
	if (p->ltag == 0) return TB_In_LastNode(p->L);//根据中序展开的推演结果,前驱不是ltag指向的目标就是左子树中的最右下节点(左根右)
	else return p->L;  //p.ltag==1 ->有线索化了.
}

void TB_Thread_InOrder(TB& T) {  //特种中序主函数
	for (TB p = TB_In_FirstNode(T); p != NULL; p = TB_In_NextNode(p))
		TB_Normal_visit(p);
}

void TB_Thread_Rev_InOrder(TB& T) { //特种反向中序主函数
	for (TB p = TB_In_LastNode(T); p != NULL; p = TB_In_PreNode(p))
		TB_Normal_visit(p);
}
```

‍

==Post==

```cpp
TB TB_Post_LastNode(TB p) {
	if (p->rtag == 0) return p->R; //有右孩子, 前驱就是右孩子,否则有左孩子就是左孩子
	else return p->L;
}

TB TB_Post_PreNode(TB p) { //前序遍历p的后续:左孩子>右孩子
	if (p->ltag == 0)return TB_Post_LastNode(p);
	else return p->L;
}

void TB_Thread_Rev_PostOrder(TB& T) {  //特种前序主函数
	for (TB p = T; p != NULL; p = TB_Post_PreNode(p))
		TB_Normal_visit(p);
}  //整体和先序的找后续一样,两个极端.
```

‍

‍

### 图

‍

#### 基操

‍

==定义==

```cpp
//定义的顶点结构:
typedef string DataType;  //指定string字符串为表内的数据对象
typedef struct VertexType { //顶点结构
	int no{}; //N.O. 顶点的编号
	DataType data; //顶点包含的数据 
}VT;

//邻接矩阵 MG
typedef struct MGraph {
	VT VG[MaxV_num];  //顶点表,类型为自定义VT
	int EG[MaxV_num][MaxV_num]{}; //打上了{}代表已经提前都是0的邻接矩阵,0,1或是Wij和0或Infinity
	int Vnum{}; //顶点数
	int Enum{}; //边数 ==Anum
}MG, * AMG;

//邻接表 LG
typedef struct EdgeNode { //表(后继)结点的类型
	int no{};   //代表的顶点序号
	DataType data;  //权值or信息
	struct EdgeNode* next{}; //指向下一个节点
}ED, * EN;
typedef struct VertexNode { //头结点类型
	VT data; //头结点
	EN First_Edge{}; //指向第一个表结点的指针
}Vnode;
typedef struct Algraph { //邻接表类型
	Vnode List[MaxV_num];  //表的本体
	int Vnum{}; //顶点数
	int Enum{}; //边数
}LG, * ALG;

//Queue存放顶点对象VT;
typedef struct Queue {
	VT node[MaxSize]{};
	int front{};
	int back{}; //队头队尾指针,rear=back,代表插入的队尾,是不断前进的
}*QU, que;
```

‍

==构造: 输入式/数组式==

```cpp
void MG_input(MG& M) { //输入邻接矩阵的边的信息(0代表第一个点)
	cin >> M.Enum >> M.Vnum; //输入邻接矩阵的顶点数和边数
	int outone, inone;
	for (int i = 0; i < M.Enum; i++) { //输入边的信息
	inp:cin >> outone >> inone; //输入行,列(可以只搞上三角)
		if (outone == inone) {//输到对角元素上了,不行,自己没有自己
			cout << "\n错误!不能指向自己!\n";
			goto inp;
		}
		M.EG[outone][inone] = 1; //对应的值赋1;
	}
	for (int i = 1; i < M.Vnum; i++) // 为邻接矩阵下三角赋值-简单的镜面对称
		for (int j = 0; j < i; j++)
			M.EG[i][j] = M.EG[j][i];
	for (int i = 0; i < M.Vnum; i++) {
		M.VG[i].no = i;  //顶点元素赋值
	}
}
```

```cpp
void MG_EZnew(MG& M) { // 简单生成一个邻接矩阵5-5 (eg), 也可以有一个临时数组
	M.Enum = M.Vnum = 5;  //分别对5个顶点的出度赋值,然后后面景象到入度.无向图.
	M.EG[0][4] = 1;
	M.EG[0][2] = 1;
	M.EG[1][2] = M.EG[1][3] = 1;
	M.EG[2][0] = M.EG[2][1] = M.EG[2][3] = 1;
	M.EG[3][2] = M.EG[3][1] = 1;
	M.EG[4][0] = 1;

	//还有顶点的名字赋值:no项:
	for (int i = 0; i < M.Vnum; i++) {
		M.VG[i].no = i;  //顶点元素赋值
	}
}
```

‍

==转换为邻接表==

```cpp
void LG_Create_from_MG(ALG& lg, MG mg) {//由图的邻接矩阵创建图的邻接表
	EdgeNode* p; //指向表结点的临时指针
	lg = new LG; //新建LG邻接表

	for (int i = 0; i < mg.Vnum; i++) //为邻接表所有头结点的指针域赋初值
		lg->List[i].First_Edge = NULL;
	for (int i = 0; i < mg.Vnum; i++)
		for (int j = mg.Vnum - 1; j >= 0; j--)
			if (mg.EG[i][j] != 0)  //这里有条边的话.
			{   //若顶点i、j之间有边
				p = new EdgeNode; // 新表结点
				p->no = j;
				p->data = mg.EG[i][j];
				p->next = lg->List[i].First_Edge; 
				lg->List[i].First_Edge = p;
			}
	lg->Vnum = mg.Vnum;
	lg->Enum = mg.Enum;
	for (int i = 0; i < lg->Vnum; i++) {
		lg->List[i].data.no = i;  //顶点元素赋值
	}
}
```

‍

==输出==

```cpp
void MG_print(MGraph mg) { //简单输出图的邻接矩阵
	for (int i = 0; i < mg.Vnum; i++) {
		for (int j = 0; j < mg.Vnum; j++)
			printf("%3d", mg.EG[i][j]);
		cout << endl;
	}
}
```

```cpp
void LG_print(ALG lg) {//输出图的邻接表
	EN p;
	for (int i = 0; i < lg->Vnum; i++) {
		p = lg->List[i].First_Edge; //指向第一位
		if (p) printf("%3d：", i); //打印结点
		while (p) { //打印链表内元素
			printf("%3d", p->no);
			p = p->next;
		}
		cout << endl;
	}
}
```

‍

==插入==

```cpp
void MG_inVex(MG& G, VT v) {	//插入顶点 ,由于数组下标比数量少1，可用旧顶点数做下标
	G.VG[G.Vnum] = v;   //顶点表增加
	G.Vnum++;      		  //顶点数加1 
	for (int i = 0; i < G.Vnum; i++) {  //初始化新顶点与其他顶点关联:0
		G.EG[G.Vnum - 1][i] = 0;
		G.EG[i][G.Vnum - 1] = 0;  //倒置下三角赋值,学到了
	}
}
```

‍

==删除==

```cpp
void MG_delVex(MG& G, VT v) {	//顶点删除 - 简单的方法: 只是将节点对应的行和列变为0,同时加上一个isVoid变量来判断是否为无效点.
	int p = MG_findVex(G, v);	//存放待删顶点下标位置

	for (int i = 0; i < G.Vnum; i++) {			//删除待删顶点所在列数据
		for (int j = p + 1; j < G.Vnum; j++) {
			G.EG[i][j - 1] = G.EG[i][j]; //后面的移动到前面来覆盖
		}
	}
	for (int i = 0; i < G.Vnum; i++) {			//删除待删顶点所在行数据
		for (int j = p + 1; j < G.Vnum; j++) {
			G.EG[j - 1][i] = G.EG[j][i]; //后面的移动到前面来覆盖
		}
	}
	for (int i = p + 1; i < G.Vnum; i++) 		//删除在顶点集中的待删顶点
		G.VG[i - 1] = G.VG[i]; //后面的移动到前面来覆盖

	G.Vnum--;							//顶点数自减
}
```

‍

==边的操作==

```cpp
void MG_inEdge(MG& G, VT v1, VT v2) { //插入边,同时可以插入权值
	int p1 = MG_findVex(G, v1), p2 = MG_findVex(G, v2);	//存放两个关联点位置
	G.EG[p1][p2] = G.EG[p2][p1] = 1;	//将关联点在矩阵两处(边权)赋值
}

void MG_delEdge(MG& G, VT v1, VT v2) {//删除边
	int p1 = MG_findVex(G, v1), p2 = MG_findVex(G, v2);	//存放两个关联点位置
	G.EG[p1][p2] = G.EG[p2][p1] = 0;	 //将关联点在矩阵两处边权赋为0
}
```

‍

‍

#### BFS

```cpp
void MG_BFS(MG& mg, QU& Q, int v) {  //广度优先遍历核心部分.
	MG_BDfs_visit(mg, v);  //设计的MG矩阵的广度优先以及深度优先搜索visit函数
	visited[v] = true;
	QU_in(Q, mg.VG[v]); //入队0.
	while (!QU_isVoid(Q)) {
		int u = QU_head(Q); //
		QU_out(Q); //简单的队头元素出队,并取值.
		for (int j = 0; j < mg.Vnum; j++) {    //从u元素对应的行开始找临界对象
			if (mg.EG[u][j] != 0 && visited[j] == false) { //有边且未被访问过
				MG_BDfs_visit(mg, j);
				visited[j] = true;  // 置顶点已被访问标志
				QU_in(Q, mg.VG[j]); // 顶点j进队
			}
		}
	}
}

void MG_BFS_Main(MG& mg) { //邻接矩阵的广度优先遍历
	for (int i = 0; i < mg.Vnum; i++)
		visited[i] = false; //标记数组初始化;
	QU Q = QU_new(Q);
	for (int i = 0; i < mg.Vnum; i++)  //继续调用
		if (visited[i] == false)  //对于每个联通分量调用一次BFS
			MG_BFS(mg, Q, i);
}

void LG_BFS(LG& lg, QU& Q, int v) {  //
	LG_BDfs_visit(lg, v);
	visited[v] = true;
	QU_in(Q, lg.List[v].data); //入队
	EdgeNode* p;
	while (!QU_isVoid(Q)) {
		int u = QU_head(Q); //
		QU_out(Q); //简单的队头元素出队,并取值.// 按邻接表顺序考察与顶点v邻接的各顶点w
		for (p = lg.List[u].First_Edge; p != NULL; p = p->next) {
			int k = p->no;   //设置k为中介,获得边节点p的顶点值
			if (visited[k] == false) {
				LG_BDfs_visit(lg, k);
				visited[k] = true;    // 置顶点w已被访问标志
				QU_in(Q, lg.List[k].data);   //放入k
			}
		}
	}
}

void LG_BFS_Main(LG& lg) { //邻接表的广度优先遍历.
	for (int i = 0; i < lg.Vnum; i++)
		visited[i] = false; //标记数组初始化;
	QU Q = QU_new(Q);
	for (int i = 0; i < lg.Vnum; i++)
		if (visited[i] == false)
			LG_BFS(lg, Q, i);
}
```

‍

#### DFS

```cpp
void MG_DFS(MG& mg, int v) {
	MG_BDfs_visit(mg, v);
	visited[v] = true;
	for (int j = 0; j < mg.Vnum; j++) {
		if (mg.EG[v][j] != 0 && visited[j] == false)
			MG_DFS(mg, j);
	}
}

void MG_DFS_Main(MG& mg) { //邻接矩阵的深度优先遍历(from0)
	for (int i = 0; i < mg.Vnum; i++)
		visited[i] = false; //标记数组初始化;
	for (int i = 0; i < mg.Vnum; i++)  //继续调用
		if (visited[i] == false)  //对于每个联通分量调用一次BFS
			MG_DFS(mg, i);
}

void LG_DFS(LG& lg, int v) {
	EdgeNode* p;
	LG_BDfs_visit(lg, v);
	visited[v] = true;
	p = lg.List[v].First_Edge;
	while (p) {                                // 检查所有与顶点i相邻接的顶点
		if (visited[p->no] == 0) // 如果该顶点未被访问过
			LG_DFS(lg, p->no);     // 从邻接顶点出发深度优先搜索
		p = p->next;              // 考察下一个邻接顶点
	}
}

void LG_DFS_Main(LG& lg) { //邻接表的广度优先遍历.
	for (int i = 0; i < lg.Vnum; i++)
		visited[i] = false; //标记数组初始化;
	for (int i = 0; i < lg.Vnum; i++)
		if (visited[i] == false)
			LG_DFS(lg, i);
}
```

‍

#### 最短路径

‍

==BFS求无权图最短路径==

```cpp
int path[MaxV_num];  //最短路径的来源
```

```cpp
void LG_MD_BFS(LG& lg, int u) {  //求u到其他顶点的最短路径distance[u]
	QU Q; //创建队列
	QU_new(Q);
	EdgeNode* p;

	for (int i = 0; i < lg.Vnum; i++)
		visited[i] = false; //标记数组初始化;
	for (int i = 0; i < lg.Vnum; i++) {
		Distance[i] = Infinity; //initial
		path[i] = -1; //from where?
	}

	Distance[u] = 0;
	visited[u] = true;
	QU_in(Q, lg.List[u].data); //入队

	while (!QU_isVoid(Q)) {
		u = QU_head(Q); //
		QU_out(Q); //简单的队头元素出队,并取u值.

		for (p = lg.List[u].First_Edge; p != NULL; p = p->next) {
			int k = p->no;   //设置k为中介,获得边节点p的顶点值
			if (visited[k] == false) {
				Distance[k] = Distance[u] + 1; //路径长度加1(可改对应权值
				path[k] = u;
				visited[k] = true;    // 置顶点w已被访问标志
				QU_in(Q, lg.List[k].data);   //放入k
			}
		}
	}
	cout << "目标顶点" << u << "到其他节点的带权路径长度为\n";
	for (int i = 0; i < lg.Vnum; i++)
		cout << Distance[i] << "  ";
	cout << endl;
}

void MG_MD_BFS(MG& mg, int u) {  //求u到其他顶点的最短路径distance[u]
	QU Q; //创建队列
	QU_new(Q);

	for (int i = 0; i < mg.Vnum; i++)
		visited[i] = false; //标记数组初始化;
	for (int i = 0; i < mg.Vnum; i++) {
		Distance[i] = Infinity; //initial
		path[i] = -1; //from where?
	}

	Distance[u] = 0;
	visited[u] = true;
	QU_in(Q, mg.VG[u]); //入队

	while (!QU_isVoid(Q)) {
		u = QU_head(Q); //
		QU_out(Q); //简单的队头元素出队,并取u值.
		for (int j = 0; j < mg.Vnum; j++) {    //从u元素对应的行开始找临界对象
			if (mg.EG[u][j] != 0 && visited[j] == false) { 
				Distance[j] = Distance[u] + 1;
				path[j] = u;
				visited[j] = true;    // 置顶点j已被访问标志
				QU_in(Q, mg.VG[j]);   //放入j
			}
		}
	}
	cout << "目标顶点" << u << "到其他节点的带权路径长度为\n";
	for (int i = 0; i < mg.Vnum; i++)
		cout << Distance[i] << "  ";
	cout << endl;
}
```

‍
