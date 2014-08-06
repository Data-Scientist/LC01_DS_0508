
#Reproducible Research

----------
引用Robot同学的一段[总结](http://mooc.guokr.com/post/606482/)作为介绍

I fully agree this is really very very important, but not sure does it deserve to be one seperate course(might be as I know markdown, knitr already). Some video was taken from other lectures, so occassionally I feel the whole course was repeat the same thing in some lectures. However overall this course was quite good, I learned a lot.

这门课最核心的地方就是写Rmarkdown，介绍了一些流程，然后强调了一些概念和规范，对于有些同学可能很基础。Whatever,enjoy it!

##Week 1 
###概念
The term reproducible research refers to the idea that the ultimate product of academic research is the paper along with the full computational environment used to produce the results in the paper such as the code, data, etc. that can be used to reproduce the results and create new work based on the research.（wikipedia）

###重要性
 1. 可重复性研究的主要目的是方便交流
 2. 节省时间，提高效率

###研究流程
可重复性研究的大致流程[点击看图](http://pan.baidu.com/s/1eQ1RGqu)

###工具
本课程主要用knitr包

###数据分析步骤
1. Define the question
2. Define the ideal data set
3. Determine what data you can access
4. Obtain the data
5. Clean the data
6. Exploratory data analysis
7. Statistical prediction/modeling
8. Interpret results
9. Challenge results
10. Synthesize/write up results
11. Create reproducible code

###数据分析的文件
1. 数据
2. 图片
3. 代码
4. 文本

###补充
* 问题（暂无）
* 经验（暂无）


##Week 2
###R代码标准
1. 用文本编辑器
2. 缩进代码
3. 限制代码宽度
4. 限制单个函数的长度

###Markdown
Markdown 是一种轻量级标记语言，创始人为 John Gruber 和 Aaron Swartz。它允许人们“使用易读易写的纯文本格式编写文档，然后转换成有效的 XHTML/HTML 文档。
[Markdown语法快速入门](http://www.oschina.net/question/100267_75314)

###RMarkdown
R markdown是R语言运行环境RStudio所使用的扩展的markdown标签语言，能够方便的利用R语言生成web格式的报告。它包括核心的Markdown语法，并能将其中插入的R代码区块的运行结果显示在最终文档里。
[RMarkdown](http://jianshu.io/p/gwTSKt)

###Knitr
knitr是R语言中一个用来动态生成报告的包，用户可以在报告中嵌入数据分析的源代码，通过knitr编译直接生成一份报告，而无需复制粘贴结果，所有结果由knitr执行源代码动态生成。knitr可以结合LaTeX、LyX、HTML、Markdown以及reStructuredText文档使用。它的设计范式源于文学编程，目的是促进可重复的科学研究。(Wikipedia)

###补充
* 问题（暂无）
* 经验（暂无）


##Week 3
###交流结果
1. 研究论文结构
  * Title / Author list
  * Abstract
  * Body / Results
  * Supplementary Materials / the gory details
  * Code / Data / really gory details
2. 邮件结构
  * Subject line / Sender info
  * Email body
  * Attachment(s)
  * Links to Supplementary Materials

###检查清单
1. DO
  * Start With Good Science
  * Teach a Computer
  * Use Some Version Control
  * Keep Track of Your Software Environment
  * Set Your Seed
  * Think About the Entire Pipeline
2. DON'T
  * Do Things By Hand
  * Point And Click
  * Save Output

###Evidence-based Data Analysis（似乎没看到翻成循证的...）
1. What we get
  * Transparency
  * Data Availability
  * Software / Methods Availability
  * Improved Transfer of Knowledge
2. What we do NOT get
  * Validity / Correctness of the analysis

###补充
问题

 * 其实对这个课程理解很浅，毕竟隔行，太多东西不懂，希望补充

经验（暂无）


##Week 4
Case study
