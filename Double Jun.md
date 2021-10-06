# JSP 跳转作用

在web中可以使用<<jsp:forward>>指令，将一个用户的请求（request），从一个页面传递到另一个页面。

~~~
一、页面跳转语法
    1、不传递参数
    	<jsp:forward page="{要包含的文件路径|<%=表达式%>}"/>
    2、传递参数
    	<jsp:forward page="{要包含的文件路径|<%=表达式%>}">
    		<jsp:param name="参数名称" value="参数内容"/>
    		...可以向被包含页面中传递多个参数
    	</jsp:forward>
~~~

使用跳转可以完成页面请求的传递。

跳转操作属于==服务器==跳转，跳转之后的页面路径不改变

## 实例操作：用户登录程序实现

1、login.jsp：提供用户的登录表单，可以输入用户ID和密码

2、logincheck.jsp：登录检查页，根据==表单==提交过来的ID和密码进行数据库验证，成功跳转到登录成功页，否则    跳转到登录失败页。

3、loginsuccess.jsp：登录成功页，显示欢迎信息。

4、loginfailure.jsp：登录失败页，提示用户输入错误，并提供重新登录的超链接。

### ==login.jsp==

~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
  <head>
    <title>login</title>
  </head>
  <body>
  <form action="logincheck.jsp" method="post">
  <table>
    <td>
      <tr>用户名：</tr>
      <tr><input type="text" name="username"></tr>
    </td>
    <br>
    <td>
      <tr>密&nbsp;&nbsp;&nbsp;码：</tr>
      <tr><input type="password" name="pwd"></tr>
    </td>
    <br>
    <td>
      <tr><input type="submit" value="登录"></tr>
      &nbsp;&nbsp;
      <tr><input type="reset" value="重置"></tr>
    </td>
  </table>
  </form>
  </body>
</html> 
~~~

### ==logincheck==(重点)

~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>logincheck</title>
</head>
<body>
<%
    String name = request.getParameter("username");
    String password = request.getParameter("pwd");
    if(name != null && name.equals("lyj") && password != null && password.equals("520")) {
%>
    <jsp:forward page="loginsuccess.jsp">
        <jsp:param name="name" value="name"/>
    </jsp:forward>
<%
    }else {
%>
    <jsp:forward page="loginfailure.jsp"/>
<%
    }
%>
</body>
</html>
~~~

### ==loginsuccess==

~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>loginsuccess</title>
</head>
<body>
<h1>登录成功</h1>
<h2>Welcome!</h2>
<h3><%=request.getParameter("username")%></h3>
</body>
</html>
~~~

### ==loginfailure==

~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>loginfailure</title>
</head>
<body>
<h1>登录失败</h1>
<h2><a href="index.jsp">再次登录</a> </h2>
</body>
</html>
~~~



# 网上计数器

~~~jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>count</title>
</head>
<body>
<%!int counter=0;
    synchronized void counterFunction()
    {
        counter++;
    }
%>
<%counterFunction();%>
<h1>网站计数器</h1>
<h2>你是第<%=counter%>位访问者</h2>
</body>
</html>
~~~



# Markdown合并表格单元格

## 1、Markdown本身不支持单元格合并

Markdown语法自带如下表格实现：

```
| 第一列  | 第二列 |
|--|--|
| testk1 | testv1 |
| testk2 | testv2 |
```

效果如下：

| 第一列 | 第二列 |
| ------ | ------ |
| testk1 | testv1 |
| testk2 | testv2 |

但是，Markdown语法本身并不支持复杂表格的插入和实现，比如合并多行表格。

**那如果想要合并单元格的时候，怎么实现呢？**

考虑到 Markdown 支持 html ，所以，我们可以通过插入 html 中的 table 来实现。

## 2、Html 合并行

实现代码如下：

```
<table>
    <tr>
        <td>第一列</td> 
        <td>第二列</td> 
   </tr>
    <tr>
        <td colspan="2">这里是合并行</td>    
    </tr>
    <tr>
        <td colspan="2">这里也是合并行</td>    
    </tr>
</table>
```

效果如下：

<table>
    <tr>
        <td>第一列</td> 
        <td>第二列</td> 
   </tr>
    <tr>
        <td colspan="2">这里是合并行</td>    
    </tr>
    <tr>
        <td colspan="2">这里也是合并行</td>    
    </tr>
</table>
## 3、Html 合并列

实现代码如下：

```
<table>
    <tr>
        <td>第一列</td> 
        <td>第二列</td> 
   </tr>
    <tr>
        <td rowspan="2">这里是合并列</td>    
        <td >行二列二</td>  
    </tr>
    <tr>
        <td >行三列二</td>  
    </tr>
</table>
```

效果如下：

<table>
    <tr>
        <td>第一列</td> 
        <td>第二列</td> 
   </tr>
    <tr>
        <td rowspan="2">这里是合并列</td>    
        <td >行二列二</td>  
    </tr>
    <tr>
        <td >行三列二</td>  
    </tr>
</table>
## 4、Html 合并行和列

实现代码如下：

```
<table>
    <tr>
        <td>第一列</td> 
        <td>第二列</td> 
   </tr>
   <tr>
        <td colspan="2">我是合并行</td>    
   </tr>
   <tr>
        <td>行二列一</td> 
        <td>行二列二</td> 
   </tr>
    <tr>
        <td rowspan="2">我是合并列</td>    
        <td >行三列二</td>  
    </tr>
    <tr>
        <td >行四列二</td>  
    </tr>
</table>
```

效果如下：

<table>
    <tr>
        <td>第一列</td> 
        <td>第二列</td> 
   </tr>
   <tr>
        <td colspan="2">我是合并行</td>    
   </tr>
   <tr>
        <td>行二列一</td> 
        <td>行二列二</td> 
   </tr>
    <tr>
        <td rowspan="2">我是合并列</td>    
        <td >行三列二</td>  
    </tr>
    <tr>
        <td >行四列二</td>  
    </tr>
</table>


# HTML

## 网页的组成部分

~~~
	内容（结构）、 表现  、行为
	内容（结构），是我们在页面中可以看到的数据。我们称之为内容。一般内容 我们使用html 技术来展示。
	表现，指的是这些内容在页面上的展示形式。比如说。布局，颜色，大小等等。一般使用CSS 技术实现。
	行为，指的是页面中元素与输入设备交互的响应。一般使用 javascript 技术实现。
~~~

## HTML简介

~~~
	Hyper Text Markup Language （超文本标记语言） 简写：HTML
	HTML通过标签来标记要显示的网页中的各个部分。网页文件本身是一种文本文件，通过在文本文件中添加标记符，可以告诉浏览器如何显示其中的内容（如：文字如何处理，画面如何安排，图片如何显示等）
~~~

## 创建HTML文件

第一个HTML示例：

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
	hello world
</body>
</html>
~~~

​		注：Java文件是需要先编译, 再由JAVA虚拟机跑起来。但 HTML 文件它不需要编译, 直接由浏览器进行解析执行

## HTML文件的书写规范

~~~html
<html>                                  表示整个 html 页面的开始
<head>                                  头信息
<meta charset="UTF-8">                  
<title>标题</title>                     标题
</head>
<body>                                  body 是页面的主体内容
页面主体内容
</body>
</html>                                 表示整个 html 页面的结束

Html的代码注释 
<!-- 这是 html 注释，可以在页面右键查看源代码中看到 -->
~~~

## HTML的标签介绍

~~~
1.标签的格式:
	<标签名>封装的数据</标签名>
2.标签名大小写不敏感。
3.标签拥有自己的属性。
	i. 分为基本属性： bgcolor="red" 		可以修改简单的样式效果
	ii.事件属性： onclick="alert('你好！');" 可以直接设置事件响应后的代码。
4.标签又分为，单标签和双标签。
	i. 单标签格式： <标签名 /> 			  <br> 换行 <hr> 水平线
	ii. 双标签格式: <标签名> ...封装的数据...</标签名>
~~~

~~~html
标签的语法：

<!-- ①标签不能交叉嵌套 -->
正确：<div><span>早安，尚硅谷</span></div>
错误：<div><span>早安，尚硅谷</div></span>

<hr />

<!-- ②标签必须正确关闭 -->
<!-- i.有文本内容的标签： -->
正确：<div>早安，尚硅谷</div>
错误：<div>早安，尚硅谷

<hr />

<!-- ii.没有文本内容的标签： -->
正确：<br />
错误：<br>

<hr />

<!-- ③属性必须有值，属性值必须加引号 -->
正确：<font color="blue">早安，尚硅谷</font>
错误：<font color=blue>早安，尚硅谷</font>
错误：<font color>早安，尚硅谷</font>

<hr />

<!-- ④注释不能嵌套 -->
正确：<!-- 注释内容 --> <br/>
错误：<!-- <!-- 这是错误的 html 注释 --> -->

<hr />

注意事项：
1.html 代码不是很严谨。有时候标签不闭合，也不会报错。
~~~

## 常用标签介绍

### font字体标签

~~~html
<body>
<!-- 字体标签
	需求 1 ：在网页上显示 我是字体标签 ，并修改字体为 宋体，颜色为红色。
	font 标签是字体标签 , 它可以用来修改文本的字体 , 颜色 , 大小 ( 尺寸 )
    color 属性修改颜色
    face 属性修改字体
    size 属性修改文本大小
-->
<font color="red" face=" 宋体" size="7">我是字体标签</font>
<body>
~~~

### 特殊字符

~~~html
	通常情况下，HTML 会裁掉文档中的空格。假如你在文档中连续输入 10 个空格，那么 HTML 会去掉其中的9个。如果使用&nbsp;，就可以在文档中增加空格。
~~~

### 标题标签

~~~html
<body>
<!-- 标题标签
需求 1 ：演示标题 1 到 标题 6 的
	h1 - h6 都是标题标签
	h1 最大
	h6 最小
	align 属性是对齐属性
	left 左对齐 ( 默认 )
	center 剧中
	right 右对齐
-->
<h1 align="left">标题 1</h1>
<h2 align="center">标题 2</h2>
<h3 align="right">标题 3</h3>
<h4>标题 4</h4>
<h5>标题 5</h5>
<h6>标题 6</h6>
<h7>标题 7</h7>
<body>
~~~

### 超链接（重点）

~~~html
<body>
<!-- a 标签是 超链接
    href 属性设置连接的地址
    target 属性设置哪个目标进行跳转
    _self 表示当前页面 ( 默认值 )
    _blank 表示打开新页面来进行跳转
-->
<a href="http://localhost:8080">百度</a><br/>
<a href="http://localhost:8080" target="_self">百度_self</a><br/>
<a href="http://localhost:8080" target="_blank">百度_blank</a><br/>
<body>
~~~

### 列表标签

~~~html
<body>
<!-- 需求 1 ：使用无序，列表方式，把东北 F4 ，赵四，刘能，小沈阳，宋小宝，展示出来
    ul 是无序列表
	ol 是有序列表
    type 属性可以修改列表项前面的符号
    li 是列表项
-->
<ul type="none">
    <li>赵四</li>
    <li>刘能</li>
    <li>小沈阳</li>
    <li>宋小宝</li>
</ul>
</body>
~~~

| 标签 |     描述     |
| :--: | :----------: |
| <ol> | 定义有序列表 |
| <ul> | 定义无序列表 |
| <li> |  定义列表项  |

### img标签

~~~html
<body>
<!-- 需求 1 ：使用 img 标签显示一张美女的照片。并修改宽高，和边框属性
    img 标签是图片标签 , 用来显示图片
    src 属性可以设置图片的路径
    width 属性设置图片的宽度
    height 属性设置图片的高度
    border 属性设置图片边框大小
    alt 属性设置当指定路径找不到图片时 , 用来代替显示的文本内容

    在 JavaSE 中路径也分为相对路径和绝对路径 .
    相对路径 : 从工程名开始算
    绝对路径 : 盘符 :/ 目录 / 文件名

    在 web 中路径分为相对路径和绝对路径两种
    相对路径 :
    . 表示当前文件所在的目录
    .. 表示当前文件所在的上一级目录
    文件名 表示当前文件所在目录的文件 , 相当于 ./ 文件名 ./ 可以省略
    绝对路径 :
    正确格式是 : http://ip:port/ 工程名 / 资源路径
    错误格式是 : 盘符 :/ 目录 / 文件名
-->
<img src="1.jpg" width="200" height="260" border="1" alt=" 美女找不到"/>
<img src="../2.jpg" width="200" height="260" />
<img src="../imgs/3.jpg" width="200" height="260" />
<img src="../imgs/4.jpg" width="200" height="260" />
<img src="../imgs/5.jpg" width="200" height="260" />
<img src="../imgs/6.jpg" width="200" height="260" />
~~~



# JDBC连接SqlServer

~~~java
/*
一、加载JDBC驱动程序：
	在连接数据库之前，首先要加载想要连接的数据库的驱动到JVM,这通过java.lang.Class类的静态方法forName(String className)实现
成功加载后，会将Driver类的实例注册到DriverManager类中
二、提供JDBC连接的URL：
	连接URL定义了连接数据库时的协议、子协议、数据源标识
	书写形式： 协议:子协议:数据源标识
	协议：在JDBC中总是以jdbc开始   子协议：是桥连接的驱动程序或者是数据库管理系统名称
	数据源标识：标记找到数据库来源的地址与连接端口
三、创建数据库的连接：
	要连接数据库，需要向java.sql.DriverManager请求并获得Connection对象(该对象就代表一个数据库的连接)
	使用DriverManager类的getConnection(String url,String user,String password)方法
传入指定欲连接的数据库的路径、数据库用户名、数据库密码来获得
*/
package JDBC.SqlServer;
import java.sql.*;

public class jdbc {
    public static void main(String[] args) {
        String driverName = "com.microsoft.sqlserver.jdbc.SQLServerDriver";
        String dbURL = "jdbc:sqlserver://127.0.0.1:1433;DatabaseName=Library";
        String userName = "sa";            //sqlserver用户名
        String userPwd = "1234";    //sqlserver用户密码
        try {
            Class.forName(driverName);   //加载sqlserver的驱动类
            System.out.println("加载SQLServer驱动类成功!");
        } catch (ClassNotFoundException a) {
            System.out.println("加载SQLServer驱动失败!");
            a.printStackTrace();
        }
        Connection dbcon = null;           //处理与特定数据库的连接
        try {
            dbcon = DriverManager.getConnection(dbURL, userName, userPwd);
            System.out.println("数据库连接成功!");
            dbcon.close();
        } catch (SQLException e) {
            System.out.println("数据库连接失败!");
            e.printStackTrace();
        }
    }
}

~~~



# JDBC连接MySql

~~~java
package JDBC.Mysql;

import java.sql.*;

public class mysql {
    public static void main(String[] args) {
        Connection con;
        //jdbc驱动
        String driver="com.mysql.cj.jdbc.Driver";
        //这里我的数据库是book
        String url="jdbc:mysql://localhost:3306/book?&useSSL=false&serverTimezone=UTC";
        String user="root";
        String password="lyjym121380";
        try {
            //注册JDBC驱动程序
            Class.forName(driver);
            //建立连接
            con = DriverManager.getConnection(url, user, password);
            if (!con.isClosed()) {
                System.out.println("数据库连接成功");
            }
            con.close();
        } catch (ClassNotFoundException e) {
            System.out.println("数据库驱动没有安装");

        } catch (SQLException e) {
            e.printStackTrace();
            System.out.println("数据库连接失败");
        }
    }
}

~~~



# Typora图床令牌

dbad4249cc0153858b14e7623c5260a6



# 微信小程序APPID

wxdf5550b53314f267



# Java练手

## 冒泡排序

~~~java
public class MaoPao {
    public static void main(String[] args) {
        int[] arry = {12, 67, 57, 48, 88};
        for (int i = 0; i < arry.length; i++) {
            System.out.print(arry[i] + " ");
        }
        System.out.println();

        for (int i = 0; i < arry.length; i++) {
            for (int j = 0; j < arry.length - 1; j++) {
                if (arry[j] > arry[j + 1]) {
                    int temp = arry[j];
                    arry[j] = arry[j + 1];
                    arry[j + 1] = temp;
                }
            }
        }
        for (int i = 0; i < arry.length; i++) {
            System.out.print(arry[i] + " ");
        }
    }
}
~~~

## 获得电脑字体

~~~java
//获取本机的全部字体
import java.awt.*;

public class GetAllFonts {
    public static void main(String[] args) {
        //获取本地的绘图设备和字体的集合
        GraphicsEnvironment eq =GraphicsEnvironment.getLocalGraphicsEnvironment();
        //取得全部可用的字体
        String [] fontNames = eq.getAvailableFontFamilyNames();
        //循环输出全部内容
        for(int i = 0 ; i <fontNames.length ; i++){
            //输出每一个字体的名称
            System.out.println(fontNames[i]);
        }
    }
}
~~~

## 画图板

### 主类



# python学习

## Python教程

### 查看Python版本

~~~python
1.在命令窗口(Windows 使用 win+R 调出 cmd 运行框)使用以下命令查看我们使用的 Python 版本：
	python -v

2.进入Python的交互式编程模式查看版本：
	Python 3.9.2 (tags/v3.9.2:1a79785, Feb 19 2021, 13:44:55) [MSC v.1928 64 bit (AMD64)] on win32
	Type "help", "copyright", "credits" or "license" for more information.
	>>>
~~~

### 第一个Python程序

~~~python
print("Hello World!")
~~~

## Python基础语法

### 标识符

- 第一个字符必须是字母表中字母或下划线 **_** 。
- 标识符的其他的部分由字母、数字和下划线组成。
- 标识符对大小写敏感。

在 Python 3 中，可以用中文作为变量名，非 ASCII 标识符也是允许的了。

### Python关键字

我们不能把它们用作任何标识符名称。Python 的标准库提供了一个 keyword 模块，可以输出当前版本的所有关键字：

~~~python
>>> import keyword
>>> keyword.kwlist
['False', 'None', 'True', '__peg_parser__', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
>>>
~~~

### 注释

单行注释以 **#** 开头

~~~python
# 第一个注释
print("Hello,Python!")	# 第二个注释
~~~

多行注释可以用多个 **#** 号，还有 **'''** 和 **"""**

~~~python
# 第一个注释
# 第二个注释

'''
第三个注释
第四个注释
'''

""""
第五个注释
第六个注释
""""

print("Hello,Python!")
~~~

### 行与缩进

python最具特色的就是使用缩进来表示代码块，不需要使用大括号 **{}** 。

缩进的空格数是可变的，但是<font color="red">同一个代码块的语句必须包含相同的缩进空格数</font>。

~~~python
if Ture:
    print("Ture")
else:
    print("False")
~~~

以下代码最后一行语句缩进数的空格数不一致，会导致运行错误：

~~~python
if True:
    print ("Answer")
    print ("True")
else:
    print ("Answer")
  print ("False")    # 缩进不一致，会导致运行错误

以上程序由于缩进不一致，执行后会出现类似以下错误：
 File "test.py", line 6
    print ("False")    # 缩进不一致，会导致运行错误
                                      ^
IndentationError: unindent does not match any outer indentation level
~~~

### 多行语句

Python 通常是一行写完一条语句，但如果语句很长，可以使用反斜杠(\)来实现多行语句

~~~python
total = item_one + \
        item_two + \
        item_three
~~~

在 [], {}, 或 () 中的多行语句，不需要使用反斜杠(\)

~~~python
total = ['item_one', 'item_two', 'item_three',
        'item_four', 'item_five']
~~~

### 数字（Number）类型

python中数字有四种类型：整数、布尔型、浮点数和复数。

- **int** (整数), 如 1, 只有一种整数类型 int，表示为长整型，没有 python2 中的 Long。
- **bool** (布尔), 如 True。
- **float** (浮点数), 如 1.23、3E-2
- **complex** (复数), 如 1 + 2j、 1.1 + 2.2j

### 字符串（String）

- python中单引号和双引号使用完全相同。
- 使用三引号('''或""")可以指定一个多行字符串。
- 转义符 '\'
- 反斜杠可以用来转义，使用r可以让反斜杠不发生转义。。 如 r"this is a line with \n" 则\n会显示，并不是换行。
- 按字面意义级联字符串，如"this " "is " "string"会被自动转换为this is string。
- 字符串可以用 + 运算符连接在一起，用 * 运算符重复。
- Python 中的字符串有两种索引方式，从左往右以 0 开始，从右往左以 -1 开始。
- Python中的字符串不能改变。
- Python 没有单独的字符类型，一个字符就是长度为 1 的字符串。
- 字符串的截取的语法格式如下：**变量[头下标:尾下标:步长]**

~~~python
word = "字符串"
sentence = "这是一个句子"
paragraph = """这是一个段落，
可以由多行组成"""
~~~

实例

~~~python
str='123456789'
 
print(str)                 # 输出字符串
print(str[0:-1])           # 输出第一个到倒数第二个的所有字符
print(str[0])              # 输出字符串第一个字符
print(str[2:5])            # 输出从第三个开始到第五个的字符
print(str[2:])             # 输出从第三个开始后的所有字符
print(str[1:5:2])          # 输出从第二个开始到第五个且每隔一个的字符（步长为2）
print(str * 2)             # 输出字符串两次
print(str + '你好')         # 连接字符串
 
print('------------------------------')
 
print('hello\nrunoob')      # 使用反斜杠(\)+n转义特殊字符
print(r'hello\nrunoob')     # 在字符串前面添加一个 r，表示原始字符串，不会发生转义

以上实例输出结果：
123456789
12345678
1
345
3456789
24
123456789123456789
123456789你好
------------------------------
hello
runoob
hello\nrunoob
~~~

### 同一行显示多条语句

~~~python
import sys; x = 'runoob'; sys.stdout.write(x + '\n')

输出结果为：
runoob
~~~

使用交互式命令行执行，输出结果为：

~~~python
>>> import sys; x = 'runoob'; sys.stdout.write(x + '\n')
runoob
7
~~~

### 多个语句构成代码组

缩进相同的一组语句构成一个代码块，我们称之代码组。

像if、while、def和class这样的复合语句，首行以关键字开始，以冒号( : )结束，该行之后的一行或多行代码构成代码组。

我们将首行及后面的代码组称为一个子句(clause)。

~~~python
if expression : 
   suite
elif expression : 
   suite 
else : 
   suite
~~~

### Print输出

**print** 默认输出是换行的，如果要实现不换行需要在变量末尾加上 **end=" "**：

~~~python
x="a"
y="b"
# 换行输出
print( x )
print( y )
 
print('---------')
# 不换行输出
print( x, end=" " )
print( y, end=" " )
print()

结果：
a
b
---------
a b
~~~

### import 与 from...import

在 python 用 **import** 或者 **from...import** 来导入相应的模块。

将整个模块(somemodule)导入，格式为： **import somemodule**

从某个模块中导入某个函数,格式为： **from somemodule import somefunction**

从某个模块中导入多个函数,格式为： **from somemodule import firstfunc, secondfunc, thirdfunc**

将某个模块中的全部函数导入，格式为： **from somemodule import \***

导入 sys 模块

~~~python
import sys
print('================Python import mode==========================')
print ('命令行参数为:')
for i in sys.argv:
    print (i)
print ('\n python 路径为',sys.path)
~~~

导入 sys 模块的 argv,path 成员

~~~python
from sys import argv,path  #  导入特定的成员
 
print('================python from import===================================')
print('path:',path) # 因为已经导入path成员，所以此处引用时不需要加sys.path
~~~

## Python基本数据类型

Python 中的变量不需要声明。每个变量在使用前都必须赋值，变量赋值以后该变量才会被创建。

在 Python 中，变量就是变量，它没有类型，我们所说的"类型"是变量所指的内存中对象的类型。

等号（=）用来给变量赋值。

等号（=）运算符左边是一个变量名,等号（=）运算符右边是存储在变量中的值。

~~~python
counter = 100          # 整型变量
miles   = 1000.0       # 浮点型变量
name    = "runoob"     # 字符串

print (counter)
print (miles)
print (name)

结果：
100
1000.0
runoob


counter = 100  # 整型变量
miles = 1000.0  # 浮点型变量
name = "runoob"  # 字符串

print(type(counter))
print(type(miles))
print(type(name))

结果：
<class 'int'>
<class 'float'>
<class 'str'>
~~~

### 多个变量赋值

同时为多个变量赋值

```python
a = b = c = 1

创建一个整型对象，值为 1，从后向前赋值，三个变量被赋予相同的数值。
```

也可以为多个对象指定多个变量。

```python
a, b, c = 1, 2, "runoob"

两个整型对象 1 和 2 的分配给变量 a 和 b，字符串对象 "runoob" 分配给变量 c。
```

### **/标准数据类型/**

<font color="red">Python3 中有六个标准的数据类型：</font>

- Number（数字）
- String（字符串）
- List（列表）
- Tuple（元组）
- Set（集合）
- Dictionary（字典）

Python3 的六个标准数据类型中：

- **不可变数据（3 个）：**Number（数字）、String（字符串）、Tuple（元组）；
- **可变数据（3 个）：**List（列表）、Dictionary（字典）、Set（集合）。

### Number(数字)

Python3 支持 **int、float、bool、complex（复数）**。

在Python 3里，只有一种整数类型 int，表示为长整型，没有 python2 中的 Long。

内置的 type() 函数可以用来查询变量所指的对象类型。

~~~python
>>> a, b, c, d = 20, 5.5, True, 4+3j
>>> print(type(a), type(b), type(c), type(d))
<class 'int'> <class 'float'> <class 'bool'> <class 'complex'>
~~~

此外还可以用 isinstance 来判断：

实例

~~~python
>>> a = 111
>>> isinstance(a, int)
True
>>>
~~~

isinstance 和 type 的区别在于：

- type()不会认为子类是一种父类类型。

- isinstance()会认为子类是一种父类类型。

  ~~~python
  >>> class A:
  ...     pass
  ... 
  >>> class B(A):
  ...     pass
  ... 
  >>> isinstance(A(), A)
  True
  >>> type(A()) == A 
  True
  >>> isinstance(B(), A)
  True
  >>> type(B()) == A
  False
  ~~~

**注意：***在 Python2 中是没有布尔型的，它用数字 0 表示 False，用 1 表示 True。到 Python3 中，把 True 和 False 定义成关键字了，但它们的值还是 1 和 0，它们可以和数字相加。*

指定一个值时，Number 对象就会被创建：

```
var1 = 1
var2 = 10
```

可以使用del语句删除一些对象引用。

<font color="red">del语句的语法是：</font>

```
del var1[,var2[,var3[....,varN]]]
```

可以通过使用del语句删除单个或多个对象。例如：

```
del var
del var_a, var_b
```

### 数值运算

~~~python
>>> 5 + 4  # 加法
9
>>> 4.3 - 2 # 减法
2.3
>>> 3 * 7  # 乘法
21
>>> 2 / 4  # 除法，得到一个浮点数
0.5
>>> 2 // 4 # 除法，得到一个整数
0
>>> 17 % 3 # 取余
2
>>> 2 ** 5 # 乘方
32
~~~

**注意：**

- 1、Python可以同时为多个变量赋值，如a, b = 1, 2。
- 2、一个变量可以通过赋值指向不同类型的对象。
- 3、数值的除法包含两个运算符：**/** 返回一个浮点数，**//** 返回一个整数。
- 4、在混合计算时，Python会把整型转换成为浮点数。

Python还支持复数，复数由实数部分和虚数部分构成，可以用a + bj,或者complex(a,b)表示， 复数的实部a和虚部b都是浮点型

### String(字符串)

Python中的字符串用单引号 **'** 或双引号 **"** 括起来，同时使用反斜杠 \转义特殊字符。

字符串的截取的语法格式如下：

```python
变量[头下标:尾下标]
```

索引值以 0 为开始值，-1 为从末尾的开始位置。

<img src="https://gitee.com/LyjYm/typora-photo/raw/master/photo/20210922211521.png" alt="url" />

加号 **+** 是字符串的连接符， 星号 ***** 表示复制当前字符串，与之结合的数字为复制的次数。

~~~python
str = 'Runoob'

print (str)          # 输出字符串
print (str[0:-1])    # 输出第一个到倒数第二个的所有字符
print (str[0])       # 输出字符串第一个字符
print (str[2:5])     # 输出从第三个开始到第五个的字符
print (str[2:])      # 输出从第三个开始的后的所有字符
print (str * 2)      # 输出字符串两次，也可以写成 print (2 * str)
print (str + "TEST") # 连接字符串

Runoob
Runoo
R
noo
noob
RunoobRunoob
RunoobTEST
~~~

Python 使用反斜杠 \ 转义特殊字符，如果你不想让反斜杠发生转义，可以在字符串前面添加一个 r，表示原始字符串：

~~~python
>>> print('Ru\noob')
Ru
oob
>>> print(r'Ru\noob')
Ru\noob
>>>
~~~

另外，反斜杠(\)可以作为续行符，表示下一行是上一行的延续。也可以使用 **"""..."""** 或者 **'''...'''** 跨越多行。

Python 没有单独的字符类型，一个字符就是长度为1的字符串。

~~~python
>>> word = 'Python'
>>> print(word[0], word[5])
P n
>>> print(word[-1], word[-6])
n P
~~~

<font color="red">**注意：**</font>

- 1、反斜杠可以用来转义，使用r可以让反斜杠不发生转义。
- 2、字符串可以用+运算符连接在一起，用*运算符重复。
- 3、Python中的字符串有两种索引方式，从左往右以0开始，从右往左以-1开始。
- 4、Python中的字符串不能改变。

### List(列表)

<font color="red">List（列表） 是 Python 中使用最频繁的数据类型。</font>

<font color="red">列表是写在方括号 **[]**之间、用逗号分隔开的元素列表。</font>

和字符串一样，列表同样可以被索引和截取，列表被截取后返回一个包含所需元素的新列表。

列表截取的语法格式如下：

```python
变量[头下标:尾下标]
```

索引值以 **0** 为开始值，**-1** 为从末尾的开始位置。

<img src="https://gitee.com/LyjYm/typora-photo/raw/master/photo/20210922211514.png" alt="List" style="zoom:50%;" />

加号 **+** 是列表连接运算符，星号 ***** 是重复操作。

~~~python
list = [ 'abcd', 786 , 2.23, 'runoob', 70.2 ]
tinylist = [123, 'runoob']

print (list)            # 输出完整列表
print (list[0])         # 输出列表第一个元素
print (list[1:3])       # 从第二个开始输出到第三个元素
print (list[2:])        # 输出从第三个元素开始的所有元素
print (tinylist * 2)    # 输出两次列表
print (list + tinylist) # 连接列表

结果：
['abcd', 786, 2.23, 'runoob', 70.2]
abcd
[786, 2.23]
[2.23, 'runoob', 70.2]
[123, 'runoob', 123, 'runoob']
['abcd', 786, 2.23, 'runoob', 70.2, 123, 'runoob']
~~~

与Python字符串不一样的是，列表中的元素是可以改变

~~~python
>>> a = [1, 2, 3, 4, 5, 6]
>>> a[0] = 9
>>> a[2:5] = [13, 14, 15]
>>> a
[9, 2, 13, 14, 15, 6]
>>> a[2:5] = []   # 将对应的元素值设置为 []
>>> a
[9, 2, 6]
~~~

<font color="red">**注意：**</font>

- 1、List写在方括号之间，元素用逗号隔开。
- 2、和字符串一样，list可以被索引和切片。
- 3、List可以使用+操作符进行拼接。
- 4、List中的元素是可以改变的。

Python 列表截取可以接收第三个参数，参数作用是截取的步长，以下实例在索引 1 到索引 4 的位置并设置为步长为 2（间隔一个位置）来截取字符串：

<img src="https://gitee.com/LyjYm/typora-photo/raw/master/photo/20210922211506.png" style="zoom: 80%;" />

~~~ python
# 通过空格将字符串分隔符，把各个单词分隔为列表
    inputWords = input.split(" ")
 
    # 翻转字符串
    # 假设列表 list = [1,2,3,4],  
    # list[0]=1, list[1]=2 ，而 -1 表示最后一个元素 list[-1]=4 ( 与 list[3]=4 一样)
    # inputWords[-1::-1] 有三个参数
    # 第一个参数 -1 表示最后一个元素
    # 第二个参数为空，表示移动到列表末尾
    # 第三个参数为步长，-1 表示逆向
    inputWords=inputWords[-1::-1]
 
    # 重新组合字符串
    output = ' '.join(inputWords)
     
    return output
 
if __name__ == "__main__":
    input = 'I like runoob'
    rw = reverseWords(input)
    print(rw)
    

结果：
runoob like I
~~~

### Tuple(元祖)

元组（tuple）与列表类似，不同之处在于元组的元素不能修改。元组写在小括号 **()** 里，元素之间用逗号隔开。

元组中的元素类型也可以不相同：

~~~python
tuple = ( 'abcd', 786 , 2.23, 'runoob', 70.2  )
tinytuple = (123, 'runoob')

print (tuple)             # 输出完整元组
print (tuple[0])          # 输出元组的第一个元素
print (tuple[1:3])        # 输出从第二个元素开始到第三个元素
print (tuple[2:])         # 输出从第三个元素开始的所有元素
print (tinytuple * 2)     # 输出两次元组
print (tuple + tinytuple) # 连接元组

结果：
('abcd', 786, 2.23, 'runoob', 70.2)
abcd
(786, 2.23)
(2.23, 'runoob', 70.2)
(123, 'runoob', 123, 'runoob')
('abcd', 786, 2.23, 'runoob', 70.2, 123, 'runoob')
~~~

元组与字符串类似，可以被索引且下标索引从0开始，-1 为从末尾开始的位置。也可以进行截取

其实，可以把字符串看作一种特殊的元组。

~~~python
>>> tup = (1, 2, 3, 4, 5, 6)
>>> print(tup[0])
1
>>> print(tup[1:5])
(2, 3, 4, 5)
>>> tup[0] = 11  # 修改元组元素的操作是非法的
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
>>>
~~~

虽然tuple的元素不可改变，但它可以包含可变的对象，比如list列表。

<font color="red">string、list 和 tuple 都属于 sequence（序列）。</font>

**注意：**

- 1、与字符串一样，元组的元素不能修改。
- 2、元组也可以被索引和切片，方法一样。
- 3、注意构造包含 0 或 1 个元素的元组的特殊语法规则。
- 4、元组也可以使用+操作符进行拼接。

### Set(集合)

集合（set）是由一个或数个形态各异的大小整体组成的，构成集合的事物或对象称作元素或是成员。

基本功能是进行成员关系测试和删除重复元素。

可以使用大括号 **{ }** 或者 **set()** 函数创建集合，注意：创建一个空集合必须用<font color="red"> **set()** </font>而不是 **{ }**，因为 **{ }** 是用来创建一个空字典。

~~~python
创建格式：
parame = {value01,value02,...}
或者
set(value)
~~~

~~~python
sites = {'Google', 'Taobao', 'Runoob', 'Facebook', 'Zhihu', 'Baidu'}

print(sites)   # 输出集合，重复的元素被自动去掉

# 成员测试
if 'Runoob' in sites :
    print('Runoob 在集合中')
else :
    print('Runoob 不在集合中')


# set可以进行集合运算
a = set('abracadabra')
b = set('alacazam')

print(a)

print(a - b)     # a 和 b 的差集

print(a | b)     # a 和 b 的并集

print(a & b)     # a 和 b 的交集

print(a ^ b)     # a 和 b 中不同时存在的元素

结果：
{'Zhihu', 'Baidu', 'Taobao', 'Runoob', 'Google', 'Facebook'}
Runoob 在集合中
{'b', 'c', 'a', 'r', 'd'}
{'r', 'b', 'd'}
{'b', 'c', 'a', 'z', 'm', 'r', 'l', 'd'}
{'c', 'a'}
{'z', 'b', 'm', 'r', 'l', 'd'}
~~~

### Dictionary（字典）

字典（dictionary）是Python中另一个非常有用的内置数据类型。

列表是有序的对象集合，字典是无序的对象集合。两者之间的区别在于：字典当中的元素是通过键来存取的，而不是通过偏移存取。

字典是一种映射类型，字典用 **{ }** 标识，它是一个无序的 **键(key) : 值(value)** 的集合。

键(key)必须使用不可变类型。

在同一个字典中，键(key)必须是唯一的。

~~~python
dict = {}
dict['one'] = "1 - 菜鸟教程"
dict[2]     = "2 - 菜鸟工具"

tinydict = {'name': 'runoob','code':1, 'site': 'www.runoob.com'}


print (dict['one'])       # 输出键为 'one' 的值
print (dict[2])           # 输出键为 2 的值
print (tinydict)          # 输出完整的字典
print (tinydict.keys())   # 输出所有键
print (tinydict.values()) # 输出所有值

结果：
1 - 菜鸟教程
2 - 菜鸟工具
{'name': 'runoob', 'code': 1, 'site': 'www.runoob.com'}
dict_keys(['name', 'code', 'site'])
dict_values(['runoob', 1, 'www.runoob.com'])
~~~

构造函数 dict() 可以直接从键值对序列中构建字典如下：

~~~python
>>> dict([('Runoob', 1), ('Google', 2), ('Taobao', 3)])
{'Runoob': 1, 'Google': 2, 'Taobao': 3}
>>> {x: x**2 for x in (2, 4, 6)}
{2: 4, 4: 16, 6: 36}
>>> dict(Runoob=1, Google=2, Taobao=3)
{'Runoob': 1, 'Google': 2, 'Taobao': 3}
>>>
~~~

**注意：**

- 1、字典是一种映射类型，它的元素是键值对。
- 2、字典的关键字必须为不可变类型，且不能重复。
- 3、创建空字典使用 **{ }**。

## Python注释

Python中的注释有单行注释和多行注释

<font color="red">单行注释</font>以 **#** 开头

~~~python
# 这是一个注释
print("Hello, World!")
~~~

<font color="red">多行注释</font>用三个单引号 **'''** 或者三个双引号 **"""** 将注释括起来

### 1、单引号（'''）

~~~python
'''
这是多行注释，用三个单引号
这是多行注释，用三个单引号 
这是多行注释，用三个单引号
'''
print("Hello, World!")
~~~

### 2、双引号（"""）

~~~python
"""
这是多行注释，用三个双引号
这是多行注释，用三个双引号 
这是多行注释，用三个双引号
"""
print("Hello, World!")
~~~

## Python运算符

Python 语言支持以下类型的运算符:

- [[算术运算符]](# 算术运算符)
- [[比较（关系）运算符]](# 比较运算符)
- [[赋值运算符]](# 赋值运算符)
- [逻辑运算符]
- [位运算符]
- [成员运算符]
- [身份运算符]
- [运算符优先级]

### 算术运算符

以下假设变量a为10，变量b为21：

| 运算符 | 描述                                            | 实例                                 |
| :----- | :---------------------------------------------- | :----------------------------------- |
| +      | 加 - 两个对象相加                               | a + b 输出结果 31                    |
| -      | 减 - 得到负数或是一个数减去另一个数             | a - b 输出结果 -11                   |
| *      | 乘 - 两个数相乘或是返回一个被重复若干次的字符串 | a * b 输出结果 210                   |
| /      | 除 - x 除以 y                                   | b / a 输出结果 2.1                   |
| %      | 取模 - 返回除法的余数                           | b % a 输出结果 1                     |
| **     | 幂 - 返回x的y次幂                               | a**b 为10的21次方                    |
| //     | 取整除 - 向下取接近商的整数                     | >>> 9//2     4     >>> -9//2      -5 |

Python所有算术运算符的操作

~~~python
a = 21
b = 10
c = 0
 
c = a + b
print ("1 - c 的值为：", c)
 
c = a - b
print ("2 - c 的值为：", c)
 
c = a * b
print ("3 - c 的值为：", c)
 
c = a / b
print ("4 - c 的值为：", c)
 
c = a % b
print ("5 - c 的值为：", c)
 
# 修改变量 a 、b 、c
a = 2
b = 3
c = a**b 
print ("6 - c 的值为：", c)
 
a = 10
b = 5
c = a//b 
print ("7 - c 的值为：", c)

结果：
1 - c 的值为： 31
2 - c 的值为： 11
3 - c 的值为： 210
4 - c 的值为： 2.1
5 - c 的值为： 1
6 - c 的值为： 8
7 - c 的值为： 2
~~~

### 比较运算符

以下假设变量a为10，变量b为20：

| 运算符 | 描述                                                         | 实例                                         |
| ------ | ------------------------------------------------------------ | -------------------------------------------- |
| ==     | 等于 - 比较对象是否相等                                      | (a == b) 返回 False。                        |
| !=     | 不等于 - 比较两个对象是否不相等                              | (a != b) 返回 true.                          |
| ~~<>~~ | ~~不等于 -  比较两个对象是否不相等。python3 <font color="red">已废弃</font>。~~ | ~~(a <> b) 返回 true。这个运算符类似 != 。~~ |
| >      | 大于 - 返回x是否大于y                                        | (a > b) 返回 False。                         |
| <      | 小于 - 返回x是否小于y。所有比较运算符返回1表示真，返回0表示假。这分别与特殊的变量True和False等价。 | (a < b) 返回 true。                          |
| >=     | 大于等于	- 返回x是否大于等于y。                           | (a >= b) 返回 False。                        |
| <=     | 小于等于 -	返回x是否小于等于y。                           | (a <= b) 返回 true。                         |

~~~python
a = 21
b = 10
c = 0
 
if  a == b :
   print "1 - a 等于 b"
else:
   print "1 - a 不等于 b"
 
if  a != b :
   print "2 - a 不等于 b"
else:
   print "2 - a 等于 b"

 
if  a < b :
   print "4 - a 小于 b" 
else:
   print "4 - a 大于等于 b"
 
if  a > b :
   print "5 - a 大于 b"
else:
   print "5 - a 小于等于 b"
 
# 修改变量 a 和 b 的值
a = 5
b = 20
if  a <= b :
   print "6 - a 小于等于 b"
else:
   print "6 - a 大于  b"
 
if  b >= a :
   print "7 - b 大于等于 a"
else:
   print "7 - b 小于 a"

结果：
1 - a 不等于 b
2 - a 不等于 b
3 - a 大于等于 b
4 - a 大于 b
5 - a 小于等于 b
6 - b 大于等于 a
~~~

### 赋值运算符

以下假设变量a为10，变量b为20：

| 运算符 | 描述                                                         | 实例                                                         |
| :----- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| =      | 简单的赋值运算符                                             | c = a + b 将 a + b 的运算结果赋值为 c                        |
| +=     | 加法赋值运算符                                               | c += a 等效于 c = c + a                                      |
| -=     | 减法赋值运算符                                               | c -= a 等效于 c = c - a                                      |
| *=     | 乘法赋值运算符                                               | c *= a 等效于 c = c * a                                      |
| /=     | 除法赋值运算符                                               | c /= a 等效于 c = c / a                                      |
| %=     | 取模赋值运算符                                               | c %= a 等效于 c = c % a                                      |
| **=    | 幂赋值运算符                                                 | c**= a 等效于 c = c ** a(注意**)                             |
| //=    | 取整除赋值运算符                                             | c //= a 等效于 c = c // a                                    |
| :=     | 海象运算符，可在表达式内部为变量赋值。**Python3.8 版本新增运算符**。 | 在这个示例中，赋值表达式可以避免调用 len() 两次:`if (n := len(a)) > 10:    print(f"List is too long ({n} elements, expected <= 10)")` |

~~~python
 
a = 21
b = 10
c = 0
 
c = a + b
print ("1 - c 的值为：", c)
 
c += a
print ("2 - c 的值为：", c)
 
c *= a
print ("3 - c 的值为：", c)
 
c /= a 
print ("4 - c 的值为：", c)
 
c = 2
c %= a
print ("5 - c 的值为：", c)
 
c **= a
print ("6 - c 的值为：", c)
 
c //= a
print ("7 - c 的值为：", c)

结果
~~~





# Python练手

## 小猪佩奇

~~~python
# coding: utf-8

import turtle as t

t.screensize(400, 300)
t.pensize(4) # 设置画笔的大小
t.colormode(255) # 设置GBK颜色范围为0-255
t.color((255,155,192),"pink") # 设置画笔颜色和填充颜色(pink)
t.setup(840,500) # 设置主窗口的大小为840*500
t.speed(10) # 设置画笔速度为10
#鼻子
t.pu() # 提笔
t.goto(-100,100) # 画笔前往坐标(-100,100)
t.pd() # 下笔
t.seth(-30) # 笔的角度为-30°
t.begin_fill() # 外形填充的开始标志
a=0.4
for i in range(120):
   if 0<=i<30 or 60<=i<90:
       a=a+0.08
       t.lt(3) #向左转3度
       t.fd(a) #向前走a的步长
   else:
       a=a-0.08
       t.lt(3)
       t.fd(a)
t.end_fill() # 依据轮廓填充
t.pu() # 提笔
t.seth(90) # 笔的角度为90度
t.fd(25) # 向前移动25
t.seth(0) # 转换画笔的角度为0
t.fd(10)
t.pd()
t.pencolor(255,155,192) # 设置画笔颜色
t.seth(10)
t.begin_fill()
t.circle(5) # 画一个半径为5的圆
t.color(160,82,45) # 设置画笔和填充颜色
t.end_fill()
t.pu()
t.seth(0)
t.fd(20)
t.pd()
t.pencolor(255,155,192)
t.seth(10)
t.begin_fill()
t.circle(5)
t.color(160,82,45)
t.end_fill()
#头
t.color((255,155,192),"pink")
t.pu()
t.seth(90)
t.fd(41)
t.seth(0)
t.fd(0)
t.pd()
t.begin_fill()
t.seth(180)
t.circle(300,-30) # 顺时针画一个半径为300,圆心角为30°的园
t.circle(100,-60)
t.circle(80,-100)
t.circle(150,-20)
t.circle(60,-95)
t.seth(161)
t.circle(-300,15)
t.pu()
t.goto(-100,100)
t.pd()
t.seth(-30)
a=0.4
for i in range(60):
   if 0<=i<30 or 60<=i<90:
       a=a+0.08
       t.lt(3) #向左转3度
       t.fd(a) #向前走a的步长
   else:
       a=a-0.08
       t.lt(3)
       t.fd(a)
t.end_fill()
#耳朵
t.color((255,155,192),"pink")
t.pu()
t.seth(90)
t.fd(-7)
t.seth(0)
t.fd(70)
t.pd()
t.begin_fill()
t.seth(100)
t.circle(-50,50)
t.circle(-10,120)
t.circle(-50,54)
t.end_fill()
t.pu()
t.seth(90)
t.fd(-12)
t.seth(0)
t.fd(30)
t.pd()
t.begin_fill()
t.seth(100)
t.circle(-50,50)
t.circle(-10,120)
t.circle(-50,56)
t.end_fill()
#眼睛
t.color((255,155,192),"white")
t.pu()
t.seth(90)
t.fd(-20)
t.seth(0)
t.fd(-95)
t.pd()
t.begin_fill()
t.circle(15)
t.end_fill()
t.color("black")
t.pu()
t.seth(90)
t.fd(12)
t.seth(0)
t.fd(-3)
t.pd()
t.begin_fill()
t.circle(3)
t.end_fill()
t.color((255,155,192),"white")
t.pu()
t.seth(90)
t.fd(-25)
t.seth(0)
t.fd(40)
t.pd()
t.begin_fill()
t.circle(15)
t.end_fill()
t.color("black")
t.pu()
t.seth(90)
t.fd(12)
t.seth(0)
t.fd(-3)
t.pd()
t.begin_fill()
t.circle(3)
t.end_fill()
#腮
t.color((255,155,192))
t.pu()
t.seth(90)
t.fd(-95)
t.seth(0)
t.fd(65)
t.pd()
t.begin_fill()
t.circle(30)
t.end_fill()
#嘴
t.color(239,69,19)
t.pu()
t.seth(90)
t.fd(15)
t.seth(0)
t.fd(-100)
t.pd()
t.seth(-80)
t.circle(30,40)
t.circle(40,80)
#身体
t.color("red",(255,99,71))
t.pu()
t.seth(90)
t.fd(-20)
t.seth(0)
t.fd(-78)
t.pd()
t.begin_fill()
t.seth(-130)
t.circle(100,10)
t.circle(300,30)
t.seth(0)
t.fd(230)
t.seth(90)
t.circle(300,30)
t.circle(100,3)
t.color((255,155,192),(255,100,100))
t.seth(-135)
t.circle(-80,63)
t.circle(-150,24)
t.end_fill()
#手
t.color((255,155,192))
t.pu()
t.seth(90)
t.fd(-40)
t.seth(0)
t.fd(-27)
t.pd()
t.seth(-160)
t.circle(300,15)
t.pu()
t.seth(90)
t.fd(15)
t.seth(0)
t.fd(0)
t.pd()
t.seth(-10)
t.circle(-20,90)
t.pu()
t.seth(90)
t.fd(30)
t.seth(0)
t.fd(237)
t.pd()
t.seth(-20)
t.circle(-300,15)
t.pu()
t.seth(90)
t.fd(20)
t.seth(0)
t.fd(0)
t.pd()
t.seth(-170)
t.circle(20,90)
#脚
t.pensize(10)
t.color((240,128,128))
t.pu()
t.seth(90)
t.fd(-75)
t.seth(0)
t.fd(-180)
t.pd()
t.seth(-90)
t.fd(40)
t.seth(-180)
t.color("black")
t.pensize(15)
t.fd(20)
t.pensize(10)
t.color((240,128,128))
t.pu()
t.seth(90)
t.fd(40)
t.seth(0)
t.fd(90)
t.pd()
t.seth(-90)
t.fd(40)
t.seth(-180)
t.color("black")
t.pensize(15)
t.fd(20)
#尾巴
t.pensize(4)
t.color((255,155,192))
t.pu()
t.seth(90)
t.fd(70)
t.seth(0)
t.fd(95)
t.pd()
t.seth(0)
t.circle(70,20)
t.circle(10,330)
t.circle(70,30)
t.done()
~~~

## 九九乘法表

~~~python
for i in range(1,10):
    for j in range(1,i+1):
        print('%s*%s=%s' %(i,j,i*j),end = ' ')
    print()
~~~

## 猜单词

~~~python
import random

print("欢迎来到猜单词游戏")
words = ("python" , "hello","game","world","random") # 单词序列元组
jumble = ''
iscontinue = 'y'
while iscontinue.lower()=='y':
    word = random.choice(words) # 从单词元组中随机挑选一个单词
    correct = word # 利用新变量保持挑选的单词，用于之后的比较
    jumble = ""
    while word:
        position = random.randrange(len(word)) # 从单词中随机选取一个字符位置
        jumble += word[position] # 将字符拼接
        word = word[:position] + word[(position+1):] # 移除字符
    print(jumble)
    guess = input("输入你认为的单词：")
    while True:
        if guess == correct:
            print("猜对了！")
            iscontinue = input("是否继续（Y/N）：")
            break
        elif guess == ' ':
            exit(0)
        else:
            print("猜错了（输入空格可退出）")
            guess = input("继续猜：")
~~~

## 猜拳

~~~python
import random  # 导入随机模块

num = 1
yin_num = 0
shu_num = 0
while num <= 3:
    if shu_num == 2 or yin_num == 2:
        break
    user = int(input('请出拳 0（石头） 1（剪刀） 2（布）'))
    if user > 2:
        print('不能出大于2的值')
    else:
        data = ['石头', '剪刀', '布']
        com = random.randint(0, 2)
        print("您出的是{}，电脑出的是{}".format(data[user], data[com]))
        if user == com:
            print('平局')
            continue
        elif (user == 0 and com == 1) or (user == 1 and com == 2) or (user == 2 and com == 0):
            print('你赢了')
            yin_num += 1
        else:
            print('你输了')
            shu_num += 1
    num += 1
~~~

## 时间

```python
import turtle
from datetime import *


# 抬起画笔，向前运动一段距离放下
def Skip(step):
    turtle.penup()
    turtle.forward(step)
    turtle.pendown()


def mkHand(name, length):
    # 注册Turtle形状，建立表针Turtle
    turtle.reset()
    Skip(-length * 0.1)
    # 开始记录多边形的顶点。当前的乌龟位置是多边形的第一个顶点。
    turtle.begin_poly()
    turtle.forward(length * 1.1)
    # 停止记录多边形的顶点。当前的乌龟位置是多边形的最后一个顶点。将与第一个顶点相连。
    turtle.end_poly()
    # 返回最后记录的多边形。
    handForm = turtle.get_poly()
    turtle.register_shape(name, handForm)


def Init():
    global secHand, minHand, hurHand, printer
    # 重置Turtle指向北
    turtle.mode("logo")
    # 建立三个表针Turtle并初始化
    mkHand("secHand", 135)
    mkHand("minHand", 125)
    mkHand("hurHand", 90)
    secHand = turtle.Turtle()
    secHand.shape("secHand")
    minHand = turtle.Turtle()
    minHand.shape("minHand")
    hurHand = turtle.Turtle()
    hurHand.shape("hurHand")

    for hand in secHand, minHand, hurHand:
        hand.shapesize(1, 1, 3)
        hand.speed(0)

    # 建立输出文字Turtle
    printer = turtle.Turtle()
    # 隐藏画笔的turtle形状
    printer.hideturtle()
    printer.penup()


def SetupClock(radius):
    # 建立表的外框
    turtle.reset()
    turtle.pensize(7)
    for i in range(60):
        Skip(radius)
        if i % 5 == 0:
            turtle.forward(20)
            Skip(-radius - 20)

            Skip(radius + 20)
            if i == 0:
                turtle.write(int(12), align="center", font=("Courier", 14, "bold"))
            elif i == 30:
                Skip(25)
                turtle.write(int(i / 5), align="center", font=("Courier", 14, "bold"))
                Skip(-25)
            elif (i == 25 or i == 35):
                Skip(20)
                turtle.write(int(i / 5), align="center", font=("Courier", 14, "bold"))
                Skip(-20)
            else:
                turtle.write(int(i / 5), align="center", font=("Courier", 14, "bold"))
            Skip(-radius - 20)
        else:
            turtle.dot(5)
            Skip(-radius)
        turtle.right(6)


def Week(t):
    week = ["星期一", "星期二", "星期三",
            "星期四", "星期五", "星期六", "星期日"]
    return week[t.weekday()]


def Date(t):
    y = t.year
    m = t.month
    d = t.day
    return "%s %d%d" % (y, m, d)


def Tick():
    # 绘制表针的动态显示
    t = datetime.today()
    second = t.second + t.microsecond * 0.000001
    minute = t.minute + second / 60.0
    hour = t.hour + minute / 60.0
    secHand.setheading(6 * second)
    minHand.setheading(6 * minute)
    hurHand.setheading(30 * hour)

    turtle.tracer(False)
    printer.forward(65)
    printer.write(Week(t), align="center",
                  font=("Courier", 14, "bold"))
    printer.back(130)
    printer.write(Date(t), align="center",
                  font=("Courier", 14, "bold"))
    printer.home()
    turtle.tracer(True)

    # 100ms后继续调用tick
    turtle.ontimer(Tick, 100)


def main():
    # 打开/关闭龟动画，并为更新图纸设置延迟。
    turtle.tracer(False)
    Init()
    SetupClock(160)
    turtle.tracer(True)
    Tick()
    turtle.mainloop()


if __name__ == "__main__":
    main()
```

## 下载音乐

~~~python
import json, requests, re, time

# 伪装浏览器头部，可通用
headers = {
    'user-agent': 'Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/63.0.3239.132 Safari/537.36'}

# 外层循环，保证每次下载结束后 可控制程序是否结束
songname = input('请输入你要下载的歌曲名称：')
key_word = ''  # 用于保存输入的歌曲名
begin_while = True  # 开启项目循环
while begin_while:
    key_word = songname
    # 找数据，数据藏在js文件里 callback=jQuery112404079296929561982_1579355885832&可以去掉，去掉后，就是完整的json字符串，如加上则需要截取字符串为JSON格式字符串
    res_music = requests.get(
        'https://songsearch.kugou.com/song_search_v2?keyword={}&page=1&pagesize=30&userid=-1&clientver=&platform=WebFilter&tag=em&filter=2&iscorrection=1&privilege_filter=0&_=1579355491182'.format(
            songname))
    # 清理多余的参数后的url，也能获取到数据
    # res_music = requests.get('https://songsearch.kugou.com/song_search_v2?keyword={}&platform=WebFilter'.format(songname))

    json_music = res_music.json()
    music_list = json_music['data']['lists']

    # 利用enumerate函数，将下标作为序号，方便输入选择
    music_hash_dic = {}
    number = 0
    for y, x in enumerate(music_list):
        number = y + 1
        songname = x['FileName'].replace('<em>', '').replace('</em>', '')  # 去掉数据里<em></em>标签
        songhash = x['FileHash']
        songid = x['AlbumID']
        music_hash_dic[str(y + 1)] = [songhash, songid, songname]  # 将序号作为键，把hash，id，歌曲名称作为字典键值
        print(str(y + 1) + '  |  ' + songname)

    # 下面的url有部分没用，但需要带cookies，建议用此url较方便
    print('=' * 25)  # 25个'='
    user_choice = ''
    if number > 0:
        user_choice = input('请输入你要下载的歌曲序号：')
    while True:  # 内层循环
        if user_choice.isdigit():
            if int(user_choice) > 0 and int(user_choice) <= number:
                user_choice = str(int(user_choice))  # 去掉 01前面的0
                hash_url = 'https://wwwapi.kugou.com/yy/index.php?r=play/getdata&callback=jQuery1910017709359422550808_1579413714166&hash={}&album_id={}&dfid=2AZfAI3Sfdln0Z7zq545MRQu&mid=6bcee3705b8816d0cba7e664a5d55ea7&platid=4&_=1579228034450'.format(
                    music_hash_dic[user_choice][0], music_hash_dic[user_choice][1])
                soup = requests.get(hash_url, headers=headers)
                # 获取到最后完整的下载链接
                song_download = ''.join(soup.text.split('https')[-1:]).replace('\\', '').replace('://', '').replace(
                    '"}});', '')
                finall_url = 'https://' + '{}'.format(song_download)
                if '\"err_code\":0' in soup.text:  # 下载过多，酷狗方会限制IP，可用代理IP池解决，此处不拓展，后面教程会专门提供
                    if 'mp3' in finall_url:
                        # 下载歌曲
                        with open(music_hash_dic[user_choice][2] + '.mp3', 'wb') as f:
                            print(music_hash_dic[user_choice][2], '正在下载...')
                            f.write(requests.get(finall_url).content)
                            print(music_hash_dic[user_choice][2], '下载完成，歌曲在执行文件，同一级路径')
                            print('=' * 25)  # 25个'='
                            # break
                    else:
                        print('=' * 25)  # 25个'='
                        print("此歌曲为VIP版本请选择其他版本")
                        print('=' * 25)  # 25个'='
                        songname = key_word  # 把搜索的歌曲名称重新传给 songname，达到重新加载循环歌曲列表
                        print("重新读取歌曲列表中...")
                        time.sleep(3)
                        break
                else:
                    print("由于酷狗限制，下载次数过多，请稍候尝试")
                user_choice_again = input('输入【8】结束使用，其他任意键继续下载歌曲：    ')
                if user_choice_again != '8':
                    songname = input('请输入你要下载的歌曲名称：')
                    break  # 关闭内层循环  注意此时外层循环是没有关闭的
                else:
                    print('=' * 25)  # 25个'='
                    print('再见，欢迎下次使用，退出中...')
                    time.sleep(3)
                    begin_while = False  # 关闭项目循环，注意外层循环关闭
                    break  # 关闭内层循环

            else:
                user_choice = input('请输入列表中的序号，再次输入歌曲序号：')
        else:
            user_choice = input('输入有误，请输入你要下载的歌曲序号：')
    else:
        songname = input('未找到歌曲，请重新输入你要下载的歌曲名称：')
~~~

## 豆瓣电影T250

~~~python
import re
import requests
from bs4 import BeautifulSoup
from openpyxl import Workbook
from openpyxl.styles import Alignment

def top250():
        wb = Workbook()
        ws = wb['Sheet']
        num = 0
        num1 = 0
        lst = []
        name_lst = []
        dy_lst = []
        zy_lst = []
        time_lst = []
        country_lst = []
        leixing_lst = []
        pj_lst = []
        people_lst = []
        quote_lst = []
        headers = {
                "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9",
                "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/81.0.4044.129 Safari/537.36 Edg/81.0.416.68",
        }
        while num <= 225:
                url = 'https://movie.douban.com/top250?start=' + str(num) + '&filter='
                with requests.get(url=url, headers=headers) as r:
                        if r.status_code == 200:
                                r.encoding = r.apparent_encoding
                                soup = BeautifulSoup(r.text, 'html.parser')
                                ol = soup.find('ol')
                                li = ol.find_all('li')
                                for i in li:
                                        name = i.find('span').text
                                        p = i.find('p')
                                        p = str(p).split('<br/>', 1)
                                        fst = p[0].split('>', 1)[1]
                                        sec = p[1].split('<', 1)[0]
                                        daoyan = fst.split('主', 1)[0]
                                        daoyan = daoyan.replace('导演:', '')
                                        daoyan = daoyan.replace('\xa0', '')
                                        daoyan = daoyan.replace('\n                             ', '')
                                        try:
                                                zhuyan = fst.split('主演:', 1)[1]
                                        except:
                                                zhuyan = ''
                                        time = sec.split(' / ', 2)[0]
                                        time = time.replace('\xa0', '')
                                        time = time.replace('\n                            ', '')
                                        time = re.findall(r'\d{4}', time)[-1]
                                        country = sec.split('\xa0/\xa0', 2)[1]
                                        country = country.replace('\xa0', '')
                                        type = sec.split('\xa0/\xa0', 2)[2]
                                        type = type.replace('\xa0', '')
                                        type = type.replace('\n                        ', '')
                                        star = i.find('div', attrs={'class': "star"})
                                        span = star.find_all('span')
                                        pingjia = span[1].text
                                        people = span[3].text.split('评价', 1)[0]
                                        try:
                                                quote = i.find('p', attrs={'class': "quote"}).text
                                                quote = quote.replace('\n', '')
                                        except:
                                                quote = ''
                                        name_lst.append(name)
                                        dy_lst.append(daoyan)
                                        zy_lst.append(zhuyan)
                                        time_lst.append(time)
                                        country_lst.append(country)
                                        leixing_lst.append(type)
                                        pj_lst.append(pingjia)
                                        people_lst.append(people)
                                        quote_lst.append(quote)

                                num1 += 1
                                print('第{}页爬取完毕！'.format(num1))
                                if num == 225:
                                        print('爬取结束，开始写入excel。。。')
                                        paiming = list(range(1, 251))
                                        lst.append(paiming)
                                        lst.append(name_lst)
                                        lst.append(dy_lst)
                                        lst.append(zy_lst)
                                        lst.append(time_lst)
                                        lst.append(country_lst)
                                        lst.append(leixing_lst)
                                        lst.append(pj_lst)
                                        lst.append(people_lst)
                                        lst.append(quote_lst)
                                        head = ['排名', '电影名称', '导演', '主演', '年份', '地区', '类型', '评分', '评价人数', '一句简介']
                                        ws.append(head)
                                        for i in range(len(lst)):
                                                for j in range(len(lst[0])):
                                                        ws.cell(j + 2, i + 1).value = lst[i][j]
                                        print('写入excel完成！')
                                        for cell in ws['1']:
                                                cell.alignment = Alignment(horizontal='center', vertical='center')
                                        for cell in ws['A']:
                                                cell.alignment = Alignment(horizontal='center', vertical='center')
                                        for cell in ws['B']:
                                                cell.alignment = Alignment(horizontal='center', vertical='center')
                                        for cell in ws['E']:
                                                cell.alignment = Alignment(horizontal='center', vertical='center')
                                        for cell in ws['H']:
                                                cell.alignment = Alignment(horizontal='center', vertical='center')
                                        for cell in ws['I']:
                                                cell.alignment = Alignment(horizontal='center', vertical='center')
                                        ws.column_dimensions['B'].width = 25
                                        ws.column_dimensions['I'].width = 13
                                        wb.save('豆瓣电影top250.xlsx')
                                num += 25
                        else:
                                print('失败！')

if __name__ == '__main__':
        print('开始爬取！')
        top250()
~~~
