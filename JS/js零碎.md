##1.将arguments转化为数组
<pre>
var args = Array.prototye.slice.apply(arguments);
</pre>
##2.contains
<pre>
var span = document.getElementById("mySPAN");
var div = document.getElementById("myDIV").contains(span);

true
</pre>

##3.三目表达式右结合
a?b:c?d:e将按a?b:（c?d:e）执行

先执行+

```
'value is'+(val!='0')?'define':'undefine'
define
```

##4.parseInt('123int') 
123,可转换八进制十六进制

##5.splice 第二个参数删除的数量
##6.+new Array（017）  NaN   
017八进制，因此生成15为空数组，+[] 会进行类型转换因此，为NaN

## 7.注意在js中取得input值时是字符串，因此如果计算时注意转化为整数

## 8.fouc

FOUC(文档样式短暂失效)?如果使用import方法对CSS进行导入,会导致某些页面在Windows 下的Internet Explorer出现一些奇怪的现象:以无样式显示页面内容的瞬间闪烁,这种现象称之为文档样式短暂失效(Flash of Unstyled Content),简称为FOUC。原因大致为：1，使用import方法导入样式表。2，将样式表放在页面底部3，有几个样式表，放在html结构的不同位置。其实原理很清楚：当样式表晚于结构性html加载，当加载到此样式表时，页面将停止之前的渲染。此样式表被下载和解析后，将重新渲染页面，也就出现了短暂的花屏现象。解决方法：使用LINK标签将样式表放在文档HEAD中。