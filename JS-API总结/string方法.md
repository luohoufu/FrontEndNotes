[TOC]



#一、查找方法

##1 字符方法
###charAt
###charCodeAt
返回指定位置的字符的unicode编码
###fromCharCode
根据字符编码创建字符串

string.fromCharCode(numx,numx,numx)
##2 位置方法
###indexOf
从前向后检索字符串，看第一次出现子串的下标
###lastIndexOf
从后向前查看  
##3 匹配方法
###match 找到一个或多个正则表达式的匹配 没找到返回Null

* a.  若有g，则返回数组所匹配的子串的数组，没找到返回NUll,没有派生属性，不提供与子表达式匹配的文本信息，不声明匹配穿的位置；可以用RegExp.exec()方法代替.
* b.  没有g，执行一次匹配，数组 array[0] 匹配文本  array[...]匹配子串，即（）中的匹配
* c.  数组属性：input 调用该方法的字符串对象；index ：匹配文本的起始字符在字符串中的下标；lastIndex：匹配文本的末尾字符串在字符串中的位置

例子：

* document.write(str.match("world"))  结果：world
* var str="1 plus 2 equal 3"；
* document.write(str.match(/\d+/g)) 结果1,2,3[全局匹配的正则表达式检索字符串中的所有数字]

###search
* 参数：正则表达式
* 返回值：**字符串第一个与正则表达式匹配的起始下标**
* 未找到：返回-1
* 忽略全局标记g和lastindex属性，总是从第一个开始检索
###replace
* 参数：需要替换的正则表达式对象或字符串
* 若参数1是**字符串，则只进行一次匹配替换**
* 若替换**所有子串必须制定全局标记g**
* 若参数2位字符串可使用特殊字符序列（$&匹配整个模式的子字符串，$'匹配子字符串之前的子字符串，$`匹配子字符串之后的字符串，$n第n个捕获组的子字符串）

<pre>
在本例中，我们将把 "Doe, John" 转换为 "John Doe" 的形式：
name = "Doe, John";
name.replace(/(\w+)\s*, \s*(\w+)/, "$2 $1")
</pre>
###split
str.split(' ',int) int为返回数组的最大长度，其余的删除
#二、操作方法
##1 拼接方法。返回新的字符串
###concat
返回新的字符串 与+功能相同
##2 截取方法
###下标截取
* slice ''.slice(start[起始下标],end[结尾下标]) 可为负
* substring ''.sunstring() 不允许为负

###长度截取（不推荐）
*substr

##3 空格处理
* trim  返回去掉开头和结尾空格后的字符串  String.trim('   visit school')
* trimleft 清楚前置空格
* trimRight 清楚后置空格

##4 比较方法
###localeCompare
* 负数 原字符串<参数字符串
* 0 
* 正数 原字符串>参数字符串
#三、编码方法
##1 字符串常规编码与解码(不推荐)
###escape 
###unescape
##2 URI字符串编码与解码
###encodeURI
###decodeURI
##3 URI组件编码与解码
###encodeURIComponent
###decodeURIComponent

该方法不会对 ASCII 字母和数字进行编码，也不会对这些 ASCII 标点符号进行编码： - _ . ! ~ * ' ( ) 。

该方法的目的是对 URI 进行完整的编码，因此对以下在 URI 中具有特殊含义的 ASCII 标点符号，encodeURI() 函数是不会进行转义的：;/?:@&=+$,#

如果 URI 组件中含有分隔符，比如 ? 和 #，则应当使用 encodeURIComponent() 方法分别对各组件进行编码。
#四、转换方法
##1 大小写
###大写 toUpperCase()
###小写 toLowerCase()