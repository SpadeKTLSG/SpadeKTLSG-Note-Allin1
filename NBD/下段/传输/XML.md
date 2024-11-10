‍

### Header

‍

‍

# 知识

‍

## 介绍

‍

### 不是 HTML 的替代

XML 被设计用来传输和存储数据，其焦点是数据的内容

HTML 被设计用来显示数据，其焦点是数据的外观

‍

## 概念

‍

‍

# 基础

‍

## 组成

XML 文件中常见的组成元素有:文档声明、元素、属性、注释、转义字符、字符区. 文件后缀名为 xml

‍

* **文档声明**  
  ​`<?xml version="1.0" encoding="utf-8" standalone="yes" ?>`​，文档声明必须在第一行，以 `<?xml`​ 开头，以 `?>`​ 结束，

  * version：指定 XML 文档版本. 必须属性，这里一般选择 1.0
  * enconding：指定当前文档的编码，可选属性，默认值是 utf-8
  * standalone：该属性不是必须的，描述 XML 文件是否依赖其他的 xml 文件，取值为 yes/no
* **元素**

  * 格式 1：`<person></person> `​
  * 格式 2：`<person/>`​
  * 普通元素的结构由开始标签、元素体、结束标签组成
  * 标签由一对尖括号和合法标识符组成，标签必须成对出现. 特殊的标签可以不成对，必须有结束标记 </>
* 元素体：可以是元素，也可以是文本，例如：`<person><name>张三</name></person>`​

  * 空元素：空元素只有标签，而没有结束标签，但**元素必须自己闭合**，例如：`<sex/>`​
  * 元素命名：区分大小写、不能使用空格冒号、不建议用 XML、xml、Xml 等开头
  * 必须存在一个根标签，有且只能有一个
* **属性**：`<name id="1" desc="高富帅">`​

  * 属性是元素的一部分，它必须出现在元素的开始标签中
  * 属性的定义格式：`属性名=“属性值”`​，其中属性值必须使用单引或双引号括起来
  * 一个元素可以有 0~N 个属性，但一个元素中不能出现同名属性
  * 属性名不能使用空格 , 不要使用冒号等特殊字符，且必须以字母开头
* **注释**：<!--注释内容-->  
  XML的注释与HTML相同，既以 `<!--`​ 开始，`-->`​ 结束.
* **转义字符**  
  XML 中的转义字符与 HTML 一样. 因为很多符号已经被文档结构所使用，所以在元素体或属性值中想使用这些符号就必须使用转义字符（也叫实体字符），例如：">"、"<"、"'"、"""、"&"  
  XML 中仅有字符 < 和 & 是非法的. 省略号、引号和大于号是合法的，把它们替换为实体引用

  |字符|预定义的转义字符|说明|
  | :----: | :----------------: | :------: |
  |<|​`&lt;`​|小于|
  |>|​` &gt;`​|大于|
  |"|​` &quot;`​|双引号|
  |'|​` &apos;`​|单引号|
  |&|​` &amp;`​|和号|
* **字符区**

  ```xml
  <![CDATA[
  	文本数据
  ]]>
  ```

  * CDATA 指的是不应由 XML 解析器进行解析的文本数据（Unparsed Character Data）
* CDATA 部分由 "<![CDATA[" 开始，由 "]]>" 结束；

  * 大量的转义字符在xml文档中时，会使XML文档的可读性大幅度降低. 这时使用CDATA段就会好一些
  * 规则：

    * CDATA 部分不能包含字符串 ]]>，也不允许嵌套的 CDATA 部分
    * 标记 CDATA 部分结尾的 ]]> 不能包含空格或折行

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <?xml-stylesheet type="text/css" href="../css/xml.css" ?>
  <!-- 7.处理指令：导入外部的css样式控制xml的界面效果，没有啥用，xml不是为了展示好看的！-->
  <!-- 1.申明 抬头 必须在第一行-->
  <!-- 2.注释，本处就是注释，必须用前后尖括号围起来 -->
  <!-- 3.标签（元素），注意一个XML文件只能有一个根标签-->
  <student>
      <!-- 4.属性信息：id , desc-->
      <name id="1" desc="高富帅">西门庆</name>
      <age>32</age>
      <!-- 5.实体字符：在xml文件中，我们不能直接写小于号，等一些特殊字符
          会与xml文件本身的内容冲突报错，此时必须用转义的实体字符。
      -->
      <sql>
         <!-- select * from student where age < 18 && age > 10; -->
          select * from student where age &lt; 18 &amp;&amp; age &gt; 10;
      </sql>
      <!-- 6.字符数据区：在xml文件中，我们不能直接写小于号，等一些特殊字符
          会与xml文件本身的内容冲突报错，此时必须用转义的实体字符
          或者也可以选择使用字符数据区，里面的内容可以随便了！
          -->
      <sql2>
          <![CDATA[
               select * from student where age < 18 && age > 10;
          ]]>
      </sql2>
  </student>
  ```

---

‍

## CRUD

‍

‍

### 增

‍

#### 示例

‍

person.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<person id="110">
	<age>18</age>		<!--年龄-->
	<name>张三</name>	  <!--姓名-->
	<sex/>				<!--性别-->
</person>
```

‍

```java
public class YootkXMLDemo {
    public static void main(String[] args) throws Exception {
        // 1、通过DocumentBuilderFactory类中的static方法获取本类的实例化对象
        DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
        // 2、创建DocumentBuilder对象实例
        DocumentBuilder builder = factory.newDocumentBuilder(); // 是为了解析XML文件
        // 3、创建一个空白的Document文档对象
        Document document = builder.newDocument(); // 空白文档
        // 4、定义本次操作要设置的数据内容
        String ids[] = new String[]{"muyan", "lee"};
        String names[] = new String[]{"沐言科技", "李兴华"};
        int ages[] = new int[]{19, 16};

        // 5、创建一个根节点
        Element membersElement = document.createElement("members");

        // 6、利用循环的模式将数据保存在相应的XML节点之中
        for (int x = 0; x < ids.length; x++) {
            Element memberElement = document.createElement("member"); // 创建节点
            Element nameElement = document.createElement("name"); // 创建节点
            Element ageElement = document.createElement("age"); // 创建节点
            memberElement.setAttribute("id", ids[x]); // 为member元素设置属性内容
            nameElement.appendChild(document.createTextNode(names[x])); // 创建并添加文本节点
            ageElement.appendChild(document.createTextNode(String.valueOf(ages[x]))); // 创建并添加文本节点
            memberElement.appendChild(nameElement);
            memberElement.appendChild(ageElement);
            membersElement.appendChild(memberElement);
        }
        document.appendChild(membersElement); // 文档里面需要进行节点的保存

        // 7、迭代输出测试
        NodeList memberList = document.getElementsByTagName("member"); // 根据名称获取节点列表
        for (int x = 0; x < memberList.getLength() ; x ++) {
            Element memberElement = (Element) memberList.item(x); // 获取指定的元素
            String id = memberElement.getAttribute("id"); // 获取属性的信息
            NodeList nameList = memberElement.getElementsByTagName("name"); // name节点集合
            NodeList ageList = memberElement.getElementsByTagName("age"); // age节点集合
            String name = nameList.item(0).getFirstChild().getTextContent();
            String age = ageList.item(0).getFirstChild().getTextContent();
            System.out.printf("ID：%s、姓名：%s、年龄：%s\n", id, name, age);
        }
    }
}
```

```java
        // 8、利用Java给出的辅助工具类，进行内容的输出处理
        TransformerFactory transformerFactory = TransformerFactory.newInstance();
        Transformer transformer = transformerFactory.newTransformer();
        transformer.setOutputProperty(OutputKeys.ENCODING, "UTF-8");
        transformer.transform(new DOMSource(document), new StreamResult(new File("H:" + File.separator + "自定义XML.xml")));
```

‍

‍

### 删

‍

#### 示例

```java
public class YootkXMLDemo {
    public static void main(String[] args) throws Exception {
        File file = new File("H:" + File.separator + "自定义XML.xml");
        // 1、通过DocumentBuilderFactory类中的static方法获取本类的实例化对象
        DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
        // 2、创建DocumentBuilder对象实例
        DocumentBuilder builder = factory.newDocumentBuilder(); // 是为了解析XML文件
        // 3、创建一个空白的Document文档对象
        Document document = builder.parse(file); // 解析XML文件
        // 4、获取要删除的元素
        NodeList urlList = document.getElementsByTagName("url"); // url元素是要删除的元素

        // 5、删除一个元素的前提：获取删除父元素.remove()删除
        for (int x = 0; x < urlList.getLength(); x ++) {    // 动态的获取节点的长度
            Element urlElement = (Element) urlList.item(x); // 获取删除元素
            urlElement.getParentNode().removeChild(urlElement);
        }

        // 6、利用Java给出的辅助工具类，进行内容的输出处理
        TransformerFactory transformerFactory = TransformerFactory.newInstance();
        Transformer transformer = transformerFactory.newTransformer();
        transformer.setOutputProperty(OutputKeys.ENCODING, "UTF-8");
        transformer.transform(new DOMSource(document), new StreamResult(file));
    }
}
```

‍

### 改

‍

#### 示例

```java
public class YootkXMLDemo {
    public static void main(String[] args) throws Exception {
        File file = new File("H:" + File.separator + "自定义XML.xml");
        // 1、通过DocumentBuilderFactory类中的static方法获取本类的实例化对象
        DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
        // 2、创建DocumentBuilder对象实例
        DocumentBuilder builder = factory.newDocumentBuilder(); // 是为了解析XML文件
        // 3、创建一个空白的Document文档对象
        Document document = builder.parse(file); // 解析XML文件
        // 4、如果要想添加节点，那么首先一定要获取要添加节点的父节点
        NodeList memberList = document.getElementsByTagName("member"); // 获取全部的Member节点

        // 5、循环每一个Node节点
        for (int x = 0; x < memberList.getLength(); x ++) { // 获取每一个Node
            Element memberElement = (Element) memberList.item(x); // 获取指定索引的元素
            Element urlElement = document.createElement("url"); // 创建新节点
            urlElement.appendChild(document.createTextNode("www.yootk.com")); // 设置文本内容
            memberElement.appendChild(urlElement); // 添加子节点
        }

        // 8、利用Java给出的辅助工具类，进行内容的输出处理
        TransformerFactory transformerFactory = TransformerFactory.newInstance();
        Transformer transformer = transformerFactory.newTransformer();
        transformer.setOutputProperty(OutputKeys.ENCODING, "UTF-8");
        transformer.transform(new DOMSource(document), new StreamResult(file));
    }
}
```

‍

## 约束

‍

### DTD

DTD (Document Type Definition)文档类型定义，用来约束xml文档

DTD 可以定义在 XML 文档中出现的元素、这些元素出现的次序、它们如何相互嵌套以及 XML 文档结构的其它详细信息.

‍

### Schema

‍

Schema 语言也可作为 XSD（XML Schema Definition）

Schema 约束文件本身也是一个 XML 文件，符合 XML 的语法，这个文件的后缀名 .xsd

一个 XML 中可以引用多个 Schema 约束文件，多个 Schema 使用名称空间区分（名称空间类似于 Java 包名）

dtd 里面元素类型的取值比较单一常见的是 PCDATA 类型，但是在 Schema 里面可以支持很多个数据类型

‍

‍

‍

## SAX解析

‍

### 示例

```java
import org.xml.sax.Attributes;
import org.xml.sax.SAXException;
import org.xml.sax.helpers.DefaultHandler;

import javax.xml.parsers.SAXParser;
import javax.xml.parsers.SAXParserFactory;
import java.io.File;

class MemberSAXHelp extends DefaultHandler {
    @Override
    public void startDocument() throws SAXException {
        System.out.println("【startDocument()】开始读取XML文档：<?xml version=\"1.0\" encoding=\"UTF-8\" standalone=\"no\"?>");
    }

    @Override
    public void startElement(String uri, String localName, String qName, Attributes attributes) throws SAXException {
        System.out.println("【startElement()】元素开始读取，qName = " + qName + "、id = " + attributes.getValue("id"));
    }

    @Override
    public void characters(char[] ch, int start, int length) throws SAXException {
        System.out.println("【characters()】文本节点内容：" + new String(ch, start, length));
    }

    @Override
    public void endElement(String uri, String localName, String qName) throws SAXException {
        System.out.println("【endElement()】元素结束读取，qName = " + qName);
    }

    @Override
    public void endDocument() throws SAXException {
        System.out.println("【endDocument()】文档读取完毕。");
    }
}

public class YootkXMLDemo {
    public static void main(String[] args) throws Exception {
        File file = new File("H:" + File.separator + "自定义XML.xml");
        SAXParserFactory factory = SAXParserFactory.newInstance();
        SAXParser parser = factory.newSAXParser();
        parser.parse(file, new MemberSAXHelp());
    }
}

```

‍

```java
public class YootkXMLDemo {
    public static void main(String[] args) throws Exception {
        // 1、定义当前的程序所需要使用到的XML数据内容
        String ids[] = new String[]{"muyan", "lee"};
        String names[] = new String[]{"沐言科技", "李兴华"};
        int ages[] = new int[]{19, 16};
        // 2、DOM4J是针对于DOM封装，所以一般都需要借助于一个工具类进行处理
        Document document = DocumentHelper.createDocument(); // 创建一个空白文档
        // 3、与文档最接近的概念那么就是根元素，所以直接进行根元素的创建
        Element membersElements = document.addElement("members"); // 创建并添加子元素
        // 4、循环生成所有的子元素
        for (int x = 0; x < names.length; x ++) {
            Element memberElement = membersElements.addElement("member"); // 创建子元素
            memberElement.addAttribute("id", ids[x]); // 设置元素属性内容
            Element nameElement = memberElement.addElement("name");
            nameElement.setText(names[x]); // 设置元素文本
            Element ageElement = memberElement.addElement("age");
            ageElement.setText(String.valueOf(ages[x]));
        }
        // 5、将当前的内存中保存的DOM树的结构直接进行输出
        XMLWriter xmlWriter = new XMLWriter(new FileOutputStream(new File("h:" + File.separator + "yootk.xml")));
        xmlWriter.write(document);// XML文件的输出
        xmlWriter.close();
    }
}
```

```java
public class YootkXMLDemo {
    public static void main(String[] args) throws Exception {
        // 1、配置要读取的XML文件的路径
        File file = new File("h:" + File.separator + "yootk.xml");
        // 2、创建一个SAX解析器，由DOM4J组件所提供的
        SAXReader saxReader = new SAXReader();
        // 3、通过给定的SAX解析器读取XML文档内容
        Document document = saxReader.read(new FileInputStream(file));
        // 4、所有的文档数据全部都在document之中，那么进行查询
        Element rootElement = document.getRootElement(); // 获取根元素
        // 5、根据根元素获取其全部的子元素
        List<Element> memberList = rootElement.elements("member"); // 获取全部的member元素
        // 6、获取Iterator接口实例
        Iterator<Element> iterator = memberList.iterator();
        while (iterator.hasNext()) {
            Element element = iterator.next(); // 获取指定的元素
            String id = element.attributeValue("id"); // 获取属性
            String name = element.elementText("name"); // 获取元素内容
            String age = element.elementText("age"); // 获取元素内容
            System.out.printf("用户ID：%s、姓名：%s、年龄：%s。\n", id, name, age);
        }
    }
}
```

‍

‍

## DOM

DOM（Document Object Model）：文档对象模型，把文档的各个组成部分看做成对应的对象，把 XML 文件全部加载到内存，在内存中形成一个树形结构，再获取对应的值

‍

‍

### DOM树解析

```java
public class YootkXMLDemo {

    public static void main(String[] args) throws Exception {
        // 1、通过DocumentBuilderFactory类中的static方法获取本类的实例化对象
        DocumentBuilderFactory factory = DocumentBuilderFactory.newInstance();
        // 2、创建DocumentBuilder对象实例
        DocumentBuilder builder = factory.newDocumentBuilder(); // 是为了解析XML文件
        // 3、设置要加载的XML文件的路径
        String filePath = "h:" + File.separator + "muyan.xml"; // 程序文件路径
        File file = new File(filePath); // 获得一个文件对象
        // 4、有了现在的文件和DocumentBuilder对象实例，就可以进行DOM树的生成了
        Document document = builder.parse(file); // 文件解析
        // 5、此时肯定要获取全部的<member>节点的数据，所以就可以通过节点查找的方法进行处理
        NodeList memberList = document.getElementsByTagName("member"); // 根据名称获取节点列表
        // 6、进行集合的迭代
        for (int x = 0; x < memberList.getLength() ; x ++) {
            Element memberElement = (Element) memberList.item(x); // 获取指定的元素
            String id = memberElement.getAttribute("id"); // 获取属性的信息
            NodeList nameList = memberElement.getElementsByTagName("name"); // name节点集合
            NodeList ageList = memberElement.getElementsByTagName("age"); // age节点集合
            String name = nameList.item(0).getFirstChild().getTextContent();
            String age = ageList.item(0).getFirstChild().getTextContent();
            System.out.printf("ID：%s、姓名：%s、年龄：%s\n", id, name, age);
        }
    }
}

```

‍

‍

# 高级

‍

## Dom4J

‍

### 解析

XML 解析就是从 XML 中获取到数据，DOM 是解析思想

‍

‍

‍

Dom4J 实现

* Dom4J 解析器构造方法：`SAXReader saxReader = new SAXReader()`​
* SAXReader 常用 API：

  * ​`public Document read(File file)`​：Reads a Document from the given File
  * ​`public Document read(InputStream in)`​：Reads a Document from the given stream using SAX
* Java Class 类 API：

  * ​`public InputStream getResourceAsStream(String path)`​：加载文件成为一个字节输入流返回

‍

### 根元素

Document 方法：`Element getRootElement()`​ 获取根元素

```java
// 需求：解析books.xml文件成为一个Document文档树对象，得到根元素对象。
public class Dom4JDemo {
    public static void main(String[] args) throws Exception {
        // 1.创建一个dom4j的解析器对象：代表整个dom4j框架。
        SAXReader saxReader = new SAXReader();
        // 2.第一种方式（简单）：通过解析器对象去加载xml文件数据，成为一个Document文档树对象。
        //Document document = saxReader.read(new File("Day13Demo/src/books.xml"));
  
        // 3.第二种方式（代码多点）先把xml文件读成一个字节输入流
        // 这里的“/”是直接去src类路径下寻找文件。
        InputStream is = Dom4JDemo01.class.getResourceAsStream("/books.xml");
        Document document = saxReader.read(is);
        System.out.println(document);
		//org.dom4j.tree.DefaultDocument@27a5f880 [Document: name null]
		// 4.从document文档树对象中提取根元素对象
        Element root = document.getRootElement();
        System.out.println(root.getName());//books
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<books>
    <book id="0001" desc="第一本书">
        <name>  JavaWeb开发教程</name>
        <author>    张三</author>
        <sale>100.00元   </sale>
    </book>
    <book id="0002">
        <name>三国演义</name>
        <author>罗贯中</author>
        <sale>100.00元</sale>
    </book>
    <user>
    </user>
</books>

```

‍

‍

### 子元素

Element 元素的 API:

* String getName()：取元素的名称.
* List<Element> elements()：获取当前元素下的全部子元素（一级）
* List<Element> elements(String name)：获取当前元素下的指定名称的全部子元素（一级）
* Element element(String name)：获取当前元素下的指定名称的某个子元素，默认取第一个（一级）

```java
public class Dom4JDemo {
    public static void main(String[] args) throws Exception {
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read(new File("Day13Demo/src/books.xml"));
        // 3.获取根元素对象
        Element root = document.getRootElement();
        System.out.println(root.getName());

        // 4.获取根元素下的全部子元素
        List<Element> sonElements = root.elements();
        for (Element sonElement : sonElements) {
            System.out.println(sonElement.getName());
        }
        // 5.获取根源下的全部book子元素
        List<Element> sonElements1 = root.elements("book");
        for (Element sonElement : sonElements1) {
            System.out.println(sonElement.getName());
        }
  
        // 6.获取根源下的指定的某个元素
        Element son = root.element("user");
        System.out.println(son.getName());
        // 默认会提取第一个名称一样的子元素对象返回！
        Element son1 = root.element("book");
        System.out.println(son1.attributeValue("id"));
    }
}

```

‍

‍

### 属性

Element 元素的 API：

* List<Attribute> attributes()：获取元素的全部属性对象
* Attribute attribute(String name)：根据名称获取某个元素的属性对象
* String attributeValue(String var)：直接获取某个元素的某个属性名称的值

Attribute 对象的 API：

* String getName()：获取属性名称
* String getValue()：获取属性值

```java
public class Dom4JDemo {
    public static void main(String[] args) throws Exception {
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read(new File("Day13Demo/src/books.xml"));
        Element root = document.getRootElement();
        // 4.获取book子元素
        Element bookEle = root.element("book");

        // 5.获取book元素的全部属性对象
        List<Attribute> attributes = bookEle.attributes();
        for (Attribute attribute : attributes) {
            System.out.println(attribute.getName()+"->"+attribute.getValue());
        }

        // 6.获取Book元素的某个属性对象
        Attribute descAttr = bookEle.attribute("desc");
        System.out.println(descAttr.getName()+"->"+descAttr.getValue());

        // 7.可以直接获取元素的属性值
        System.out.println(bookEle.attributeValue("id"));
        System.out.println(bookEle.attributeValue("desc"));
    }
}
```

‍

‍

### 文本

Element：

* String elementText(String name)：可以直接获取当前元素的子元素的文本内容
* String elementTextTrim(String name)：去前后空格,直接获取当前元素的子元素的文本内容
* String getText()：直接获取当前元素的文本内容
* String getTextTrim()：去前后空格,直接获取当前元素的文本内容

```java
public class Dom4JDemo {
    public static void main(String[] args) throws Exception {
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read(new File("Day13Demo/src/books.xml"));
        Element root = document.getRootElement();
        // 4.得到第一个子元素book
        Element bookEle = root.element("book");

        // 5.直接拿到当前book元素下的子元素文本值
        System.out.println(bookEle.elementText("name"));
        System.out.println(bookEle.elementTextTrim("name")); // 去前后空格
        System.out.println(bookEle.elementText("author"));
        System.out.println(bookEle.elementTextTrim("author")); // 去前后空格

        // 6.先获取到子元素对象，再获取该文本值
        Element bookNameEle = bookEle.element("name");
        System.out.println(bookNameEle.getText());
        System.out.println(bookNameEle.getTextTrim());// 去前后空格
    }
}
```

‍

## XPath

Dom4J 可以用于解析整个 XML 的数据，但是如果要检索 XML 中的某些信息，建议使用 XPath

‍

XPath 常用API

* List<Node> selectNodes(String var1) : 检索出一批节点集合
* Node selectSingleNode(String var1) : 检索出一个节点返回

‍

XPath 提供的四种检索数据的写法

1. 绝对路径：/根元素/子元素/子元素
2. 相对路径：./子元素/子元素 (.代表了当前元素)
3. 全文搜索：

    * //元素：在全文找这个元素
    * //元素1/元素2：在全文找元素1下面的一级元素 2
    * //元素1//元素2：在全文找元素1下面的全部元素 2
4. 属性查找：

    * //@属性名称：在全文检索属性对象
    * //元素[@属性名称]：在全文检索包含该属性的元素对象
    * //元素[@属性名称=值]：在全文检索包含该属性的元素且属性值为该值的元素对象

‍

### 示例

```java
public class XPathDemo {
    public static void main(String[] args) throws Exception {
        SAXReader saxReader = new SAXReader();
        InputStream is = XPathDemo.class.getResourceAsStream("/Contact.xml");
        Document document = saxReader.read(is);
        //1.使用绝对路径定位全部的name名称
        List<Node> nameNodes1 = document.selectNodes("/contactList/contact/name");
        for (Node nameNode : nameNodes) {
            System.out.println(nameNode.getText());
        }
  
        //2.相对路径。从根元素开始检索，.代表很根元素
        List<Node> nameNodes2 = root.selectNodes("./contact/name");
  
        //3.1 在全文中检索name节点
        List<Node> nameNodes3 = root.selectNodes("//name");//全部的
        //3.2 在全文中检索所有contact下的所有name节点  //包括sql，不外面的
        List<Node> nameNodes3 = root.selectNodes("//contact//name");
        //3.3 在全文中检索所有contact下的直接name节点
        List<Node> nameNodes3 = root.selectNodes("//contact/name");//不包括sql和外面
  
        //4.1 检索全部属性对象
        List<Node> attributes1 = root.selectNodes("//@id");//包括sql4
        //4.2 在全文检索包含该属性的元素对象
        List<Node> attributes1 = root.selectNodes("//contact[@id]");
        //4.3 在全文检索包含该属性的元素且属性值为该值的元素对象
        Node nodeEle = document.selectSingleNode("//contact[@id=2]");
        Element ele = (Element)nodeEle;
        System.out.println(ele.elementTextTrim("name"));//xi
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<contactList>
<contact id="1">
    <name>小白</name>
    <gender>女</gender>
    <email>bai@seazean.cn</email>
</contact>
<contact id="2">
    <name>小黑</name>
    <gender>男</gender>
    <email>hei@seazean.cn</email>
    <sql id="sql4">
        <name>sql语句</name>
    </sql>
</contact>
<contact id="3">
    <name>小虎</name>
    <gender>男</gender>
    <email>hu@seazean.cn</email>
</contact>
<contact></contact>
<name>外面的名称</name>
</contactList>
```

‍

# 实操

‍

## Bugfix
