Getting and Cleanning Data 获取并整理数据
==========================================================================
## 一、课程介绍
这门课的内容就是用R做ETL，处理原始数据（raw data)，得到干净的数据集(tidy data)，用来做后续的数据分析。  
ETL的过程涉及到四样东西：  
1. raw data  
2. a tidy data set  
3. code book:用来详细介绍tidy data set  
4. recipe:从1得到2和3的详细和准确的过程的记录和描述  

其中raw data可能是从仪器获取的二进制的文件，或者有好多个工作簿的excel文件，或者从api得到的json数据，甚至是看完显微镜之后手工录入的一些数字，总之，raw data指的是未经过处理的、原始的、第一手的数据。

对tidy data的要求：
1. 一个属性为一个列
2. 每个记录为一行
3. there should be one table for each "kind" of variable---？
4. 不同表之间有可以链接的外键。
tips：
1. 数据集的的第一行应该是属性的名称
2. 属性的名称应该是易读的，例如AgeAtDiagnosis就比AgeDx来的好
3. 通常，每个表存成一个单独的文件。

什么是code book?
1. code book是一个word或txt文件。
2. 要一个部分叫Study design，介绍怎么处理数据。
3. 还要有一个部分叫code book，描述每个属性和units。

recipe(或instruction list)，最好是一个R脚本，Python脚本也可以。脚本的输入为raw data，输出即为tidy data。
当然也可以是步骤的描述，第一步做什么，第二步做什么。。


## 二、R的文件系统操作命令  
### 1. 设置工作目录 working directory
    getwd()  
    setwd("C:/Documents and Settings/Administrator/My Documents/data") -- 使用绝对路径  
    setwd("./data") -- 使用相对路径（./data当前路径下的data目录  ../上级目录）  
### 2. 判断文件或目录是否存在  
    file.exists("best.R")   
### 3. dir()，同list.files()，打印出工作目录下的所有文件（含目录）  
        区别于ls()，后者是打印出当前工作环境下用户使用的数据集和函数。  
### 4. dir.create("data") 创建data文件夹。  
        创建文件是file.create，必须在有权限的目录下才能创建文件，通过Sys.chmod(paths, mode = "0777", use_umask = TRUE)设置目录权限。  
 
## 三、getting data 获取数据---正式进入这门课。  
### 1. 网上下载文件download.file()  
        url <- "http://data.baltimorecity.gov/api/views/dz54-2aru/rows.csv?accessType=DOWNLOAD"          
        download.file(url, destfile="./cameras.csv")  
在XP系统，R(32bit)的环境下使用https的url会提示"unsupported URL scheme"，改成http即可  
在mac下下载文件可使用参数method=curl  

## 四、reading data 读取本地文件
### 1. read.table()  
参数有很多，常用的除了file(文件名）外，还有  
        * header=TRUE/FALSE是否含标题  
        * sep=","  用“，”分隔，特别适用于.csv文件  
        * skip=n跳过n行  
        * nrows=n,读取n行  
        * quote="" 不用引号  
        * row.names，col.names
### 2. read.csv  
read.table() with sep="," ，header=TRUE  
### 3. read.xlsx(), read.xlsx2()  
需要安装xlsx包  install.packages(xlsx,type="source")  
重要参数：sheetIndex, colIndex, rowIndex  

## 五、读取XML文件--做网页解析  
请先自行学习一下xml文件的标签、元素、属性等基础知识。需加载XML包  
### 1. 解析xml文件 

```r
        library(XML)
        fileUrl <- "http://www.w3schools.com/xml/simple.xml"
        doc <- xmlTreeParse(fileUrl,useInternal=TRUE)
```
   解析html文件htmlTreeParse()
### 2. 读取根节点 

```r
        rootNode <- xmlRoot(doc)
```
### 3. 读取节点名称（标签），读取该节点的子节点名称

```r
        xmlName(rootNode)
```

```
## [1] "breakfast_menu"
```

```r
        subNodesNames <- names(rootNode)
        subNodesNames
```

```
##   food   food   food   food   food 
## "food" "food" "food" "food" "food"
```

```r
        subNodesNames[1]
```

```
##   food 
## "food"
```

```r
        subNodesNames[[1]]
```

```
## [1] "food"
```

### 4. 读取子节点：

```r
        rootNode[1]
```

```
## $food
## <food>
##   <name>Belgian Waffles</name>
##   <price>$5.95</price>
##   <description>Two of our famous Belgian Waffles with plenty of real maple syrup</description>
##   <calories>650</calories>
## </food> 
## 
## attr(,"class")
## [1] "XMLInternalNodeList" "XMLNodeList"
```

```r
        rootNode[[1]]
```

```
## <food>
##   <name>Belgian Waffles</name>
##   <price>$5.95</price>
##   <description>Two of our famous Belgian Waffles with plenty of real maple syrup</description>
##   <calories>650</calories>
## </food>
```

```r
        rootNode[[1]][[1]]
```

```
## <name>Belgian Waffles</name>
```
### 5. xmlSApply()：根据第二个参数要求，读取当前节点的所有子节点信息。

```r
        xmlSApply(rootNode,xmlValue)
```

```
##                                                                                                                     food 
##                               "Belgian Waffles$5.95Two of our famous Belgian Waffles with plenty of real maple syrup650" 
##                                                                                                                     food 
##                    "Strawberry Belgian Waffles$7.95Light Belgian waffles covered with strawberries and whipped cream900" 
##                                                                                                                     food 
## "Berry-Berry Belgian Waffles$8.95Light Belgian waffles covered with an assortment of fresh berries and whipped cream900" 
##                                                                                                                     food 
##                                                "French Toast$4.50Thick slices made from our homemade sourdough bread600" 
##                                                                                                                     food 
##                         "Homestyle Breakfast$6.95Two eggs, bacon or sausage, toast, and our ever-popular hash browns950"
```

```r
        xmlSApply(rootNode,xmlName)
```

```
##   food   food   food   food   food 
## "food" "food" "food" "food" "food"
```

```r
        xmlSApply(rootNode,xmlAttrs)
```

```
## $food
## NULL
## 
## $food
## NULL
## 
## $food
## NULL
## 
## $food
## NULL
## 
## $food
## NULL
```

```r
        xmlSApply(rootNode,xmlSize)
```

```
## food food food food food 
##    4    4    4    4    4
```
### 6. xpathSApply()：按条件解析xml文件
        * / node Tope level node
        * // node Node at any level
        * node[@attr-name] node with an attribute name
        * node[@attr-name='bob'] node with attribute name attr-name and its value 'bob'
#### 示例1：列出所有level的指定标签的元素值

```r
xpathSApply(rootNode,"//name",xmlValue)
```

```
## [1] "Belgian Waffles"             "Strawberry Belgian Waffles" 
## [3] "Berry-Berry Belgian Waffles" "French Toast"               
## [5] "Homestyle Breakfast"
```

#### 示例2：抓取并解析网页，得到所有的队名，以及它们的得分。
        scores <- xpathSApply(doc,"//li[@class='score']",xmlValue)
        teams <- xpathSApply(doc,"//li[@class='team-name']",xmlValue)

## 六、读JSON格式数据--- jsonlite包
### 1. Json数据格式
        * numbers(double)  
        * Strings(double quoted)
        * Boolean(true or false)
        * Array(ordered,comma separated enclosed in square brackets[ ])
        * Object(unordered,comma seperated collection of key: value pairs in curley brackets{ })

### 2. 示例

```r
library(jsonlite)
```

```
## Warning: package 'jsonlite' was built under R version 3.0.3
```

```r
jsonData <- fromJSON("https://api.github.com/users/jtleek/repos")  #从网络上读JSON数据
jsonData$id  #此JSON数据中所有key为id的value
```

```
##  [1] 12441219 20234724  7751816  4772877 14204342 11549405 14240696
##  [8] 11976743  8730097 14590772 12563551  6582536  6661008 19133476
## [15] 16584923  7745123 15639612 19133794 17446438 15723485 11378145
## [22] 17711648 20548045 16103392 12134722 13788992 15532926 12931390
## [29] 20508464 20369329
```

```r
names(jsonData) #列出此JSON数据的所有key
```

```
##  [1] "id"                "name"              "full_name"        
##  [4] "owner"             "private"           "html_url"         
##  [7] "description"       "fork"              "url"              
## [10] "forks_url"         "keys_url"          "collaborators_url"
## [13] "teams_url"         "hooks_url"         "issue_events_url" 
## [16] "events_url"        "assignees_url"     "branches_url"     
## [19] "tags_url"          "blobs_url"         "git_tags_url"     
## [22] "git_refs_url"      "trees_url"         "statuses_url"     
## [25] "languages_url"     "stargazers_url"    "contributors_url" 
## [28] "subscribers_url"   "subscription_url"  "commits_url"      
## [31] "git_commits_url"   "comments_url"      "issue_comment_url"
## [34] "contents_url"      "compare_url"       "merges_url"       
## [37] "archive_url"       "downloads_url"     "issues_url"       
## [40] "pulls_url"         "milestones_url"    "notifications_url"
## [43] "labels_url"        "releases_url"      "created_at"       
## [46] "updated_at"        "pushed_at"         "git_url"          
## [49] "ssh_url"           "clone_url"         "svn_url"          
## [52] "homepage"          "size"              "stargazers_count" 
## [55] "watchers_count"    "language"          "has_issues"       
## [58] "has_downloads"     "has_wiki"          "forks_count"      
## [61] "mirror_url"        "open_issues_count" "forks"            
## [64] "open_issues"       "watchers"          "default_branch"
```

```r
names(jsonData$owner) #列出JSON数据的owner对象的所有key
```

```
##  [1] "login"               "id"                  "avatar_url"         
##  [4] "gravatar_id"         "url"                 "html_url"           
##  [7] "followers_url"       "following_url"       "gists_url"          
## [10] "starred_url"         "subscriptions_url"   "organizations_url"  
## [13] "repos_url"           "events_url"          "received_events_url"
## [16] "type"                "site_admin"
```

```r
jsonData$owner$login  #列出JSON中owner对象的值（一个object)里key为login的值
```

```
##  [1] "jtleek" "jtleek" "jtleek" "jtleek" "jtleek" "jtleek" "jtleek"
##  [8] "jtleek" "jtleek" "jtleek" "jtleek" "jtleek" "jtleek" "jtleek"
## [15] "jtleek" "jtleek" "jtleek" "jtleek" "jtleek" "jtleek" "jtleek"
## [22] "jtleek" "jtleek" "jtleek" "jtleek" "jtleek" "jtleek" "jtleek"
## [29] "jtleek" "jtleek"
```

### 3. write data frames to JSON

```r
myjson <- toJSON(head(iris,3),pretty=TRUE)     #取iris集的前三行
cat(myjson)      # cat（）按格式打印json数据
```

```
## [
## 	{
## 		"Sepal.Length" : 5.1,
## 		"Sepal.Width" : 3.5,
## 		"Petal.Length" : 1.4,
## 		"Petal.Width" : 0.2,
## 		"Species" : "setosa"
## 	},
## 	{
## 		"Sepal.Length" : 4.9,
## 		"Sepal.Width" : 3,
## 		"Petal.Length" : 1.4,
## 		"Petal.Width" : 0.2,
## 		"Species" : "setosa"
## 	},
## 	{
## 		"Sepal.Length" : 4.7,
## 		"Sepal.Width" : 3.2,
## 		"Petal.Length" : 1.3,
## 		"Petal.Width" : 0.2,
## 		"Species" : "setosa"
## 	}
## ]
```

```r
iris2 <- fromJSON(myjson) #从JSON对象再到data frame
```

## 七、data.table---data.table包
### 1. data.table继承自data.frame  
### 2. 创建data.table

```r
library(data.table)
```

```
## Warning: package 'data.table' was built under R version 3.0.3
```

```r
dt <- data.table(x=rnorm(9),y=rep(c("a","b","c"),each=3),z=rnorm(9))
dt
```

```
##            x y       z
## 1:  0.004951 a  0.2729
## 2:  0.939096 a -1.3290
## 3: -0.406893 a -0.9142
## 4:  0.384260 b -0.8594
## 5:  0.298068 b  0.5499
## 6: -2.036220 b -0.6992
## 7: -0.224536 c  0.0510
## 8: -0.091570 c  0.5814
## 9: -1.762561 c  1.3433
```

```r
tables()
```

```
##      NAME NROW MB COLS  KEY
## [1,] dt      9 1  x,y,z    
## Total: 1MB
```
### 3. data.table的运算
#### * 行计算：

```r
dt[2,]  #取第二行
```

```
##         x y      z
## 1: 0.9391 a -1.329
```

```r
dt[dt$y=="a",]  #取y="a"的所有行
```

```
##            x y       z
## 1:  0.004951 a  0.2729
## 2:  0.939096 a -1.3290
## 3: -0.406893 a -0.9142
```
#### * 列计算：

```r
dt[,list(mean(x),sum(z))]  #将需要对data.table的列进行计算的函数放在逗号后面，可以用list做成一个函数列表，【】里的variable不需要用$
```

```
##         V1     V2
## 1: -0.3217 -1.003
```

```r
dt[,w:=z^2]   #增加新列，注意：=符号
```

```
##            x y       z        w
## 1:  0.004951 a  0.2729 0.074470
## 2:  0.939096 a -1.3290 1.766239
## 3: -0.406893 a -0.9142 0.835733
## 4:  0.384260 b -0.8594 0.738618
## 5:  0.298068 b  0.5499 0.302403
## 6: -2.036220 b -0.6992 0.488867
## 7: -0.224536 c  0.0510 0.002602
## 8: -0.091570 c  0.5814 0.338055
## 9: -1.762561 c  1.3433 1.804515
```

```r
dt2<-dt     
dt[,y:=2.0]   #改变dt时，dt2也同时改变
```

```
## Warning: Coerced 'double' RHS to 'character' to match the column's type;
## may have truncated precision. Either change the target column to 'double'
## first (by creating a new 'double' vector length 9 (nrows of entire table)
## and assign that; i.e. 'replace' column), or coerce RHS to 'character'
## (e.g. 1L, NA_[real|integer]_, as.*, etc) to make your intent clear and for
## speed. Or, set the column type correctly up front when you create the
## table and stick to it, please.
```

```
##            x y       z        w
## 1:  0.004951 2  0.2729 0.074470
## 2:  0.939096 2 -1.3290 1.766239
## 3: -0.406893 2 -0.9142 0.835733
## 4:  0.384260 2 -0.8594 0.738618
## 5:  0.298068 2  0.5499 0.302403
## 6: -2.036220 2 -0.6992 0.488867
## 7: -0.224536 2  0.0510 0.002602
## 8: -0.091570 2  0.5814 0.338055
## 9: -1.762561 2  1.3433 1.804515
```

```r
dt3 <- copy(dt)   
dt[,y:=3.0]  #使用copy()后，是完全新建了一个data.table，dt改变,dt3不会跟着改变
```

```
## Warning: Coerced 'double' RHS to 'character' to match the column's type;
## may have truncated precision. Either change the target column to 'double'
## first (by creating a new 'double' vector length 9 (nrows of entire table)
## and assign that; i.e. 'replace' column), or coerce RHS to 'character'
## (e.g. 1L, NA_[real|integer]_, as.*, etc) to make your intent clear and for
## speed. Or, set the column type correctly up front when you create the
## table and stick to it, please.
```

```
##            x y       z        w
## 1:  0.004951 3  0.2729 0.074470
## 2:  0.939096 3 -1.3290 1.766239
## 3: -0.406893 3 -0.9142 0.835733
## 4:  0.384260 3 -0.8594 0.738618
## 5:  0.298068 3  0.5499 0.302403
## 6: -2.036220 3 -0.6992 0.488867
## 7: -0.224536 3  0.0510 0.002602
## 8: -0.091570 3  0.5814 0.338055
## 9: -1.762561 3  1.3433 1.804515
```

```r
dt[,m:={tmp<-(x+z);log2(tmp+5)}]   #可以将多个步骤写在一个{}里，用；分隔
```

```
##            x y       z        w     m
## 1:  0.004951 3  0.2729 0.074470 2.400
## 2:  0.939096 3 -1.3290 1.766239 2.205
## 3: -0.406893 3 -0.9142 0.835733 1.879
## 4:  0.384260 3 -0.8594 0.738618 2.178
## 5:  0.298068 3  0.5499 0.302403 2.548
## 6: -2.036220 3 -0.6992 0.488867 1.179
## 7: -0.224536 3  0.0510 0.002602 2.271
## 8: -0.091570 3  0.5814 0.338055 2.457
## 9: -1.762561 3  1.3433 1.804515 2.196
```

```r
dt[,a:=x>0]   #给dt增加新列a，值为判断x>0的逻辑值
```

```
##            x y       z        w     m     a
## 1:  0.004951 3  0.2729 0.074470 2.400  TRUE
## 2:  0.939096 3 -1.3290 1.766239 2.205  TRUE
## 3: -0.406893 3 -0.9142 0.835733 1.879 FALSE
## 4:  0.384260 3 -0.8594 0.738618 2.178  TRUE
## 5:  0.298068 3  0.5499 0.302403 2.548  TRUE
## 6: -2.036220 3 -0.6992 0.488867 1.179 FALSE
## 7: -0.224536 3  0.0510 0.002602 2.271 FALSE
## 8: -0.091570 3  0.5814 0.338055 2.457 FALSE
## 9: -1.762561 3  1.3433 1.804515 2.196 FALSE
```

```r
dt[,b:=mean(x+w),by=a]   #plyr风格的计算，根据a的值分组，计算mean(x+w)
```

```
##            x y       z        w     m     a       b
## 1:  0.004951 3  0.2729 0.074470 2.400  TRUE  1.1270
## 2:  0.939096 3 -1.3290 1.766239 2.205  TRUE  1.1270
## 3: -0.406893 3 -0.9142 0.835733 1.879 FALSE -0.2104
## 4:  0.384260 3 -0.8594 0.738618 2.178  TRUE  1.1270
## 5:  0.298068 3  0.5499 0.302403 2.548  TRUE  1.1270
## 6: -2.036220 3 -0.6992 0.488867 1.179 FALSE -0.2104
## 7: -0.224536 3  0.0510 0.002602 2.271 FALSE -0.2104
## 8: -0.091570 3  0.5814 0.338055 2.457 FALSE -0.2104
## 9: -1.762561 3  1.3433 1.804515 2.196 FALSE -0.2104
```
### 4. 特殊变量 .N an integer,length 1, containing the number r
#### 示例1：分组统计

```r
set.seed(123) 
dt <- data.table(x=sample(letters[1:3],1e5,TRUE))
dt
```

```
##        x
## 1e+00: a
## 2e+00: c
## 3e+00: b
## 4e+00: c
## 5e+00: c
##    ---  
## 1e+05: c
## 1e+05: b
## 1e+05: a
## 1e+05: c
## 1e+05: a
```
方法一： 

```r
dt[,.N,by=x]
```

```
##    x     N
## 1: a 33387
## 2: c 33201
## 3: b 33412
```
方法二：使用plyr包里的ddply()  

```r
library(plyr)
```

```
## Warning: package 'plyr' was built under R version 3.0.3
```

```r
ddply(dt,.(x),summarize,count=length(x))
```

```
##   x count
## 1 a 33387
## 2 b 33412
## 3 c 33201
```
这个方法显然还可以做更多的事，比如最后一个参数的方法改成sum()，emission.type.year <- ddply(Baltimore, .(type, year), summarise, TotalEmissions = sum(Emissions))，用来对type和year分组，计算sum（Emissions)  
方法三：使用table()  

```r
table(dt)
```

```
## dt
##     a     b     c 
## 33387 33412 33201
```

#### 示例2：
The American Community Survey distributes downloadable data about United States communities. Download the 2006 microdata survey about housing for the state of Idaho using download.file() from here: 
https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2Fss06hid.csv 
and load the data into R. The code book, describing the variable names is here: 
https://d396qusza40orc.cloudfront.net/getdata%2Fdata%2FPUMSDataDict06.pdf 
How many housing units in this survey were worth more than $1,000,000?

方法一：
        house <- read.csv("getdata-data-ss06hid.csv")  
        dt <- data.table(house)  
        dt[,.N,by=VAL]  

方法二：
        house <- read.csv("getdata-data-ss06hid.csv")  
        tables(house$VAL)  

### 5. 设置key

```r
DT <- data.table(x=rep(c("a","b","c"),each=100),y=rnorm(300))
setkey(DT,x)  #设置x列为key
head(DT['a'],5)   #取键值为a的前5行
```

```
##    x       y
## 1: a  0.2596
## 2: a  0.9175
## 3: a -0.7223
## 4: a -0.8083
## 5: a -0.1414
```
应用：对设置过相同key的两个data.table做合并

```r
DT1 <- data.table(x=c('a','a','b','dt1'),y=1:4)
DT2 <- data.table(x=c('a','b','dt2'),z=5:7)
setkey(DT1,x)
setkey(DT2,x)
merge(DT1,DT2) 
```

```
##    x y z
## 1: a 1 5
## 2: a 2 5
## 3: b 3 6
```
### 6. 比read.table更快速读入文件的方法：fread(fileName)





