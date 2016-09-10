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



