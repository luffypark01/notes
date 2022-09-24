## 10.正则表达式Re模块使用技巧:

(1)5种最常用的“匹配方法”：

```golo
import re
# 最常用的匹配语法:
re.match #从“头”开始匹配
re.search #从整个文本，匹配包含
re.findall#把所有匹配到的 字符 放到以列表中的元素返回 >>>无 group方法
re.split #以匹配到的字符当做列表分隔符
re.sub #匹配字符并替换
TIP:windows上面，可以给一个句柄，再用“.group”去返回值
```

1)re.match:(以"字符"开头，进行匹配)

```stylus
import re
print(re.match("chen",'chenglong'))
```

**>>>**<re.Match object; span=(0, 4), match=‘chen’>

```stylus
import re
print(re.match("chen",'dachenglong'))
```

**>>>**None

**所以：re.match是…"chen"恰好是“chenlong”的开头；
是的话，返回值；不是，None**

2)re.search:(用的最多，是整个字符串里面去“找”)

```stylus
import re
print(re.search("chen","woshichenglong"))
```

**>>>**<re.Match object; span=(5, 9), match=‘chen’>

3)re.findall:(整个字符串里面去“找”，把所有组合，以"列表"的形式返回)

```gradle
import re
print(re.findall("5a",'asd5asd5asdas5asadas'))
```

**>>>**[‘5a’, ‘5a’, ‘5a’]

4)re.split:(整个字符串里面去“找”，以“字符”进行分割，返回列表)

```stylus
import re
print(re.split("5a",'asd5asd5asdas5asadas'))
```

**>>>**[‘asd’, ‘sd’, ‘sdas’, ‘sadas’]

5)re.sub:(整个字符串里面去“找”，以“字符”进行代替)

```routeros
import re
print(re.sub("5a",'666','asd5asd5asdas5asadas',count=2))
```

**>>>**asd666sd666sdas5asadas

(2)常用的“表达式符号”：

| 符号           | 使用                          |
| -------------- | ----------------------------- |
| “ ^ ”          | 来匹配字符“开头”，和match一样 |
| “ . ”          | 匹配除“ \ n ”的任意字符       |
| “ $ ”          | 匹配字符“结尾”                |
| “ * ”          | 匹配 * 前面的字符，0次或多次  |
| “ + ”          | 匹配前面的字符，一次或多次    |
| “ ？”          | 匹配 ？前面的字符 一次或0次   |
| “ {m} ”        | 匹配前面字符，m次             |
| “{n,m}”        | 匹配前面字符，n~m次           |
| “ I ”          | 匹配 “I” 或左或右的字符       |
| “(…)”          | 分组匹配                      |
|                |                               |
|                |                               |
| “ \A ”         | 以字符开头匹配                |
| “ \ Z ”        | 以字符结尾匹配                |
| “ \d ”         | 匹配数字 0~9                  |
| " \D "         | 匹配非数字                    |
| “ \w ”         | 匹配 大小写字母 + 数字        |
| “ \W ”         | 匹配非 大小写 + 数字          |
| “ \s ”         | 匹配空白字符、\t、\n、\r      |
| “(?P< id >…))” | 装逼专用，分组的字典匹配      |

\1) " ^ ":(以"字符"开头，进行匹配)

```stylus
import re
print(re.search("^chen","chenlong"))
print(re.search("^chen","woshichenglongchenlong"))
```

**>>>**<re.Match object; span=(0, 4), match=‘chen’>
None

\2) “ . ”:(以"非 \n 字符"，进行匹配)；" . "(表示一个)；
“ .+ ”(表示多个)

```stylus
import re
print(re.search(".","asdasdasad"))
print(re.search(".+","asdas\ndasad"))
```

**>>>**<re.Match object; span=(0, 1), match=‘a’>
<re.Match object; span=(0, 5), match=‘asdas’>

\3) “ $ ”:(以前面字符结尾)

```stylus
import re
print(re.search("g+$","chenlongggg"))
print(re.search("g$","chenlongchen"))
```

**>>>**<re.Match object; span=(7, 11), match=‘gggg’>
None

\4) “ * ”:(以前面字符0次或多次)

```stylus
import re
print(re.search("add*",'ad'))
print(re.search("add*",'add'))
print(re.search("add*",'addddd'))
```

**>>>**<re.Match object; span=(0, 2), match=‘ad’>

<re.Match object; span=(0, 3), match=‘add’>

<re.Match object; span=(0, 6), match=‘addddd’>

\5) “ + ”:(以前面字符1次或多次)

```stylus
import re
print(re.search('ad+',"aa"))
print(re.search('ad+',"ad"))
print(re.search('ad+',"adddd"))
```

**>>>**None

<re.Match object; span=(0, 2), match=‘ad’>

<re.Match object; span=(0, 5), match=‘adddd’>

\6) “ ？ ”:(以前面字符1次或0次)

```stylus
import re
print(re.search("ad?",'a'))
print(re.search("ad?",'ad'))
print(re.search("ad?",'add'))
```

**>>>**<re.Match object; span=(0, 1), match=‘a’>

<re.Match object; span=(0, 2), match=‘ad’>

<re.Match object; span=(0, 2), match=‘ad’>

\7) “ {m} / {n,m} ” 、“(…)”:(以前面字符m次 或 n~m次)、(分组)

```stylus
import re
print(re.search("(ad){2}","adadadadad"))
print(re.search("(ad){2,6}","adadadadad"))
```

**>>>**<re.Match object; span=(0, 4), match=‘adad’>
<re.Match object; span=(0, 10), match=‘adadadadad’>

\8) “ I ”:(以前面字符 左或者右)

```stylus
import re
print(re.search("abc|ABC",'ABCabcadfas'))
print(re.search("ABC|abc",'ABCabcadfas'))
```

**>>>**<re.Match object; span=(0, 3), match=‘ABC’>
<re.Match object; span=(0, 3), match=‘ABC’>

\9) “ 其他 ”:

```routeros
#Author:Jony c
#!/usr/bin/env  python 
# -*- coding:utf-8 -*-
import re
print(re.search("\Aaa","aasdasdas"))
print(re.search("a\Z","aasdasdasa"))
print(re.search("\d","aasdasdasa"))
print(re.search("\D+","aasdasdasa"))
print(re.search("\w+","aaASD1235asdasa"))
print(re.search("\w+","aaASD1235as##dasa"))
print(re.search("\W","aaASD1235asdasa"))
print(re.search("\s","aaASD 1235asdasa"))
print(re.search("\S+","aaASD 1235asdasa"))#大写“\S”，非\t\n\r..这些...
#装逼操作：
print(re.search("(?P<id>[A-Z]+)",'asASSFASCA1234').groupdict())#“P”大写
```

**>>>**<re.Match object; span=(0, 2), match=‘aa’>

<re.Match object; span=(9, 10), match=‘a’>

None

<re.Match object; span=(0, 10), match=‘aasdasdasa’>

<re.Match object; span=(0, 15), match=‘aaASD1235asdasa’>

<re.Match object; span=(0, 11), match=‘aaASD1235as’>

None

<re.Match object; span=(5, 6), match=’ '>

<re.Match object; span=(0, 5), match=‘aaASD’>

{‘id’: ‘ASSFASCA’}

\10) “ 转义 ”:

```stylus
import re
print(re.search("(ad){2}(\|=\|=)","adad|=|="))
```

**>>>**" | " ,前面要加 " \ " ,转义，表示 “ | ” 在这里没有特殊意义

(3)“匹配模式”：

| 模式 | 使用                                         |
| ---- | -------------------------------------------- |
| re.I | 忽视大小写                                   |
| re.M | 改变 “ ^ ” / " $ "的行为                     |
| re.S | 改变**“ . ”**；让**“ . ”**可以匹配到换行符； |

1)“re.I”：

```stylus
print(re.search("ad",'dADdddd',re.I))#忽视大小写..
```

**>>>**print(re.search(“ad”,‘dADdddd’,re.I))#忽视大小写…

2)“re.M”：

```stylus
print(re.search("^a",'dadADdddd',re.M))
print(re.search("^a",'\nada',re.M))
print(re.search("a$",'\nada\n',re.M))
print(re.search("a$",'\nadad',re.M))
```

**>>>**好像只对前面加 “\n” ，改变了行为，(有大佬懂得话，麻烦cue我)

3)“re.S”：

```stylus
import re
print(re.search(".+","\nsadasd",re.S))
```

**>>>**<re.Match object; span=(0, 7), match=’\nsadasd’>

(4)“贪婪匹配”和“惰性匹配”：

| 匹配模式 |          |
| -------- | -------- |
| 贪婪匹配 | **.\***  |
| 惰性匹配 | **.\*?** |

[开源中国，可以进行Re测试](https://tool.oschina.net/regex/)

**1.贪婪匹配：**![在这里插入图片描述](https://img-blog.csdnimg.cn/20210429110259122.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDk0MTM4NQ==,size_16,color_FFFFFF,t_70)**2.惰性匹配：**![在这里插入图片描述](https://img-blog.csdnimg.cn/20210429110505252.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NDk0MTM4NQ==,size_16,color_FFFFFF,t_70)

**.\*?和.\*+	是什么意思**

后边多一个？表示懒惰模式。
必须跟在*或者+后边用
如：\<img src="test.jpg" width="60px" height="80px"/>
如果用正则匹配src中内容非懒惰模式匹配
src=".\*"
匹配结果是：src="test.jpg" width="60px" height="80px"
意思是从="往后匹配，直到最后一个"匹配结束

懒惰模式正则：
src=".*?"
结果：src="test.jpg"
因为匹配到第一个"就结束了一次匹配。不会继续向后匹配。因为他懒惰嘛。

.表示除\n之外的任意字符
*表示匹配0-无穷
+表示匹配1-无穷

去掉括号实例

```

<span style="font-size:14px;">public class Test {
  public static void main(String[] args) {
	String s = "图片(img=32,34)http://www.sds.com/jpg(/img)图片(img=32,34)http://www.sds.com/jpg(/img)"; 
	System.out.println(s.replaceAll("\\[.+?\\]",""));
  }
}</span>
```

