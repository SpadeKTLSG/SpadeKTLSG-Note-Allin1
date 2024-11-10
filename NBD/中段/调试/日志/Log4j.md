‍

‍

(**log for java)Java的log,**  Apache的一个开源项目，可以灵活地记录日志信息，我们可以通过Log4j的配置文件灵活配置日志的记录格式、记录级别、输出格式，而不需要修改已有的日志记录代码. 

‍

### Header

‍

# 知识

‍

## 日志相关

‍

### 日志级别

一般日志级别包括：ALL，DEBUG， INFO， WARN， ERROR，FATAL，OFF

‍

# 基础

‍

## appender

* ConsoleAppender: 日志输出到控制台； FileAppender：输出到文件；
* RollingFileAppender：输出到文件，文件达到一定阈值时，自动备份日志文件;
* DailyRollingFileAppender：可定期备份日志文件，默认一天一个文件，也可设置为每分钟一个、每小时一个；
* WriterAppender：可自定义日志输出位置。

‍

‍

## 输出格式

1. org.apache.log4j.HTMLLayout（以HTML表格形式布局），
2. org.apache.log4j.PatternLayout（可以灵活地指定布局模式），
3. org.apache.log4j.SimpleLayout（包含日志信息的级别和信息字符串），
4. org.apache.log4j.TTCCLayout（包含日志产生的时间、线程、类别等等信息）

‍

```text
-X号: X信息输出时左对齐；  
%p: 输出日志信息优先级，即DEBUG，INFO，WARN，ERROR，FATAL,  
%d: 输出日志时间点的日期或时间，默认格式为ISO8601，也可以在其后指定格式，比如：%d{yyy MMM dd HH:mm:ss,SSS}，输出类似：2002年10月18日 22：10：28，921  
%r: 输出自应用启动到输出该log信息耗费的毫秒数  
%c: 输出日志信息所属的类目，通常就是所在类的全名  
%t: 输出产生该日志事件的线程名  
%l: 输出日志事件的发生位置，相当于%C.%M(%F:%L)的组合,包括类目名、发生的线程，以及在代码中的行数。举例：Testlog4.main (TestLog4.java:10)  
%x: 输出和当前线程相关联的NDC(嵌套诊断环境),尤其用到像java servlets这样的多客户多线程的应用中。  
%%: 输出一个"%"字符  
%F: 输出日志消息产生时所在的文件名称  
%L: 输出代码中的行号  
%m: 输出代码中指定的消息,产生的日志具体信息  
%n: 输出一个回车换行符，Windows平台为"/r/n"，Unix平台为"/n"输出日志信息换行  
可以在%与模式字符之间加上修饰符来控制其最小宽度、最大宽度、和文本的对齐方式。如：  
1)%20c：指定输出category的名称，最小的宽度是20，如果category的名称小于20的话，默认的情况下右对齐。  
2)%-20c:指定输出category的名称，最小的宽度是20，如果category的名称小于20的话，"-"号指定左对齐。  
3)%.30c:指定输出category的名称，最大的宽度是30，如果category的名称大于30的话，就会将左边多出的字符截掉，但小于30的话也不会有空格。  
4)%20.30c:如果category的名称小于20就补空格，并且右对齐，如果其名称长于30字符，就从左边较远输出的字符截掉。
```

‍
