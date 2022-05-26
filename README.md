# TSLA 2019~2022年股票数据可视化
数据可视化期末作业，使用JavaScript编写，目的是练习使用d3进行数据的可视化。数据集由老师提供。上传GitHub以作记录。
## 可视化设计
经过了解，目前主要的股票可视化有K线图（虽然不炒股完全不知道K线是什么）和股票交易量。\
\
K线图的由大阴/阳线和阴/阳线实体构成。大线的长度为当日最高价与最低价的差值，实体的长度为当日开盘价与收盘价的差值；如果当日开盘价低于收盘价（股价上涨），大线和实体会显示为红色，反之则为绿色。（国外好像是反过来？）\
\
交易量一般用柱状图表示。\
\
老师上课时主要教授的是rect、circle以及path在svg上的绘制，本次计划使用rect来实现K线图与交易量的绘制。具体的实现步骤为：\
\
K线图使用交大的rect绘制实体，并根据开盘价（Open）与收盘价（Close）的比较来显示红/绿色，rect的高为两者（映射的）差值。\
\
交易量用连续的矩形来绘制柱状图，并且依据开盘价（Open）与收盘价（Close）的比较来显示红/绿色（虽然我觉得不是这个逻辑但是也不打算深究了）。\
\
K线图与交易量图由不同的svg画布绘制，两者rect的宽度属性都依据rect的总数进行动态计算，防止有数据在画布显示范围外。\
\
图表最大的显示范围是年，也可以单独显示某一个月的数据。年月范围的可以通过图标上方的单选框调节。
## 最终效果图
![效果截图](https://user-images.githubusercontent.com/95087553/170284615-0f028fe7-6b3e-40c0-9bb1-b00317ed13fb.png)
## 遇到的问题
### 1、数据的读取
之前老师授课的时候都是使用.json格式的文件，而本次作业老师提供的是.csv文件。两者在读取上就只是d3.json()和d3.csv()的区别，但是csv格式在读取之后在浏览器控制台上console出来的Array会多出来一个columns属性，再加上我又是因为可视化现学的JS，就很慌，以为他俩不能用一样的方法处理。\
\
在程序编写的前期（其实是大半部分）我花费了大量的时间来研究d3读取json文件和csv文件后的差异，期间作出的尝试包括大不限于依照（想象中的）csv格式重新编写数据筛选方法（虽然最后还是自己写了）、试图在JS代码中将读取的csv格式转换成json格式、以及在.html文件（作业代码）外使用Python将csv格式转变为json格式后再进行读取（关键是代码写到一半发现网上有现成的格式转换器我真是日了狗了）等。然后根据大量的试验确定了除了csv格式的文件悲d3读取后Array会带有一个columns属性之外其他都一样，可以一样处理（两天努力全部木大）。
### 2、数据的筛选
由于可以通过网页单选框选择图表显示范围，所以必须把原始数据依照单选框的选择情况进行筛选。原来老师在授课的过程中使用的是lodash.filter()和lodash.includes()两个方法来进行筛选，原本我是想照猫画虎魔改一下老师教的代码的，然后我就发现了一个问题：原始数据中的日期是以“YY-MM-DD”的方式记录，要将其进行年月筛选，就必须将记录中的Date字符串取出来，以“-”进行分割再比较。而直接写成&nbsp;lodash.split(rawData.Date,"-")[0]&nbsp;这样的格式是不行的，必须要精确到某一条记录。这就意味着我得将原始数据整个遍历一遍进行比较才行。\
\
然后我又花了几个小时研究怎么将便利后的布尔值传回给lodash.filter()，最后还是依照自己的理解用三层for循环自己写了个筛选方法。
### 3、比例尺
因为特斯拉这两年的股价上至一千五，下至四十八，我只有300高的svg画布肯定顶不住这折腾，所以肯定要创建一个比例尺。最初我是将High、Low、Open、Close四个属性分别建立比例尺，但是在计算rect高度的时候就报错一些的rect的height为负。我想是因为两个映射定义域不一样所以可能会导致明明原来值比较低的最后映射出来的值反而会比较高，后来我尝试输入固定值或者计算出开/收盘价的最大最小值做定义域，但是画的矩形老是飞出画布。\
\
最后是老师机械降神帮我修改了代码才使得比例尺创建成功。原理目前不是很懂，贴张图先：
![image](https://user-images.githubusercontent.com/95087553/170301294-1ca4d284-530b-4668-9224-598be87b6114.png) \
话说我通过这次经历才知道Math.max()不能直接传一个数组进去……
## 总结
首先感谢7ki7ki酱酱陪我度过四个难忘的夜晚！ \
\
然后就是打算造轮子之前一定要了解有没有现成工具！变成前/时逛论坛和网上冲浪真的很重要！\
\
编程好玩是真的好玩，人菜也是真的菜！吾日三省吾身我是菜狗\
\
最后祝我身体健康！再见！
