Java处理带BOM的文本

说起BOM，这个问题还比较麻烦，因为BOM不可见，但用程序做不同编码文本处理时候却常常需要考虑到BOM的问题。在此之前，先对BOM做个简单认识。

先看看带BOM的文件：

源文件：

![](/assets/201006121276336513515.png)

16进制打开：

![](/assets/201006121276336369609.png)

下面举个例子，针对UTF-8的文件BOM做个处理 ：

`String xmla = StringFileToolkit.file2String(new File("D:\projects\mailpost\src\a.xml"),"UTF-8");`

`byte[] b = xmla.getBytes("UTF-8");`

`String xml = new String(b,3,b.length-3,"UTF-8");`

`Document doc1 = DocumentHelper.parseText(xml);`

`Element e1 = (Element)doc1.selectSingleNode("/ResponseData/Body/RetDesc");`

`Element e2 = (Element)doc1.selectSingleNode("/ResponseData/Head/RespID");`

`Element e3 = (Element)doc1.selectSingleNode("/ResponseData/Body/RetCode");`

`Element e4 = (Element)doc1.selectSingleNode("/ResponseData/Body/RetDesc");`

思路是：先按照UTF-8编码读取文件后，跳过前三个字符，重新构建一个新的字符串，然后用Dom4j解析处理，这样就不会报错了。

其他编码的方式处理思路类似，其实可以写一个通用的自动识别的BOM的工具，去掉BOM信息，返回字符串。

不过这个处理过程已经有牛人解决过了：[http:\/\/koti.mbnet.fi\/akini\/java\/unicodereader\/](http://koti.mbnet.fi/akini/java/unicodereader/ "http://koti.mbnet.fi/akini/java/unicodereader/")

-------------------------------------------------一下引用了百科

# 什么是BOM

BOM（byte-order mark），即字节顺序标记，它是插入到以UTF-8、UTF16或UTF-32编码Unicode文件开头的特殊标记，用来识别Unicode文件的编码类型。对于UTF-8来说，BOM并不是必须的，因为BOM用来标记多字节编码文件的编码类型和字节顺序（big-endian或little-endian）。

在绝大多数编辑器中都看不到BOM字符，因为它们能理解Unicode，去掉了读取器看不到的题头信息。若要查看某个Unicode文件是否以BOM开头，可以使用十六进制编辑器。下表列出了不同编码所对应的BOM。

| **BOM** | **Encoding** |
| --- | --- |
| EF BB BF | UTF-8 |
| FE FF | UTF-16 \(big-endian\) |
| FF FE | UTF-16 \(little-endian\) |
| 00 00 FE FF | UTF-32 \(big-endian\) |
| FF FE 00 00 | UTF-32 \(little-endian\) |

# BOM的来历

为了识别 Unicode 文件，Microsoft 建议所有的 Unicode 文件应该以 ZERO WIDTH NOBREAK SPACE（U+FEFF）字符开头。这作为一个“特征符”或“字节顺序标记（byte-order mark，BOM）”来识别文件中使用的编码和字节顺序。

# 不同的系统对BOM的支持

因为一些系统或程序不支持BOM，因此带有BOM的Unicode文件有时会带来一些问题。

1. JDK1.5以及之前的Reader都不能处理带有BOM的UTF-8编码的文件，解析这种格式的xml文件时，会抛出异常：Content is not allowed in prolog.

2. Linux\/UNIX 并没有使用 BOM，因为它会破坏现有的 ASCII 文件的语法约定。

不同的编辑工具对BOM的处理也各不相同。使用Windows自带的记事本将文件保存为UTF-8编码的时候，记事本会自动在文件开头插入BOM（虽然BOM对UTF-8来说并不是必须的），但是editplus就不会这样做。

# BOM与XML

XML解析读取XML文档时，W3C定义了3条规则：

1. 如果文档中有BOM，就定义了文件编码；

2. 如果文档中没有BOM，就查看XML声明中的编码属性；

3. 如果上述两者都没有，就假定XML文档采用UTF-8编码。

# 参考资料：

1. UTF-8, UTF-16, UTF-32 & BOM：[http:\/\/www.unicode.org\/faq\/utf\_bom.html\#BOM](http://www.unicode.org/faq/utf_bom.html#BOM)

2. XML FAQ：Encoding：[http:\/\/www.opentag.com\/xfaq\_enc.htm](http://www.opentag.com/xfaq_enc.htm)

3. Linux Unicode 编程：[http:\/\/www.ibm.com\/developerworks\/cn\/linux\/i18n\/unicode\/linuni\/](http://www.ibm.com/developerworks/cn/linux/i18n/unicode/linuni/)

[http:\/\/bugs.sun.com\/bugdatabase\/view\_bug.do?bug\_id=4508058](http://bugs.sun.com/bugdatabase/view_bug.do?bug_id=4508058)

[http:\/\/koti.mbnet.fi\/akini\/java\/java\_utf8\_xml\/](http://koti.mbnet.fi/akini/java/java_utf8_xml/)

[http:\/\/koti.mbnet.fi\/akini\/java\/unicodereader\/](http://koti.mbnet.fi/akini/java/unicodereader/)

[http:\/\/www.unicode.org\/faq\/utf\_bom.html](http://www.unicode.org/faq/utf_bom.html)

