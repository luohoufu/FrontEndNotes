[TOC]

[参考](https://segmentfault.com/a/1190000004533485?_ea=657625)

```css
background-image:url(data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAAEAAAAkCAYAAABIdFAMAAAAGXR
FWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAHhJRE
FUeNo8zjsOxCAMBFB/KEAUFFR0Cbng3nQPw68ArZdAlOZppPFIB
hH5EAB8b+Tlt9MYQ6i1BuqFaq1CKSVcxZ2Acs6406KUgpt5/　
LCKuVgz5BDCSb13ZO99ZOdcZGvt4mJjzMVKqcha68iIePB86G
AiOv8CDADlIUQBs7MD3wAAAABJRU5ErkJggg%3D%3D)
```

**Data URl scheme**是在RFC2397中定义的，目的是将一些小的数据，直接嵌入到网页中，从而不用再从外部文件载入。比如上面那串字符，其实是一张小图片，将这些字符复制黏贴到火狐的地址栏中并转到，就能看到它了，一张1X36的白灰png图片



* data：取得数据的协定名称
* image／png；数据类型名称
* Base64， 编码方式
* 后面的字符串，编码之后的字符串

Data URI scheme 支持的数据类型

```
data:,文本数据
data:text/plain,文本数据
data:text/html,HTML代码
data:text/html;base64,base64编码的HTML代码
data:text/css,CSS代码
data:text/css;base64,base64编码的CSS代码
data:text/javascript,Javascript代码
data:text/javascript;base64,base64编码的Javascript代码
data:image/gif;base64,base64编码的gif图片数据
data:image/png;base64,base64编码的png图片数据
data:image/jpeg;base64,base64编码的jpeg图片数据
data:image/x-icon;base64,base64编码的icon图片数据
```

## base64编码

将任意一组字节转换成较长的常见文本字符序列，从而合法地作为首部字段值，base64讲用户输入或者二进制数据，打包成一种安全格式，作为http首部字段发出，无需担心其中包含破坏http分析程序的冒号、换行符或者二进制值。

base64是作为MIME多媒体邮件标准的一部分开发的，这样就可以传输富文本和任意二进制数据里。

将8为序列拆分为6位的片段，每个6位的片段分配一个字符，共64个字符，因此base64比原始值大33%

举个🌰

```
(1) 字符串"Ow!"被拆分成3个8位的字节(0x4F、0x77、0x21)。
(2) 这3个字节构成了一个24为的二进制01001111 01110111 00100001。
(3) 这些为被划分为一些6位的序列010011、110111、011100、1000001.
(4) 每个6位值都表示了从0～63之间的数字，对应base64字母表中的64个字符之一。得到的base64编码字符串是4个字符的字符串“T3ch”。然后就可以通过线路将这个字符串作为“安全的”8位字符传送出去，因为只用了一些移植性最好的字符(字母、数字等)。
```

base64转换代码

```javascript
// 现在将字符串"Ow!"转换为base64编码值
var str = 'Ow!';
// 或去字符串的二进制码
var binary = [];
for (var i = 0; i < str.length; i++) {
    // 转换为二进制表示
    var binStr = str.charCodeAt(i).toString(2);
    // 将得到的二进制放入数组中得到
    // ['1001111','1110111','100001']
    // 因为一个正常的二进制字节都是由8bit组成的,不够8bit的话不表示.上面得到的都不够8bit所以前面我们手动给补0,就得到了
    // ['01001111','01110111','00100001']
    binary.push(binStr);
}
        
// 1 把字符串按照6位分开,进行分割,得到['010011','110111','011100','1000001']
// 2 将每一个转换为十进制分别对于[19,55,28,33];
// 3 将每一位数字分别对于上面提供的base64对应表,得到对应的编码,分别对于 T 3 c h
// 4 最后就会得到base64编码T3ch
console.log('字符"Ow!"最后得到的base64编码为"T3ch"');
```

```
number.toString(2)可以按照index进制转化，转化为2进制字符串
```

## base64填充

base64编码收到一个8位字节序列，将这个二进制序列流划分成6位的块。二进制序列有时不能正好平均地分为6位的块，在这种情况下，就在序列末尾填充零位，使二进制序列的长度成为24的倍数(6和8的最小公倍数)。
对已填充的二进制进行编码时，任何完全填充(不包括原始数组中的位)的6位组都有特殊的第65个符号"="表示。如果6位组是部分填充的，就将填充位设置为0.

## 最新的浏览器提供了base64转换的方式

```
btoa('a:a')
// => "YTph"
atob('YTph')
// => "a:a"
```













