‍

‍

json本质上就是一段字符串，解析时特别方便 `{}`​ 的形式

json格式的数据：是目前web数据传输格式的最流行的方式

‍

### Header

‍

# 知识

‍

# 基础

‍

## 格式

控制台拼写时候需要转义字符\来处理"

‍

### 示例

```java
1、

{
    "name": "沐言科技",
    "homepage": "www.yootk.com",
    "depts": ["教学研发部", "财务部", "市场部"]
}

2、
{
    "name": "沐言科技",
    "location": "中国-北京",
    "homepage": "www.yootk.com"
}
3、
{
    "name": "沐言科技",
    "homepage": "www.yootk.com",
    "depts": [
        {"deptno": 10, "dname": "教学研发部", "loc": "北京"},
        {"deptno": 20, "dname": "财务部", "loc": "上海"},
        {"deptno": 30, "dname": "市场部", "loc": "广州"}
    ]
}

4、
[
    {"deptno": 10, "dname": "教学研发部", "loc": "北京"},
    {"deptno": 20, "dname": "财务部", "loc": "上海"},
    {"deptno": 30, "dname": "市场部", "loc": "广州"}
]

```

‍

‍

## CRUD

‍

创建JSON对象

```java
        JSONObject companyObject = new JSONObject(); // 获取JSONObject对象
        companyObject.put("name", "沐言科技"); // 像Map接口一样保存数据
        companyObject.put("homepage", "www.yootk.com");
        System.out.println(companyObject.toJSONString());

```

‍

‍

## 转换

‍

### 示例

‍

‍

文本转JSON对象

```java
        String jsonData = "{\"name\":\"沐言科技\",\"homepage\":\"www.yootk.com\"}"; // JSON文本
        JSONObject jsonObject = JSON.parseObject(jsonData); // 将普通的文本转为JSON对象
        for (Map.Entry<String, Object> entry : jsonObject.entrySet()) {
            System.out.println("KEY = " + entry.getKey() + ":、VALUE = " + entry.getValue());
```

```java
        String jsonData = "{\"dept\":{\"deptno\":10,\"dname\":\"教学研发部\",\"loc\":\"北京\"}}"; // JSON文本
        JSONObject jsonObject = JSON.parseObject(jsonData); // 进行文本与JSON对象之间的转换
        Dept dept = jsonObject.getObject("dept", Dept.class);
        System.out.println(dept);
```

‍

‍
