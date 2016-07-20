所谓领域专用语言（domain specific language \/ DSL），其基本思想是“求专不求全”，不像通用目的语言那样目标范围涵盖一切软件问题，而是专门针对某一特定问题的计算机语言。几乎自计算机发明伊始，人们就开始谈论DSL使用DSL了。

Unix社群是一个频繁使用DSL的社群，他们通常称之为小语言或迷你语言。（关于这一传统，Eric Raymond的《[Unix编程艺术](http://www.dearbook.com.cn/subject/unixcoding/index.htm)》有上佳[探讨](http://www.faqs.org/docs/artu/minilanguageschapter.html)。）要构建一种DSL，按最常见的Unix风格的做法，就是先定义它的语法，然后通过代码生成技术把DSL代码转成一种通用语言代码，或者写一个这种DSL的解释器。Unix有很多工具能让这件事做起来轻松些。我为这类DSL定了一个术语：“**外部DSL**”。XML配置文件是外部DSL的另一种常见形式。

DSL也是Lisp和Smalltalk社群的一项重要传统，但方式不同，他们不是动手新造一套语言，而是让Lisp或Smalltalk这种通用目的语言换个颜面变成DSL。（Paul Graham的文章《[自底向上编程](http://www.paulgraham.com/progbot.html)》对此有精彩讲述。）利用编程语言自带的语法结构定义出来的DSL，我称之为“**内部DSL**”，也叫做“**内嵌DSL**”。这是种通用策略，不仅适用于Lisp和Smalltalk，用任何语言都可以这么做，我面对问题时总是考虑着用这种策略定义出具有DSL功能的东西来解决，不过Lisp和Smalltalk程序员走得要深远得多。

关于这两类DSL，在我最近的文章《[语言工作台（Language Workbench）](http://martinfowler.com/articles/languageWorkbench.html)》中有进一步的例子，我希望DSL能使用得更普遍，文中详细讨论了这两种风格各自的优缺点，还介绍了语言工作台工具的最新进展。

直到出现了一位人物，原本内外分流的DSL走向了一个有趣的汇合，他就是[PragDave](http://pragprog.com/pragdave)。沿袭Unix的传统，用本主义程序员们（The pragmatic programmers）老早就是DSL粉丝了（《[用本主义程序员](http://www.amazon.com/exec/obidos/tg/detail/-/020161622X)》（中译本[链接](http://www.dearbook.com.cn/book/21069)）第十二节对这个话题的讨论引人入胜——我干脆把它称作“用本要义12”好了）。Dave在一次富有见地的访谈里说到，尽管代码生成是他的惯用技术，但在用Ruby编程时很少用到。

我做设计时，经常借构建一套DSL的思路来类推——有意把class和方法设计成DSL的样子。不论用什么语言，我都尽量这么做，如果做不到，我就乐得转用代码生成技术了。在我们ThoughtWorks公司的较大型系统上，代码生成以及类似的技术使用得非常普遍。

什么时候需要把DSL和主语言划清界线，我认为这个问题的答案因主语言而异，用Smalltalk时我几乎从没感觉有必要分离出一种DSL来，而这种需求在用C++\/Java\/C\#时则非常常见。

因 此，我认为有的语言适合设计内部DSL，有的不适合。适合的是那种“一条道跑到黑”的风格简约的语言，它们在某一方面比其他传统语言走得更远更纯粹（例如 Lisp的函数式风格，Smalltalk的“对象-消息”思想），这是我分析Lisp和Smalltalk得出的结论。再看Ruby，它比前两者更常规 化一些，也比它们都庞大，但仍不失为一门用来构建内部DSL的好语言。

这么看来，语言设计者需要对语言的精练程度有一个良好的决策，既要 保证常规性内容可以轻松地表达，又要为原本费神的复杂东西提供舒适的语法支持。总之，我认为这是非常重要的一点。我喜欢用Smalltalk和Ruby的 程度比喜欢用Java或C\#的程度高那么多，其原因我总是觉得难以言传，最常听到的解释是静态类型与动态类型的区别所致，但我总觉得这个说法并没有抓住要 害，更接近两者区别本质的是它们对构建内部DSL友好程度的差异。 

