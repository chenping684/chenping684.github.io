---
layout:    post
title:     "php 正则表达式"
subtitle:  "正则表达式知识整理"
date:       2015-12-15 16:26
category: php
---
> 本文整理简述`正则表达式30分钟入门教程` 并根据php官方文档进行补充以巩固所学
> [正则表达式30分钟入门教程](http://www.oschina.net/question/12_9507)
> [php官方文档之PCRE正则语法](http://php.net/manual/zh/regexp.introduction.php) 

基础知识
-------
<div class="row">
    <div class="col-md-6">
        <table cellspacing="0" border="1">
            <caption>表1.元字符</caption>
            <thead>
                <tr><th scope="col">代码</th> <th scope="col">说明</th></tr>
            </thead>
            <tbody>
                <tr><td><span class="code">.</span></td><td><span class="desc">匹配除换行符以外的任意字符</span></td></tr>
                <tr><td><span class="code">\w</span></td><td><span class="desc">匹配字母或数字或下划线或汉字</span></td></tr>
                <tr><td><span class="code">\s</span></td><td><span class="desc">匹配任意的空白符</span></td></tr>
                <tr><td><span class="code">\d</span></td><td><span class="desc">匹配数字</span></td></tr>
                <tr><td><span class="code">\b</span></td><td><span class="desc">匹配单词的开始或结束</span></td></tr>
                <tr><td><span class="code">^</span></td><td><span class="desc">匹配字符串的开始</span></td></tr>
                <tr><td><span class="code">$</span></td><td><span class="desc">匹配字符串的结束</span></td></tr>
                <tr><td><span class="code">\</span></td><td><span class="desc">一般用于转义字符</span></td></tr>
                <tr><td><span class="code">[</span></td><td><span class="desc">开始字符类定义</span></td></tr>
                <tr><td><span class="code">]</span></td><td><span class="desc">结束字符类定义</span></td></tr>
                <tr><td><span class="code">|</span></td><td><span class="desc">开始一个可选分支</span></td></tr>
                <tr><td><span class="code">(</span></td><td><span class="desc">子组的开始标记</span></td></tr>
                <tr><td><span class="code">)</span></td><td><span class="desc">子组的结束标记</span></td></tr>
                <tr><td><span class="code">?</span></td><td><span class="desc">量词，表示 0 次或 1 次匹配。位于量词后面用于改变量词的贪婪特性</span></td></tr>
                <tr><td><span class="code">*</span></td><td><span class="desc">量词，0 次或多次匹配</span></td></tr>
                <tr><td><span class="code">+</span></td><td><span class="desc">量词，1 次或多次匹配</span></td></tr>
                <tr><td><span class="code">{</span></td><td><span class="desc">自定义量词开始标记</span></td></tr>
                <tr><td><span class="code">}</span></td><td><span class="desc">自定义量词结束标记</span></td></tr>
                <tr><td><span class="code">^</span></td><td><span class="desc">(方括号内)仅在作为第一个字符时，表明字符类取反</span></td></tr>
                <tr><td><span class="code">-</span></td><td><span class="desc">(方括号内)标记字符范围</span></td></tr>
            </tbody>
        </table>
    </div>

    <div class="col-md-6">
        <table cellspacing="0" border="1">
           <caption>表2.常用的限定符</caption>
           <thead>
               <tr><th scope="col">代码/语法</th> <th scope="col">说明</th></tr>
           </thead>
           <tbody>
               <tr><td><span class="code">*</span></td><td><span class="desc">重复零次或更多次, 等价于 {0,}</span></td></tr>
               <tr><td><span class="code">+</span></td><td><span class="desc">重复一次或更多次, 等价于 {1,}</span></td></tr>
               <tr><td><span class="code">?</span></td><td><span class="desc">重复零次或一次, 等价于 {0,1}</span></td></tr>
               <tr><td><span class="code">{n}</span></td><td><span class="desc">重复n次</span></td></tr>
               <tr><td><span class="code">{n,}</span></td><td><span class="desc">重复n次或更多次</span></td></tr>
               <tr><td><span class="code">{n,m}</span></td><td><span class="desc">重复n到m次</span></td></tr>
           </tbody>
        </table>
    </div>
</div>

#### 结合表1、表2的例子
`\s`        匹配任意的空白符, 包括空格, 制表符(Tab), 换行符, 中文全角空格等<br/>
`\ba\w*\b`  匹配以字母a开头的单词——先是某个单词开始处(\b)，然后是字母a,然后是任意数量的字母或数字(\w*)， 最后是单词结束处(\b)。<br/>
`\d+`       匹配1个或更多连续的数字。 `*匹配重复任意次(可能是0次)`，而`+则匹配重复1次或更多次`。<br/>    
`\b\w{6}\b` 匹配刚好6个字符的单词。<br/>
`\d{5,12}`  字符串里包含5到12连续位数字(如果使用^和$的话,字符串就是5到12位数字,即 `^\d{5,12}$`)<br/>
`Windows\d+` 匹配Windows后面跟1个或更多数字<br/>
`^\w+`      匹配一行的第一个单词(或整个字 符串的第一个单词，具体匹配哪个意思得看选项设置)<br/>

#### 分隔符
 当使用 PCRE 函数的时候，模式需要由分隔符闭合包裹。分隔符可以使任意非字母数字、非反斜线、非空白字符。

经常使用的分隔符是正斜线(/)、hash符号(#) 以及取反符号(~)。下面的例子都是使用合法分隔符的模式。

/foo bar/
#^[^0-9]$#
+php+
%[a-zA-Z0-9_-]%

如果分隔符需要在模式内进行匹配，它必须使用反斜线进行转义。如果分隔符经常在 模式内出现， 一个更好的选择就是是用其他分隔符来提高可读性。

/http:\/\//
#http://#


#### 字符转义
如果想查找元字符本身的话，比如你查找.,或者*,就出现了问题：你没办法指定它们，因为它们会被解释成别的意思。这时你就得使用\来取消这些字符的特殊意义。
`“(”和“)”也是元字符, `
例(仅列出部分， 参照表1.元字符)：
`.` => `\.`
`?` => `\?`
`*` => `\*`
`+` => `\+`
`\` => `\\`
`(` => `\(`
`)` => `\)` 
`[` => `\[`
`]` => `\]`

#### 方括号[] 匹配自定义字符
表1的元字符是内置的， 我们也可以根据需求使用[]自定义匹配字符
`[aeiou]`   匹配任何一个英文元音字母
`[.?!]]`    匹配标点符号(.或?或!)
`[0-9]`     含意与\d就是完全一致的：一位数字
`[a-z0-9A-Z_]`  也完全等同于\w（如果只考虑英文字符的话）

#### 整合以上知识举个例子来巩固所学

`\(?0\d{2}[) -]?\d{8}` 这个表达式可以匹配几种格式的电话号码，像(010)88886666，或022-22334455， 或02912345678等。
`分析` 首先是一个转义字符`\(`,它能出现0次或1次(`?`),然后是一个0，后面跟着2个数字(`\d{2}`)，然后是`)`或`-`或空格中 的一个，它出现1次或不出现(`?`)，最后是8个数字(`\d{8}`)。

不幸的是这个表达式也可以匹配`010)12345678`或`(022-87654321`这样的“不正确”的格式。要解决这个问题，我们需要用到分枝条件。

#### 分支条件 `|`
正则表达式里的分枝条件指的是有几种规则，如 果满足其中任意一种规则都应该当成匹配，具体方法是用|把不同的规则分隔开。

`0\d{2}-\d{8}|0\d{3}-\d{7}`这个表达式能匹配两种以连字号分隔的电话号码：一种是三位区号，8位本地号(如`010-12345678`)，一种是4位区号，7位本地号(`0376-2233445`)。

`\(0\d{2}\)[- ]?\d{8}|0\d{2}[- ]?\d{8}`这个表达式匹配3位区号的电话号码，其中区号`可以用小括号括起来`，`也可以不用`，区号与本地号间可以用`连字号或空格间隔`，`也可以没有间隔`。

`\d{5}-\d{4}|\d{5}` 这个表达式用于匹配美国的邮政编码。美国邮编的规则是5位数字，或者用连字号间隔的9位数字。之所以要给出这个例子是因为它能说明一个问题：`使用分枝条件时，要注意各个条件的顺序`。 如果你把它改成`\d{5}|\d{5}-\d{4}`的话，那么就只会匹配5位的邮编(以及9位邮 编的前5位)。`原因是匹配分枝条件时，将会从左到右地测试每个条件，如果满足了某个分枝的话，就不会去再管其它的条件了`。


#### 反义

<div class="row">
    <div class="col-md-12">
        <table cellspacing="0" border="1">
            <caption>表3.常用的反义代码</caption>
            <thead>
                <tr><th scope="col">代码/语法</th> <th scope="col">说明</th></tr>
            </thead>
            <tbody>
            <tr><td><span>\W</span></td><td><span class="desc">匹配任意不是字母，数字，下划线，汉字的字符</span></td></tr>
            <tr><td><span class="code">\S</span></td><td><span class="desc">匹配任意不是空白符的字符</span></td></tr>
            <tr><td><span class="code">\D</span></td><td><span class="desc">匹配任意非数字的字符</span></td></tr>
            <tr><td><span class="code">\B</span></td><td><span class="desc">匹配不是单词开头或结束的位置</span></td></tr>
            <tr><td><span class="code">[^x]</span></td><td><span class="desc">匹配除了x以外的任意字符</span></td></tr>
            <tr><td><span class="code">[^aeiou]</span></td><td><span class="desc">匹配除了aeiou这几个字母以外的任意字符</span></td></tr>
            </tbody>
        </table>
    </div>
</div>
例子：
`\S+`匹配不包含空白符的字符串。
`<a[^>]+>`匹配用尖括号括起来的以a开头除了`>`的字符串。

### 分组 `()`
`表2.常用的限定符` 可以`重复多个单字符`，如：\d{2} 重复两个0-9数字。
如果想要`重复多个字符`就要用小括号来指定`子表达式`(也叫做`分组`)，然后你就可以指定这个子表达式的重复次数了，你也可以对子表达式进行其它一些操作(后面会有介绍)。
`(\d{1,3}\.){3}\d{1,3}`是一个简单的IP地址匹配表达式。要理解这个表达式，请按下列顺序分析它：`\d{1,3}`匹配1到3位的数字，`(\d{1,3}\.){3}`匹配三位数字加上一个英文句号(`这个整体也就是这个分组`)重复3次，最后再加上一个一到三位的数字(`\d{1,3}`)。
不幸的是，它也将匹配`256.300.888.999`这种不可能存在的IP地址。
如果能使用算术比较的话，或许能简单地解决这个问题，但是正则表达式中并不提供关于数学的任何功能，所以只能使用冗长的分组，选择，字符类来描述一个正确的IP地址：`((2[0-4]\d|25[0-5]|[01]?\d\d?)\.){3}(2[0-4]\d|25[0-5]|[01]?\d\d?)`。
理解这个表达式的关键是理解`2[0-4]\d|25[0-5]|[01]?\d\d?`，

{% highlight php startinline %}  
if(preg_match('/((2[0-4]\d|25[0-5]|[01]?\d\d?)\.){3}(2[0-4]\d|25[0-5]|[01]?\d\d?)/', 
'255.168.1.25', 
$matches)) 
{
    var_dump($matches);
}
//output:
//array (size=4)
//  0 => string '192.168.1.25'   正则表示式匹配的字符串
//  1 => string '1.'             捕获第一个括号里的字符((2[0-4]\d|25[0-5]|[01]?\d\d?)\.)
//  2 => string '1'              捕获第一个括号里的的子括号(也就是第二个括号里面的字符)里面的字符字符(2[0-4]\d|25[0-5]|[01]?\d\d?)
//  3 => string '25'             捕获第第三个括号里面的字符(2[0-4]\d|25[0-5]|[01]?\d\d?)
{% endhighlight %}




<div class="row">
    <div class="col-md-12">
        <table cellspacing="0" border="1">
            <caption>((2[0-4]\d|25[0-5]|[01]?\d\d?)\.){3} 匹配255.168.1. 的捕获过程</caption> 
            <tbody>
                <tr>
                    <th scope="col"></th> 
                    <th scope="col">正则表达式</th>
                    <th scope="col">匹配的字符串</th>
                    <th scope="col">分组1</th>
                    <th scope="col">分组2</th>
                </tr>
                <tr>
                    <td><span class="code">第一次</span></td>
                    <td><span class="desc">25[0-5]</span></td>
                    <td><span class="desc">255.</span></td>
                    <td><span class="desc">255.</span></td>
                    <td><span class="desc">255</span></td>
                </tr> 
                <tr>
                      <td><span class="code">第二次</span></td>
                      <td><span class="desc">01]?\d\d?</span></td>
                      <td><span class="desc">168.</span></td>
                      <td><span class="desc">168.</span></td>
                      <td><span class="desc">168</span></td>
                </tr> 
                <tr>
                    <td><span class="code">第三次</span></td>
                    <td><span class="desc">01]?\d\d?</span></td>
                    <td><span class="desc">1.</span></td>
                    <td><span class="desc">1.</span></td>
                    <td><span class="desc">1</span></td>
                </tr> 
            </tbody>
        </table>
    </div>
</div>


虽然使用{3}重复了3次， 但是最终的分组号是不变的， 
所以第二次覆盖第一次的结果， 第三次覆盖第二次的结果， 最终结果如上例所示
分组三的结果仅匹配一次， 最终结果是25 

#### 后向引用

<div class="row">
    <div class="col-md-12">
        <table cellspacing="0" border="1">
            <caption>表4.常用分组语法</caption> 
            <tbody>
                <tr><th scope="col">代码/语法</th> <th scope="col">说明</th></tr>
                <tr><td><span class="code">(exp)</span></td><td><span class="desc">匹配exp,并捕获文本到自动命名的组里</span></td></tr> 
                <tr><td><span class="code">(?&lt;name&gt;exp)</span></td><td><span class="desc">匹配exp,并捕获文本到名称为name的组里，也可以写成 (?'name'exp)</span></td></tr>
                <tr><td><span class="code">(?:exp)</span></td><td><span class="desc">匹配exp,不捕获匹配的文本，也不给此分组分配组号</span></td></tr>
            </tbody>
        </table>
    </div>
</div>


使用小括号指定一个子表达式后，匹配这个子表达式的文本(也就是此分组捕获的内容)可以在表达式或其它 程序中作进一步的处理。
默认情况下，每个分组会自动拥有一个组号，规则是：`从左向右，以分组的左括号为标志，第一个出现的分组的组号为1，第二个为2，以此类推`。
分组0对应整个正则表达式组号分配过程是要从左向右扫描两遍的：第一遍只给未命名组分配，第二遍只给命名组分配－－因此所有命名组的组号都大于未命名的组号,你可以使用(?:exp)这样的语法来剥夺一个分组对组号分配的参与权．

后向引用用于重复搜索前面某个分组匹配的文本。例如，`\1代表分组1匹配的文本`。

##### 程序自动分配组号
`\b(\w+)\b\s+\1\b`可以用来匹配重复的单词，像go go, 或者kitty kitty。
这个表达式首先是一个单词，也就是单词开始处和结束处之间的多于一个的字母或数字(\b(\w+)\b)，这个单词会被捕获到编号为1的分组中，然后是1个或几个空白符(`\s+`)，最后是分组1中捕获的内容（也就是前面匹配的那个单词）(`\1`)。
   
{% highlight php startinline %}   
if(preg_match('/\b(\w+)\b\s+\1\b/', 'go go', $matches)) {
    var_dump($matches);   
} else {
    echo 'nothing match';
}
//output:
//array (size=2)
//  0 => string 'go go' (length=5)  分组0对应整个正则表达式
//  1 => string 'go' (length=2)     分组1对应表达式(\w+) 括号里面的 \w+
{% endhighlight %}

##### 自己指定子表达式的组名

要指定一个子表达式的组名，请使用这样的语法：`(?<Word>\w+)`(或者把尖括号换成'也行：`(?'Word'\w+))`,这样就把\w+的 组名指定为Word了。
    
{% highlight php startinline %}   
if(preg_match('/ab(?<Word>\w+)/', 'abcd1234?!', $matches)) {
    var_dump($matches);   
} else {
    echo 'nothing match';
}
//output:
//array (size=3)
//  0 => string 'abcd1234' (length=8)      分组0对应整个正则表达式匹配的字符串
//  'Word' => string 'cd1234' (length=6)   分组Word对应表达式(?<Word>\w+) 括号里面的 \w+
//  1 => string 'cd1234' (length=6)        分组1对应表达式(?<Word>\w+) 括号里面的 \w+
{% endhighlight %}
        
   要反向引用这个分组捕获的 内容，你可以使用`\k<Word>`,所以上一个例子也可以写成这样：`\b(?<Word>\w+)\b\s+\k<Word>\b`。

 {% highlight php startinline %} 
if(preg_match('/\b(?<Word>\w+)\b\s+\k<Word>\b/', 'go go', $matches)) {
    var_dump($matches);
} 
//output:
//array (size=3)
//  0 => string 'go go' (length=5)
//  'Word' => string 'go' (length=2)
//  1 => string 'go' (length=2)
 {% endhighlight %}
 
##### (?:exp)表达式语法来剥夺一个分组对组号分配的参与权。

 {% highlight php startinline %} 
// 例1 原始的分组表达式(\d)：
if(preg_match('/(\d){3}/', '999', $matches)) {
    var_dump($matches);
} 
//output:
//array (size=2)
//  0 => string '999' (length=3)   分组0对应整个正则表达式匹配的字符串
//  1 => string '9' (length=1)     分组1对应表达式(\d)括号里面的 \w

// 例2 (?:exp)语法表达式(?:\d+)：   
 if(preg_match('/(?:\d){3}/', '999', $matches)) {
     var_dump($matches);
 }
//output:
//array (size=1)
//  0 => string '999' (length=3)    分组0对应整个正则表达式匹配的字符串
 {% endhighlight %}
 
由例1 和 例2的结果得出： 使用(?:exp)表达式语法后 `程序将不会分配组号给分组`


##### 零宽断言

<div class="row">
    <div class="col-md-12">
        <table cellspacing="0" border="1">
            <caption>表5. 零宽断言</caption> 
            <tbody>
                <tr><th scope="col">代码/语法</th> <th scope="col">说明</th></tr>
                <tr><td><span class="code">(?=exp)</span></td><td><span class="desc">匹配exp前面的位置</span></td></tr>
                <tr><td><span class="code">(?&lt;=exp)</span></td> <td><span class="desc">匹配exp后面的位置</span></td></tr>
                <tr><td><span class="code">(?!exp)</span></td><td><span class="desc">匹配后面跟的不是exp的位置</span></td></tr>
                <tr><td><span class="code">(?&lt;!exp)</span></td><td><span class="desc">匹配前面不是exp的位置</span></td></tr>
            </tbody>
        </table>
    </div>
</div>

##### (?=exp) 零宽度正预测先行断言

它断言自身出现的位置的`后面`能匹配表达式exp,比如`\b\w+(?=ing\b)`，匹配以`ing`结尾的单词的前面部分(除了ing以外的部分)，
如查找`I'm singing while you're dancing.`时，它会匹配`sing`和`danc`

{% highlight php startinline %} 
preg_match('/xx(?=([^\.]))/', 'xxsdfsd.', $b);
var_dump($b);
//output: array(2) { [0] => 'xx' [1] => 's'}
{% endhighlight %}

匹配非.前面符合xx的字符,即 `xx` 字符符合表达式  
    
#####  (?<=exp)也叫零宽度正回顾后发断言
    
它断言自身出现的位置的`前面`能匹配表达式exp,比如`(?<=\bre)\w+\b`会匹配以`re`开头的单词的后半部分(除了re以外的部分)，
例如在查找`reading a book`时，它匹配`ading`。
{% highlight php startinline %} 
$a =  "
        车损信息\n
        维修零件名称：左后门\n
        维修金额：200.0\n
        维修零件名称：左后门\n
        维修金额：240.0\n
        维修零件名称：左后叶子板\n
        维修金额：200.0\n
        维修零件名称：左后叶子板\n
        维修金额：240.0\n
        维修零件名称：左侧门槛\n
        维修金额：100.0\n
        维修零件名称：左侧门槛\n
        维修金额：100.0\n
        维修零件名称：后杠\n
        维修金额：240.0\n
        维修零件名称：左后尾灯抛光\n
        维修金额：50.0\n";

preg_match('/(?<=维修零件名称：).*\n/', $a, $b);
var_dump($b);
//output: array (size=1)  0 => string '左后门
//            ' (length=10)
{% endhighlight %}    
匹配`维修零件名称：`后面以\n结尾的字符串

### 贪婪与懒惰
<div class="row">
    <div class="col-md-12">
       <table cellspacing="0" border="1">
            <caption>表6.懒惰限定符</caption> <thead> 
       <tr>
            <th scope="col">代码/语法</th> <th scope="col">说明</th>
       </tr>
       </thead> 
       <tbody>
           <tr>
               <td><span class="code">*?</span></td>
               <td><span class="desc">重复任意次，但尽可能少重复</span></td>
           </tr>
           <tr>
               <td><span class="code">+?</span></td>
               <td><span class="desc">重复1次或更多次，但尽可能少重复</span></td>
           </tr>
           <tr>
               <td><span class="code">??</span></td>
               <td><span class="desc">重复0次或1次，但尽可能少重复</span></td>
           </tr>
           <tr>
               <td><span class="code">{n,m}?</span></td>
               <td><span class="desc">重复n到m次，但尽可能少重复</span></td>
           </tr>
           <tr>
               <td><span class="code">{n,}?</span></td>
               <td><span class="desc">重复n次以上，但尽可能少重复</span></td>
           </tr>
       </tbody>
       </table>
    </div>
</div>
当正则表达式中包含能接受重复的限定符时，通常的行为是（在使整个表达式能得到匹配的前提下）匹配尽可能多的 字符。以这个表达式为例：a.*b，它将会匹配最长的以 a开始，以b结束的字符串。如果用它来搜索aabab的话，它会匹配整个字符串aabab。这被称为`贪婪匹配`。


{% highlight php startinline %}  
if(preg_match('/a.*b/', 'aabab', $matches)) {
    var_dump($matches);
}
//output:
//array (size=1)
//  0 => string 'aabab' (length=5)   贪婪匹配,匹配整个字符串aabab
  
{% endhighlight %}

有时，我们更需要`懒惰匹配`，也就是匹配尽可能少的 字符。前面给出的限定符都可以被转化为懒惰匹配模式，只要在它后面加上一个问号?。这样.*?就意味着匹配任意数量的重复，但是在能使整个匹配成功的前提 下使用最少的重复。现在看看懒惰版的例子吧：
a.*?b匹配最短的，以a开始，以b结 束的字符串。如果把它应用于aabab的话，它会匹配aab（第一到第三个字符）和ab（第四到第五个字符）。

{% highlight php startinline %}  
if(preg_match('/a.*?b/', 'aabab', $matches)) {
    var_dump($matches);
}
//output:
//array (size=1)
//  0 => string 'aab' (length=5)   懒惰匹配,匹配整个字符串aab
{% endhighlight %}

为什么第一个匹配是aab（第一到第三个字符）而不是ab（第四到第五个字符）？简单地说，因为正则表达式有另 一条规则，比懒惰／贪婪规则的优先级更高：最先开始的匹配拥有最高的优先权——`The match that begins earliest wins`。

其他例子：
{% highlight bash startinline %}  
[root@kaifa8 unitTest]# psysh 
Psy Shell v0.6.1 (PHP 5.5.29 — cli) by Justin Hileman
>>> $status = preg_match('/(\d.){1,2}/', '1.2.', $matches);
=> 1
>>> var_dump($matches);
array(2) {
  [0] =>
  string(4) "1.2."
  [1] =>
  string(2) "2."
}
=> null
>>> $status = preg_match('/(\d.){1,2}?/', '1.2.', $matches);
=> 1
>>> var_dump($matches);
array(2) {
  [0] =>
  string(2) "1."
  [1] =>
  string(2) "1."
}
=> null
>>> 

{% endhighlight %}


### 内部选项设置
下面是PHP中常用的正则表达式选项：

<div class="row">
    <div class="col-md-12">
    <table cellspacing="0" border="1">
        <caption>表7.处理选项</caption> <thead> 
    <tr>
        <th scope="col">名称</th> <th scope="col">说明</th>
    </tr>
    </thead> 
    <tbody>
        <tr>
            <td>i(忽略大小写)</td>
            <td>字母会进行大小写不敏感匹配。。</td>
        </tr>
        <tr>
            <td>m(多行模式)</td>
            <td>默认情况下，PCRE 认为目标字符串是由单行字符组成的(实际上它可能会包含多行)， "行首"元字符(^) 仅匹配字符串的开始位置， 而"行末"元字符($) 仅匹配字符串末尾， 或者最后的换行符(除非设置了 D 修饰符)。当这个修饰符设置之后，“行首”和“行末”就会匹配目标字符串中任意换行符之前或之后，另外， 还分别匹配目标字符串的最开始和最末尾位置。如果目标字符串中没有 "\n" 字符，或者模式中没有出现 ^ 或 $，设置这个修饰符不产生任何影响。 </td>
        </tr>
        <tr>
            <td>s(单行模式)</td>
            <td>如果设置了这个修饰符，模式中的点号元字符匹配所有字符，包含换行符。如果没有这个修饰符，点号不匹配换行符。 一个取反字符类比如 [^a] 总是匹配换行符，而不依赖于这个修饰符的设置。 </td>
        </tr>
        <tr>
            <td>x(忽略空白)</td>
            <td>如果设置了这个修饰符，模式中的没有经过转义的或不在字符类中的空白数据字符总会被忽略， 并且位于一个未转义的字符类外部的#字符和下一个换行符之间的字符也被忽略。 注意：这仅用于数据字符。 空白字符 还是不能在模式的特殊字符序列中出现，比如序列 (?( 引入了一个条件子组(译注: 这种语法定义的 特殊字符序列中如果出现空白字符会导致编译错误。 比如(?(就会导致错误)。 </td>
        </tr>
        <tr>
            <td>e(显式捕获 Warning 本特性已自 PHP 5.5.0 起废弃。强烈建议不要使用本特性。 )</td>
            <td>如果设置了这个被弃用的修饰符， preg_replace() 在进行了对替换字符串的 后向引用替换之后, 将替换后的字符串作为php 代码评估执行(eval 函数方式)，并使用执行结果 作为实际参与替换的字符串。单引号、双引号、反斜线(\)和 NULL 字符在 后向引用替换时会被用反斜线转义. </td>
        </tr>         
         
       <tr>
           <td>A</td>
           <td>如果设置了这个修饰符，模式被强制为"锚定"模式，也就是说约束匹配使其仅从 目标字符串的开始位置搜索。这个效果同样可以使用适当的模式构造出来，</td>
       </tr>      
        <tr>
          <td>D</td>
          <td>如果这个修饰符被设置，模式中的元字符美元符号仅仅匹配目标字符串的末尾。如果这个修饰符 没有设置，当字符串以一个换行符结尾时， 美元符号还会匹配该换行符(但不会匹配之前的任何换行符)。 如果设置了修饰符m，这个修饰符被忽略. </td>
        </tr>       
       <tr>
          <td>S</td>
          <td>当一个模式需要多次使用的时候，为了得到匹配速度的提升，值得花费一些时间 对其进行一些额外的分析。如果设置了这个修饰符，这个额外的分析就会执行。当前， 这种对一个模式的分析仅仅适用于非锚定模式的匹配(即没有单独的固定开始字符)。 </td>
        </tr>       
                 
       <tr>
       <td>U</td>
       <td>这个修饰符逆转了量词的"贪婪"模式。 使量词默认为非贪婪的，通过量词后紧跟? 的方式可以使其成为贪婪的。 它同样可以使用 模式内修饰符设置 (?U)进行设置， 或者在量词后以问号标记其非贪婪(比如.*?)。  </td>
     </tr>    
                 
       <tr>
         <td>X</td>
         <td>模式中的任意反斜线后就 ingen 一个 没有特殊含义的字符都会导致一个错误，以此保留这些字符以保证向后兼容性。 当前没有其他特性由这个修饰符控制。 </td>
        </tr>  
        <tr>
        <td>J </td>
        <td>内部选项设置(?J)修改本地的PCRE_DUPNAMES选项。允许子组重名， (译注：只能通过内部选项设置，外部的 /J 设置会产生错误。)  </td>
        </tr>  
        <tr>
          <td>u </td>
          <td>模式字符串被认为是utf-8的.</td>
        </tr>  
    </tbody>
    </table>
    </div>
</div>

### 递归匹配
[PHP正则之递归匹配](http://www.laruence.com/2011/09/30/2179.html)
[php官方文档之递归模式](http://www.php.net/manual/en/regexp.reference.recursive.php)

### 其他

<div class="row">
    <div class="col-md-12">
       <table cellspacing="0" border="1">
        <caption>表8.尚未详细讨论的语法</caption> 
        <thead> 
            <tr><th scope="col">代码/语法</th> <th scope="col">说明</th></tr>
       </thead> 
           <tbody>
               <tr><td><span class="code">\a</span></td><td><span class="desc">报警字符(打印它的效果是电脑嘀一声)</span></td></tr>
               <tr><td><span class="code">\b</span></td><td><span class="desc">通常是单词分界位置，但如果在字符类里使用代表退格</span></td></tr>
               <tr><td><span class="code">\t</span></td><td><span class="desc">制表符，Tab</span></td></tr>
               <tr><td><span class="code">\r</span></td><td><span class="desc">回车</span></td></tr>
               <tr><td><span class="code">\v</span></td><td><span class="desc">竖向制表符</span></td></tr>
               <tr><td><span class="code">\f</span></td><td><span class="desc">换页符</span></td></tr>
               <tr><td><span class="code">\n</span></td><td><span class="desc">换行符</span></td></tr>
               <tr><td><span class="code">\e</span></td><td><span class="desc">Escape</span></td></tr>
               <tr><td><span class="code">\0nn</span></td><td><span class="desc">ASCII代码中八进制代码为nn的字符</span></td></tr>
               <tr><td><span class="code">\xnn</span></td><td><span class="desc">ASCII代码中十六进制代码为nn的字符</span></td></tr>
               <tr><td><span class="code">\unnnn</span></td><td><span class="desc">Unicode代码中十六进制代码为nnnn的字符</span></td></tr>
               <tr><td><span class="code">\cN</span></td> <td><span class="desc">ASCII控制字符。比如\cC代表Ctrl+C</span></td> </tr>
               <tr> <td><span class="code">\A</span></td><td><span class="desc">字符串开头(类似^，但不受处理多行选项的影响)</span></td></tr>
               <tr> <td><span class="code">\Z</span></td>
               <td><span class="desc">字符串结尾或行尾(不受处理多行选项的影响)</span></td></tr>
               <tr> <td><span class="code">\z</span></td><td><span class="desc">字符串结尾(类似$，但不受处理多行选项的影响)</span></td>
               </tr><tr><td><span class="code">\G</span></td> <td><span class="desc">当前搜索的开头</span></td></tr> 
               <tr><td><span class="code">\p{name}</span></td><td><span class="desc">Unicode中命名为name的字符类，例如\p{IsGreek}</span></td></tr>
               <tr> <td><span class="code">(?&gt;exp)</span></td><td><span class="desc">贪婪子表达式</span></td> </tr>
               <tr> <td><span class="code">(?&lt;x&gt;-&lt;y&gt;exp)</span></td><td><span class="desc">平衡组</span></td></tr>
               <tr><td><span class="code">(?im-nsx:exp)</span></td> <td><span class="desc">在子表达式exp中改变处理选项</span></td> </tr><td><span class="code">(?im-nsx)</span></td>
               <td><span class="desc">为表达式后面的部分改变处理选项</span></td></tr>
               <tr><td><span class="code">(?(exp)yes|no)</span></td><td><span class="desc">把exp当作零宽正向先行断言，如果在这个位置能匹配，使用yes作为 此组的表达式；否则使用no</span></td></tr>
               <tr><td><span class="code">(?(exp)yes)</span></td><td><span class="desc">同上，只是使用空表达式作为no</span></td></tr>
               <tr><td><span class="code">(?(name)yes|no)</span></td> <td><span class="desc">如果命名为name的组捕获到了内容，使用yes作为表达式；否则使用 no</span></td> </tr>
               <tr><td><span class="code">(?(name)yes)</span></td><td><span class="desc">同上，只是使用空表达式作为no</span></td> </tr>
           </tbody>
           </table>
    </div>
</div>