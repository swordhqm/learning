<div class="Left fl">

<div class="artical-Left">

<div class="crumbs"><span>[无名](http://blog.51cto.com/xdzw608)</span> _>_ <span>[Python](http://blog.51cto.com/xdzw608/category1.html)</span> _>_ <span>正文</span></div>

# python simplejson模块浅谈

<div class="artical-title-list">[原创](javascript:;) [xdzw](http://blog.51cto.com/xdzw608) <span class="fl"></span>[2015-02-06 15:29:34](javascript:;) <span class="fl"></span>[评论(0)](#comment) [3820人阅读](javascript:;)</div>

<div class="artical-content-bak main-content editor-side-new">

<div class="con editor-preview-side">

一、背景知识

*   **JSON**:

    引用百科描述如下，具体请自行搜索相关介绍：

    JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式。它基于JavaScript（Standard ECMA-262 3rd Edition - December 1999）的一个子集。 JSON采用完全独立于语言的文本格式，但是也使用了类似于C语言家族的习惯（包括C, C++, C#, Java, JavaScript, Perl, Python等）。这些特性使JSON成为理想的数据交换语言。易于人阅读和编写，同时也易于机器解析和生成(网络传输速度快)。

    表示方法：  

*   *   数据在名称/值对中

    *   数据由逗号分隔

    *   花括号保存对象

    *   方括号保存数组

    示例：

<div class="line number1 index0 alt2">

<div>

<div id="highlighter_834978" class="syntaxhighlighter  python">

<table border="0" cellpadding="0" cellspacing="0">

<tbody>

<tr>

<td class="gutter">

<div class="line number1 index0 alt2">1</div>

<div class="line number2 index1 alt1">2</div>

<div class="line number3 index2 alt2">3</div>

<div class="line number4 index3 alt1">4</div>

<div class="line number5 index4 alt2">5</div>

<div class="line number6 index5 alt1">6</div>

<div class="line number7 index6 alt2">7</div>

<div class="line number8 index7 alt1">8</div>

<div class="line number9 index8 alt2">9</div>

<div class="line number10 index9 alt1">10</div>

</td>

<td class="code">

<div class="container">

<div class="line number1 index0 alt2">`{``"programmers"``:[`</div>

<div class="line number2 index1 alt1">`{``"firstName"``:``"Brett"``,``"lastName"``:``"McLaughlin"``,``"email"``:``"aaaa"``},`</div>

<div class="line number3 index2 alt2">`{``"firstName"``:``"Jason"``,``"lastName"``:``"Hunter"``,``"email"``:``"bbbb"``},`</div>

<div class="line number4 index3 alt1">`{``"firstName"``:``"Elliotte"``,``"lastName"``:``"Harold"``,``"email"``:``"cccc"``}`</div>

<div class="line number5 index4 alt2">`],`</div>

<div class="line number6 index5 alt1">`"authors"``:[`</div>

<div class="line number7 index6 alt2">`{``"firstName"``:``"Isaac"``,``"lastName"``:``"Asimov"``,``"genre"``:``"sciencefiction"``},`</div>

<div class="line number8 index7 alt1">`{``"firstName"``:``"Tad"``,``"lastName"``:``"Williams"``,``"genre"``:``"fantasy"``},`</div>

<div class="line number9 index8 alt2">`{``"firstName"``:``"Frank"``,``"lastName"``:``"Peretti"``,``"genre"``:``"christianfiction"``}`</div>

<div class="line number10 index9 alt1">`]}`</div>

</div>

</td>

</tr>

</tbody>

</table>

</div>

</div>

</div>

*   **HOWTO-UNICODE:**  

    unicode标准描述了字符如何对应编码点(code point),使用16进制表示 00 00.

    PYTHON中，basestring派生了unicode类型和str类型

    unicode字符串是一个编码点序列，该序列在内存中会被表示成一组字节(0-255)，str是指8字节流。

    unicode字符串可以通过encode函数转换为str；str可以通过decode转换为unicode。编解码类型一般是utf-8  

    示例：

<div>

<div id="highlighter_398319" class="syntaxhighlighter  python">

<table border="0" cellpadding="0" cellspacing="0">

<tbody>

<tr>

<td class="gutter">

<div class="line number1 index0 alt2">1</div>

<div class="line number2 index1 alt1">2</div>

<div class="line number3 index2 alt2">3</div>

<div class="line number4 index3 alt1">4</div>

</td>

<td class="code">

<div class="container">

<div class="line number1 index0 alt2">`>>> u``"中国"``.encode(``'utf-8'``)`</div>

<div class="line number2 index1 alt1">`'\xe4\xb8\xad\xe5\x9b\xbd'`    `#将unicode字符串编码为str`</div>

<div class="line number3 index2 alt2">`>>> ``'\xe4\xb8\xad\xe5\x9b\xbd'``.decode(``'utf-8'``)`</div>

<div class="line number4 index3 alt1">`u``'\u4e2d\u56fd'`               `#将str解码为unicode字符串`</div>

</div>

</td>

</tr>

</tbody>

</table>

</div>

</div>

    **从文件中读和写入文件的操作都应该是操作的8位字节流，如果将unicode字符串写入文件，需要进行编码操作；如果从文件中读unicode字符串，首先读取出来的是8位字节流需要进行解码操作。**

**    一般功能代码中都直接操作unicode字符串，而只在写数据或读数据时添加对应的编解码操作。  
**

*   序列化和反序列化

    当两个进程在进行远程通信时，彼此可以发送各种类型的数据。无论是何种类型的数据，都会以二

进制序列的形式在网络上传送。发送方需要把这个对象转换为字节序列，才能在网络上传送；接收方则需要把字节序列再恢复为对象。  

    **把对象转换为字节序列的过程称为对象的序列化，比如把一个字典对象以某种格式(JSON)写到文件中；把字节序列恢复为对象的过程称为对象的反序列化，比如读取某种格式化(JSON)的文件，构造一个字典对象。**

    根据HOWTO-UNICODE的知识，把网络可以看做是一个文件，发送方写数据到网络时需要进行编码，接收方读取数据时需要进行解码。也就是说**序列化的同时会进行编码，反序列化的同时会进行解码。**

二、simplejson

    simplejson是json标准模块的扩展（基础功能相同），是pypi提供的拓展模块，需要另行安装。不过可以使用python自带的json库，基本是相同的使用方法(提供的接口功能基本一致)。在python的library文档中将JSON归为网络数据控制类，很好的说明了他们的用途，主要用于网络数据控制，编解码等。但是也具有其他的用途，比如可以用来作为配置文件的读写模块，简单的文件操作等。

    它提供的接口很少，容易掌握，而且大多数情况下会使用默认的参数。官方文档中阐明，默认的接口参数和不进行子类化会有更好的性能体现。下面我们对提供的接口进行讨论，并且仅展示必须参数，其他关键字参数将以**kwargs表示；

*   <tt class="descclassname">simplejson.</tt><tt class="descname">dump</tt>(obj, _fp_, **kwargs):将python对象写到文件中（以JSON格式）  

*   <tt class="descclassname">simplejson.</tt><tt class="descname">dumps</tt>(obj, **kwargs)：将python对象表示成字符串(JSON的格式)  

*   <tt class="descclassname">simplejson.</tt><tt class="descname">load</tt>(fp, **kwargs)：从文件中(包含JSON结构)读取为python对象  

*   <tt class="descclassname">simplejson.</tt><tt class="descname">loads</tt>(s, **kwargs)：从字符串中(包含JSON结构)读取为python对象  

*   _class_ <tt class="descclassname">simplejson.</tt><tt class="descname">JSONDecoder：load/loads的时候调用，将JSON格式序列解码为python对象  
    </tt>

*   _class_ <tt class="descclassname">simplejson.</tt><tt class="descname">JSONEncoder</tt>：dump/dumps的时候调用，将python对象编码为JSON格式序列

    联系到上面的基础知识，我们可以知道，**dump的过程其实就是向文件句柄中写数据，即对象序列化的过程，需要进行编码，只是编码的格式不只是unicode和str的转换，而是更重要的python对象类型和JSON对象类型之间的转换。同理，load的过程其实就是从文件句柄中读数据，即反序列化生成对象的过程，需要进行解码，只是解码的格式不只是str和unicode的转换，而是更重要的JSON对象类型和python对象类型之间的转换。**

    下面是JSON对象类型和Python对象类型之间的对应关系：

<table class="docutils" id="json-to-py-table" align="center">

<thead valign="bottom">

<tr class="row-odd">

<th class="head">JSON</th>

<th class="head">Python 2</th>

<th class="head">Python 3</th>

</tr>

</thead>

<tbody valign="top">

<tr class="row-even">

<td>object</td>

<td>dict</td>

<td>dict</td>

</tr>

<tr class="row-odd">

<td>array</td>

<td>list</td>

<td>list</td>

</tr>

<tr class="row-even">

<td>string</td>

<td>unicode</td>

<td>str</td>

</tr>

<tr class="row-odd">

<td>number (int)</td>

<td>int, long</td>

<td>int</td>

</tr>

<tr class="row-even">

<td>number (real)</td>

<td>float</td>

<td>float</td>

</tr>

<tr class="row-odd">

<td>true</td>

<td>True</td>

<td>True</td>

</tr>

<tr class="row-even">

<td>false</td>

<td>False</td>

<td>False</td>

</tr>

<tr class="row-odd">

<td>null</td>

<td>None</td>

<td>None</td>

</tr>

</tbody>

</table>

    下面以一个例子来结束本文，例子中附带注释：

<div>

<div id="highlighter_30719" class="syntaxhighlighter  python">

<table border="0" cellpadding="0" cellspacing="0">

<tbody>

<tr>

<td class="gutter">

<div class="line number1 index0 alt2">1</div>

<div class="line number2 index1 alt1">2</div>

<div class="line number3 index2 alt2">3</div>

<div class="line number4 index3 alt1">4</div>

<div class="line number5 index4 alt2">5</div>

<div class="line number6 index5 alt1">6</div>

<div class="line number7 index6 alt2">7</div>

<div class="line number8 index7 alt1">8</div>

<div class="line number9 index8 alt2">9</div>

<div class="line number10 index9 alt1">10</div>

<div class="line number11 index10 alt2">11</div>

<div class="line number12 index11 alt1">12</div>

<div class="line number13 index12 alt2">13</div>

<div class="line number14 index13 alt1">14</div>

<div class="line number15 index14 alt2">15</div>

<div class="line number16 index15 alt1">16</div>

<div class="line number17 index16 alt2">17</div>

<div class="line number18 index17 alt1">18</div>

<div class="line number19 index18 alt2">19</div>

<div class="line number20 index19 alt1">20</div>

<div class="line number21 index20 alt2">21</div>

<div class="line number22 index21 alt1">22</div>

<div class="line number23 index22 alt2">23</div>

<div class="line number24 index23 alt1">24</div>

<div class="line number25 index24 alt2">25</div>

<div class="line number26 index25 alt1">26</div>

<div class="line number27 index26 alt2">27</div>

<div class="line number28 index27 alt1">28</div>

<div class="line number29 index28 alt2">29</div>

<div class="line number30 index29 alt1">30</div>

<div class="line number31 index30 alt2">31</div>

<div class="line number32 index31 alt1">32</div>

<div class="line number33 index32 alt2">33</div>

<div class="line number34 index33 alt1">34</div>

<div class="line number35 index34 alt2">35</div>

<div class="line number36 index35 alt1">36</div>

<div class="line number37 index36 alt2">37</div>

<div class="line number38 index37 alt1">38</div>

</td>

<td class="code">

<div class="container">

<div class="line number1 index0 alt2">`#coding:utf-8`</div>

<div class="line number2 index1 alt1">`import` `simplejson as json`</div>

<div class="line number4 index3 alt1">`#simplejson.dump(**kwargs)`</div>

<div class="line number5 index4 alt2">`fp ``=` `open``(``'./text.json'``, ``'w+'``)`</div>

<div class="line number6 index5 alt1">`json.dump([``1``,``2``], fp)         ``##将python数组进行序列化，保存到文件中`</div>

<div class="line number7 index6 alt2">`fp.seek(``0``)`</div>

<div class="line number8 index7 alt1">`print` `"----dump----\n"``, u``'使用dump将python数组对象保存在一个包含JSON格式的文件中，文件内容为：\n'``, fp.read()`</div>

<div class="line number9 index8 alt2">`print` </div>

<div class="line number10 index9 alt1">`fp.close()         `</div>

<div class="line number12 index11 alt1">`#simplejson.dumps(**kwargs)`</div>

<div class="line number13 index12 alt2">`r_dumps ``=` `json.dumps({``"中国obj"``:[``1``,``2``], ``"obj2"``:[``3``,``4``]})  ``#将python字典进行序列化，保存到字符串中`</div>

<div class="line number14 index13 alt1">`print` `"----dumps----\n"``, u``'使用dumps将python字典对象转换为一个包含JSON格式的字符串，字符串结果为：\n'``, r_dumps`</div>

<div class="line number15 index14 alt2">`print`</div>

<div class="line number17 index16 alt2">`#simplejson.load(**kwargs)`</div>

<div class="line number18 index17 alt1">`#如果json文档格式有错误，将会抛出JSONDecoderError异常`</div>

<div class="line number19 index18 alt2">`fp ``=` `open``(``'./text.json'``, ``'r'``)`</div>

<div class="line number20 index19 alt1">`r_load ``=` `json.load(fp)           ``#将文件中的内容转换为python对象`</div>

<div class="line number21 index20 alt2">`print` `"----load----\n"``, u``"使用load读取一个包含JSON数组格式的文件后，得到一个python对象，类型是："``, ``type``(r_load)`</div>

<div class="line number22 index21 alt1">`print` </div>

<div class="line number23 index22 alt2">`#simplejson.loads(**kwargs)`</div>

<div class="line number24 index23 alt1">`#如果json文档格式有错误，将会抛出JSONDecoderError异常`</div>

<div class="line number26 index25 alt1">`#将字符串中的内容转换为一个python对象`</div>

<div class="line number27 index26 alt2">`r_loads ``=` `json.loads(``'''{"programmers":[`</div>

<div class="line number28 index27 alt1">`{"firstName":"Brett","lastName":"McLaughlin","email":"aaaa"},`</div>

<div class="line number29 index28 alt2">`{"firstName":"Jason","lastName":"Hunter","email":"bbbb"},`</div>

<div class="line number30 index29 alt1">`{"firstName":"Elliotte","lastName":"Harold","email":"cccc"}`</div>

<div class="line number31 index30 alt2">`],`</div>

<div class="line number32 index31 alt1">`"authors":[`</div>

<div class="line number33 index32 alt2">`{"firstName":"Isaac","lastName":"Asimov","genre":"sciencefiction"},`</div>

<div class="line number34 index33 alt1">`{"firstName":"Tad","lastName":"Williams","genre":"fantasy"},`</div>

<div class="line number35 index34 alt2">`{"firstName":"Frank","lastName":"Peretti","genre":"christianfiction"}`</div>

<div class="line number36 index35 alt1">`]}'''``)`</div>

<div class="line number37 index36 alt2">`print` `"----loads----\n"``, u``"使用loads读取一个包含JSON字典格式的字符串后，得到一个python对象，类型是："``, ``type``(r_loads)`</div>

<div class="line number38 index37 alt1">`print`</div>

</div>

</td>

</tr>

</tbody>

</table>

</div>

</div>

运行之后的结果显示：

<div>

<div id="highlighter_70270" class="syntaxhighlighter  python">

<table border="0" cellpadding="0" cellspacing="0">

<tbody>

<tr>

<td class="gutter">

<div class="line number1 index0 alt2">1</div>

<div class="line number2 index1 alt1">2</div>

<div class="line number3 index2 alt2">3</div>

<div class="line number4 index3 alt1">4</div>

<div class="line number5 index4 alt2">5</div>

<div class="line number6 index5 alt1">6</div>

<div class="line number7 index6 alt2">7</div>

<div class="line number8 index7 alt1">8</div>

<div class="line number9 index8 alt2">9</div>

<div class="line number10 index9 alt1">10</div>

</td>

<td class="code">

<div class="container">

<div class="line number1 index0 alt2">`-``-``-``-``dump``-``-``-``-`</div>

<div class="line number2 index1 alt1">`使用dump将python数组对象保存在一个包含JSON格式的文件中，文件内容为：`</div>

<div class="line number3 index2 alt2">`[``1``, ``2``]`</div>

<div class="line number4 index3 alt1">`-``-``-``-``dumps``-``-``-``-`</div>

<div class="line number5 index4 alt2">`使用dumps将python字典对象转换为一个包含JSON格式的字符串，字符串结果为：`</div>

<div class="line number6 index5 alt1">`{``"obj2"``: [``3``, ``4``], ``"\u4e2d\u56fdobj"``: [``1``, ``2``]}`</div>

<div class="line number7 index6 alt2">`-``-``-``-``load``-``-``-``-`</div>

<div class="line number8 index7 alt1">`使用load读取一个包含JSON数组格式的文件后，得到一个python对象，类型是： <``type` `'list'``>`</div>

<div class="line number9 index8 alt2">`-``-``-``-``loads``-``-``-``-`</div>

<div class="line number10 index9 alt1">`使用loads读取一个包含JSON字典格式的字符串后，得到一个python对象，类型是： <``type` `'dict'``>`</div>

</div>

</td>

</tr>

</tbody>

</table>

</div>

</div>

</div>

</div>

<div class="artical-copyright">版权声明：原创作品，如需转载，请注明出处。否则将追究法律责任</div>

<div class="for-tag"><span>[json](http://blog.51cto.com/search/result?q=json)</span> <span>[序列化](http://blog.51cto.com/search/result?q=%E5%BA%8F%E5%88%97%E5%8C%96)</span> <span>[python](http://blog.51cto.com/search/result?q=python)</span> <span>[simplejson](http://blog.51cto.com/search/result?q=simplejson)</span></div>

<div class="more-list">

<span type="1" blog_id="1612389" userid="4812210">1</span>

<div class="share-box fr">

分享

<div class="bdsharebuttonbox bdshare-button-style0-16" data-bd-bind="1523156541640"><span></span>[QQ分享](javascript:; "分享到QQ好友") [微博分享](javascript:; "分享到新浪微博") [微信扫一扫](javascript:; "分享到微信") ![](/qr/qr-url?url=http%3A%2F%2Fblog.51cto.com%2Fxdzw608%2F1612389)</div>

</div>

收藏

</div>

<div class="artical-list">[上一篇：python unittest框架](http://blog.51cto.com/xdzw608/1612063 "python unittest框架") [下一篇：Undefined variable from import的解决方案](http://blog.51cto.com/xdzw608/1620403 "Undefined variable from import的解决方案")</div>

</div>

## 猜你喜欢

<div class="artical-Left artical-border">

*   [数据分析之A股市场技术分析是否可行](http://blog.51cto.com/youerning/2086390)
*   [小灶时间-如果你还不会用Python虚拟环境](http://blog.51cto.com/de8ug/2087144)
*   [python数据类型 循环语句 循环关键字](http://blog.51cto.com/12183531/2088209)
*   [python web开发-flask中访问请求数据request](http://blog.51cto.com/12482328/2088937)
*   [用python写一个微信聊天机器人](http://blog.51cto.com/cwtea/2089022)
*   [简单的函数实参、形参、默认值的定义](http://blog.51cto.com/13595859/2089244)
*   [openpyxl模块（excel操作）](http://blog.51cto.com/daimalaobing/2089686)
*   [python web开发-flask中读取txt文件内容](http://blog.51cto.com/12482328/2089796)

</div>

## 发表评论

<div class="artical-Left artical-border">

<div class="comment-creat">

<div class="is-vip-bg fl">[![](http://ucenter.51cto.com/images/noavatar_middle.gif)](javascript:;) </div>

<div class="first-publish fr publish_user_id"><textarea class="textareadiv textareadiv-publish" name="" id="" placeholder="用心的评论会被更多人看到和认可" maxlength="500"></textarea>

<div class="comment-push">

Ctrl+Enter 发布

发布

取消

</div>

<input type="hidden" class="user_id" value="4812210"> <input type="hidden" class="reply_id" value="1612389"> <input type="hidden" class="first_pid" value=""></div>

</div>

</div>

</div>